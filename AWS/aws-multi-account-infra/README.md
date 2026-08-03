# Secure Multi-Account AWS Infrastructure

Production-style AWS setup spanning three accounts (dev / staging / prod) with strict IAM
boundary enforcement via cross-account roles and Service Control Policies (SCPs). Automates
VPC design, subnetting, load balancing, and Auto Scaling — eliminating manual provisioning
and reducing new-environment spin-up from hours to minutes.

## Architecture

![Architecture Diagram](diagrams/architecture.png)

- **3 AWS Accounts**: `dev`, `staging`, `prod` — isolated under an AWS Organization
- **Management Account** owns the SCPs and cross-account IAM trust policies
- **Cross-account roles** allow controlled access from a central CI/CD or admin account
  into each environment, following least-privilege principles
- **Per-account VPC** with public/private subnets across 2+ AZs
- **ALB + Auto Scaling Group** in each environment for HA compute
- **Custom AMIs** baked with a standardized bootstrap (via Packer or EC2 Image Builder)
  so new instances come up pre-configured

## Why this design

- **SCPs over IAM-only controls**: SCPs enforce guardrails at the AWS Organization level,
  so even an account admin cannot bypass them (e.g., blocking region use outside
  `us-east-1`, preventing root user actions, disallowing public S3 buckets).
- **Cross-account roles over long-lived IAM users**: no static credentials shared across
  environments; access is granted via `sts:AssumeRole` with short-lived credentials.
- **Custom AMIs over user-data bootstrapping**: baked AMIs cut instance boot/config time
  significantly versus installing packages at launch, which is what drove spin-up time
  from hours to minutes.

## Repo structure

```
aws-multi-account-infra/
├── cloudformation/
│   ├── vpc-stack.yaml               # VPC, subnets, route tables, NAT/IGW
│   ├── iam-cross-account-roles.yaml # Cross-account trust + permission roles
│   ├── scp-policies.json            # Org-level Service Control Policies
│   ├── alb-asg-stack.yaml           # ALB, target group, ASG, launch template
│   └── ami-bootstrap.yaml           # EC2 Image Builder pipeline for custom AMIs
├── diagrams/
│   └── architecture.png
├── docs/
│   └── setup-guide.md
└── .gitignore
```

## Deployment

See [docs/setup-guide.md](docs/setup-guide.md) for full step-by-step deployment
instructions across all three accounts.

Quick start (per account):

```bash
aws cloudformation deploy \
  --template-file cloudformation/vpc-stack.yaml \
  --stack-name vpc-stack \
  --parameter-overrides Environment=dev \
  --capabilities CAPABILITY_NAMED_IAM
```

## Stack

AWS Organizations, IAM, SCPs, VPC, EC2, ALB, ASG, S3, CloudFormation, EC2 Image Builder
