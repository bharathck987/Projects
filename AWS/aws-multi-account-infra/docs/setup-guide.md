# Setup & Deployment Guide

## Prerequisites

- AWS CLI v2 configured with admin access to your AWS Organization's management account
- 3 AWS accounts created (or invited) under one AWS Organization: `dev`, `staging`, `prod`
- `aws-cli` credentials profile configured for each account, e.g.:
  ```
  [dev]
  ... 
  [staging]
  ...
  [prod]
  ...
  ```

## 1. Set up AWS Organizations & SCPs

From the **management account**:

```bash
aws organizations create-organization --feature-set ALL

aws organizations create-policy \
  --name "DenyRootUserActions" \
  --type SERVICE_CONTROL_POLICY \
  --content file://cloudformation/scp-policies.json
```

Attach each policy to the relevant Organizational Unit (OU) or account via the console
or `aws organizations attach-policy`.

## 2. Deploy cross-account IAM roles

Deploy into **each** account (dev/staging/prod), replacing `<TRUSTED_ACCOUNT_ID>` with
your central CI/CD or admin account ID:

```bash
aws cloudformation deploy \
  --profile dev \
  --template-file cloudformation/iam-cross-account-roles.yaml \
  --stack-name cross-account-role \
  --parameter-overrides Environment=dev TrustedAccountId=<TRUSTED_ACCOUNT_ID> ExternalId=<RANDOM_UUID> \
  --capabilities CAPABILITY_NAMED_IAM
```

Repeat with `--profile staging` / `Environment=staging` and `--profile prod` / `Environment=prod`.

## 3. Deploy VPC per account

```bash
aws cloudformation deploy \
  --profile dev \
  --template-file cloudformation/vpc-stack.yaml \
  --stack-name vpc-stack \
  --parameter-overrides Environment=dev VpcCidr=10.0.0.0/16
```

Use a distinct, non-overlapping CIDR per environment (e.g. `10.0.0.0/16`, `10.1.0.0/16`,
`10.2.0.0/16`) so future VPC peering or Transit Gateway attachment is possible.

## 4. Build the custom AMI

```bash
aws cloudformation deploy \
  --profile dev \
  --template-file cloudformation/ami-bootstrap.yaml \
  --stack-name ami-pipeline \
  --parameter-overrides Environment=dev \
  --capabilities CAPABILITY_NAMED_IAM
```

Trigger the pipeline once manually to get your first AMI ID:

```bash
aws imagebuilder start-image-pipeline-execution \
  --image-pipeline-arn <PIPELINE_ARN_FROM_OUTPUT>
```

## 5. Deploy ALB + Auto Scaling Group

```bash
aws cloudformation deploy \
  --profile dev \
  --template-file cloudformation/alb-asg-stack.yaml \
  --stack-name compute-stack \
  --parameter-overrides Environment=dev CustomAmiId=<AMI_ID_FROM_STEP_4>
```

## 6. Verify

```bash
aws cloudformation describe-stacks --profile dev --stack-name compute-stack \
  --query "Stacks[0].Outputs"
```

Hit the returned ALB DNS name in a browser to confirm traffic is reaching the ASG instances.

## Assuming the cross-account role (from CI/CD)

```bash
aws sts assume-role \
  --role-arn arn:aws:iam::<DEV_ACCOUNT_ID>:role/dev-cross-account-deploy-role \
  --role-session-name ci-deploy \
  --external-id <RANDOM_UUID>
```

Use the returned temporary credentials to run the CloudFormation deploy commands above
without ever needing a static IAM user key in the target account.
