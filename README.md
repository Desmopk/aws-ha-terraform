# AWS High-Availability Infrastructure Automation (Terraform)

![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)

> 🚧 **STATUS: ACTIVE SPRINT** 
> This repository is currently under active development. The architectural blueprint and CI/CD pipelines are finalized. Infrastructure codebase execution and final AWS deployment are scheduled for completion by **May 25, 2026**.

## 📌 Overview
This repository contains the Infrastructure as Code (IaC) configuration to provision a secure, highly available, and load-balanced multi-tier architecture on Amazon Web Services (AWS). It demonstrates immutable infrastructure practices and asynchronous delivery methodologies.

## 🏗️ Target Architecture Blueprint

The deployment utilizes **Terraform (HCL)** to orchestrate the following resources:

### 1. Networking & Security
* **Custom VPC:** Isolated network environment across two Availability Zones (AZs) for high availability.
* **Subnetting:** Public subnets for load balancers and NAT Gateways; Private subnets for application compute resources.
* **Security Groups:** Least-privilege firewall rules restricting inbound traffic exclusively to the Application Load Balancer (ALB).

### 2. Compute & Load Balancing
* **Application Load Balancer (ALB):** Distributes incoming HTTP/HTTPS traffic across instances in multiple AZs.
* **EC2 Auto Scaling Group / Target Group:** Containerized application workloads deployed via Docker, adhering strictly to **12-Factor App** principles (stateless processes, environment-based configurations).

### 3. State Management
* **S3 Backend:** Remote state storage ensuring secure configuration tracking.
* **DynamoDB Locking:** State-locking mechanism to prevent concurrent execution conflicts and configuration drift.

## 🚀 CI/CD Pipeline (GitHub Actions)
Continuous Integration is enforced via `.github/workflows`. On every Pull Request to the `main` branch, the pipeline executes:
1. `terraform fmt -check` (Syntax standard enforcement)
2. `terraform validate` (Structural integrity check)
3. `terraform plan` (Execution blueprint generation)

## 📂 Planned Directory Structure
```text
aws-ha-terraform/
├── .github/workflows/
│   └── terraform-ci.yml      # CI/CD Pipeline definitions
├── modules/
│   ├── networking/           # VPC, Subnets, IGW, Route Tables
│   ├── compute/              # EC2 instances, ALB, Target Groups
│   └── security/             # IAM Roles, Security Groups
├── backend.tf                # S3 + DynamoDB state configuration
├── main.tf                   # Root module execution
├── variables.tf              # Environment variable definitions
├── outputs.tf                # Deployed resource ARNs and DNS endpoints
└── README.md
