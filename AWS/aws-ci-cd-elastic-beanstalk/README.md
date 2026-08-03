# AWS CI/CD Pipeline using CodePipeline, CodeBuild & Elastic Beanstalk

## Project Overview

This project demonstrates a complete CI/CD pipeline using AWS services. Source code is stored in Bitbucket, automatically built using AWS CodeBuild, orchestrated by AWS CodePipeline, and deployed to AWS Elastic Beanstalk.

---

## Architecture

![Architecture](architecture.png)

---

## AWS Services Used

- AWS CodePipeline
- AWS CodeBuild
- AWS Elastic Beanstalk
- AWS IAM
- Amazon S3
- CloudWatch

---

## CI/CD Workflow

1. Developer pushes code to Bitbucket.
2. AWS CodePipeline detects the code changes.
3. AWS CodeBuild compiles and packages the application using Maven.
4. The generated artifact is passed to AWS Elastic Beanstalk.
5. Elastic Beanstalk deploys the latest version of the application.

---

## Repository Structure

```
aws-cicd-elastic-beanstalk/
│
├── buildspec.yml
├── README.md
├── .gitignore
└── architecture.png
```

---

## Build Process

The build process is managed by **AWS CodeBuild** using the `buildspec.yml` file.

Build steps include:

- Install Java runtime
- Execute Maven build
- Package the application
- Generate deployment artifact

---

## Deployment

Deployment is fully automated through **AWS CodePipeline**, which sends the build artifact to **AWS Elastic Beanstalk** after a successful build.

---

## Outcome

- Automated CI/CD pipeline
- Zero manual deployments
- Continuous integration
- Continuous delivery
- Production-ready deployment workflow
