# AWS & DevOps Projects Repository

A collection of hands-on AWS, DevOps, Infrastructure as Code (IaC), Kubernetes, and CI/CD projects built to demonstrate real-world cloud engineering and automation practices.

This repository showcases practical implementations using AWS services, CloudFormation, Docker, Kubernetes, GitHub, and CI/CD pipelines.

---

# Repository Structure

```
AWS/
│
├── aws-ci-cd-elastic-beanstalk/
│   ├── README.md
│   ├── architecture/
│   └── buildspec.yaml
│
├── aws-multi-account-infra/
│   ├── README.md
│   ├── cloudformation/
│   │   ├── alb-asg-stack.yaml
│   │   ├── ami-bootstrap.yaml
│   │   ├── iam-cross-account-roles.yaml
│   │   ├── scp-policies.json
│   │   └── vpc-stack.yaml
│   └── docs/
│       └── setup-guide.md
│
├── kubernetes-demo/
│   ├── README.md
│   ├── architecture/
│   ├── k8s-commands.md
│   ├── mongo.yaml
│   ├── mongo-configmap.yaml
│   ├── mongo-secrets.yaml
│   └── mongo-express.yaml
│
├── .gitignore
└── README.md
```

---

# Projects Included

## 1. AWS CI/CD with Elastic Beanstalk

Build a complete Continuous Integration and Continuous Deployment (CI/CD) pipeline using AWS native services.

### Technologies

- AWS CodePipeline
- AWS CodeBuild
- AWS Elastic Beanstalk
- Amazon ECR
- Docker
- GitHub
- IAM
- CloudWatch

### Features

- Automated application deployment
- Source integration with GitHub
- Build automation
- Docker image creation
- Continuous deployment
- Build monitoring through CloudWatch

---

## 2. Secure AWS Multi-Account Infrastructure

A production-style AWS environment following AWS Organizations best practices.

### Technologies

- AWS Organizations
- CloudFormation
- IAM
- Service Control Policies (SCP)
- VPC
- Auto Scaling
- Application Load Balancer
- EC2
- Amazon EBS
- Amazon EFS

### Features

- Separate Development, Staging, and Production accounts
- Cross-account IAM roles
- Service Control Policies
- Highly available VPC design
- Multi-tier architecture
- Auto Scaling
- Secure networking
- Infrastructure as Code using CloudFormation

---

## 3. Kubernetes MongoDB Demo

Deploy MongoDB and Mongo Express inside a Kubernetes cluster.

### Technologies

- Kubernetes
- Minikube
- kubectl
- MongoDB
- Mongo Express
- ConfigMaps
- Secrets

### Features

- MongoDB deployment
- Mongo Express deployment
- Kubernetes Secrets
- ConfigMaps
- Internal service communication
- YAML-based deployment

---

# Skills Demonstrated

- AWS Cloud
- Infrastructure as Code
- CloudFormation
- DevOps
- CI/CD
- Docker
- Kubernetes
- GitHub
- Linux Administration
- IAM
- Networking
- Auto Scaling
- Load Balancing
- Monitoring
- Cloud Security

---

# Prerequisites

- AWS Account
- GitHub Account
- Docker
- Java (for CI/CD project)
- Maven
- kubectl
- Minikube or Kubernetes Cluster
- AWS CLI
- Git

---

# Repository Highlights

- Production-inspired AWS architectures
- Infrastructure as Code
- Secure multi-account AWS setup
- Automated CI/CD pipelines
- Kubernetes application deployment
- Reusable CloudFormation templates
- Well-organized project documentation

---

# Documentation

Each project contains its own documentation:

| Project | Documentation |
|---------|---------------|
| AWS CI/CD | `aws-ci-cd-elastic-beanstalk/README.md` |
| Multi-Account Infrastructure | `aws-multi-account-infra/README.md` |
| Kubernetes Demo | `kubernetes-demo/README.md` |

Additional setup instructions are available in:

```
aws-multi-account-infra/docs/setup-guide.md
```

---

# Future Enhancements

- Terraform implementation
- Amazon EKS deployment
- Amazon ECS deployment
- Jenkins CI/CD pipeline
- GitHub Actions
- AWS CodeDeploy
- Monitoring with Prometheus & Grafana
- Security scanning
- Blue/Green deployment
- ArgoCD GitOps

---

# Author

**Bharath K**

**DevOps Engineer | AWS Cloud | Kubernetes | Docker | Linux | Cloud Automation**

---

# License

This repository is intended for learning, demonstration, and portfolio purposes.
