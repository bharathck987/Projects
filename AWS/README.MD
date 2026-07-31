AWS CI/CD Pipeline for Cloud-Hosted Web Application


📌 Overview

#-overview

An automated CI/CD pipeline built entirely on AWS-native services to deploy a Java-based web application to Elastic Beanstalk, backed by an Amazon RDS (MySQL) database. Every commit to the source repository triggers an automatic build and rolling deployment — no manual intervention required.



class ProjectSummary:
    goal        = "Automate build → deploy → run for a Java web app on AWS"
    trigger     = "git push to Bitbucket (aws-ci branch)"
    flow        = "Bitbucket → CodePipeline → CodeBuild → Elastic Beanstalk → RDS"
    deployment  = "Rolling, zero-downtime, multi-AZ"
    outcome     = "Fully automated, production-style CI/CD workflow"



🏗️ Architecture

#️-architecture

Bitbucket (Source)
      │
      ▼
AWS CodePipeline  ──►  AWS CodeBuild  ──►  AWS Elastic Beanstalk  ──►  Amazon RDS (MySQL)



Stage	Service	Role
Source	Bitbucket	Hosts source code, triggers pipeline on commit
Orchestration	AWS CodePipeline	Connects source → build → deploy stages
Build	AWS CodeBuild	Compiles code with Maven, injects DB config, packages artifact
Deploy	AWS Elastic Beanstalk	Runs the app across multiple EC2 instances behind a load balancer
Data	Amazon RDS (MySQL)	Stores application data

⚙️ What Was Implemented

#️-what-was-implemented

🔹 Elastic Beanstalk

Load-balanced, multi-instance environment (Tomcat platform, Amazon Linux 2023)
Auto Scaling configured across multiple Availability Zones (2–4 instances)
Session stickiness enabled on the load balancer
Rolling deployments (50% batch) for zero-downtime updates

🔹 Amazon RDS (MySQL)

Free-tier MySQL instance with a dedicated accounts database
Security group restricting inbound traffic on port 3306 to Beanstalk instances only
Schema initialized via SQL script executed over SSH

🔹 Source Control

Migrated repository from GitHub to Bitbucket with full commit/branch history
SSH key-based authentication for all Git operations

🔹 AWS CodeBuild

Custom buildspec.yml with install → pre-build → build → post-build phases
Maven-based compilation and packaging into a deployable artifact
Dynamic injection of RDS credentials into application.properties using sed
Build logs streamed to CloudWatch for troubleshooting

🔹 AWS CodePipeline

3-stage pipeline: Source (Bitbucket) → Build (CodeBuild) → Deploy (Beanstalk)
Auto-triggered on every commit to the aws-ci branch
IAM service role scoped with least-privilege Beanstalk deployment permissions



🔐 Security Practices

#-security-practices

Practice	Implementation
No hardcoded secrets	DB credentials injected at build time, not stored in source
Network isolation	RDS not publicly accessible; reachable only from Beanstalk security group
Least privilege IAM	Scoped roles per service (CodeBuild, CodePipeline, Beanstalk)
Key-based auth	SSH keys used for all Git and EC2 access — no passwords
🐛 Troubleshooting Log

#-troubleshooting-log

Issue	Root Cause	Fix
CodeBuild failed at pre-build	Malformed sed command — missing delimiter	Corrected syntax in buildspec.yml, re-ran build
Beanstalk deployment blocked	Outdated Launch Configuration	Switched root volume to GP3 to force Launch Template usage
📈 Outcome

#-outcome

Single git push triggers full build + rolling deployment automatically
Zero-downtime updates verified across multiple instances
Centralized logging via CloudWatch for fast failure diagnosis


🚧 Future Improvements
#-future-improvements

🏗️  Terraform          →  Replace manual provisioning with IaC
🐳  Docker + EKS        →  Containerize and orchestrate instead of Beanstalk
🔐  Secrets Manager     →  Replace sed-based credential injection
🧪  Automated Testing   →  Add test stage before deployment
📊  Prometheus/Grafana  →  Application-level monitoring dashboards
