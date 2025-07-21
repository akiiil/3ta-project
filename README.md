# AWS Three‑Tier Web Application with Full‑Stack Development, Multi‑AZ High Availability, Security Best Practices, and CI/CD Automation

This project implements a **secure, scalable, highly available three‑tier architecture on AWS**, showcasing full-stack development combined with modern cloud-native infrastructure and DevOps practices.

It follows best practices for networking, security, scalability, monitoring, observability, rollback strategies, and operational excellence, with a fully automated CI/CD workflow integrated with GitHub.

---

## 📐 Architecture Overview

![Architecture Diagram](Architecture%20Diagram.png)

### Key Features:
- **Multi‑AZ deployment across two Availability Zones (AZs)** ensures fault tolerance and high availability.
- **Three-tier architecture**: Presentation (frontend), Application (backend), Data (database).
- **Auto Scaling Groups (ASGs)** with **Launch Templates** ensure elasticity and responsiveness to changing load.
- **Application Load Balancer (ALB)** routes HTTPS traffic securely and balances load across healthy instances.
- **Amazon RDS MySQL Multi‑AZ** provides resilient, highly available data persistence.
- **CloudWatch Logs and Alarms** ensure monitoring, observability, and reliability.
- Designed and implemented using **best practices for production deployments**, including rollback strategies.

---

## 🔹 Detailed Components

### 1️⃣ **VPC and Networking**
- Custom **VPC spanning two AZs** with:
  - **Public subnets**: Bastion Hosts and ALB.
  - **Private subnets**: Application/Presentation EC2 instances and RDS MySQL.
- **Internet Gateway (IGW)** for public subnet internet connectivity.
- **NAT Gateways in each AZ** allow private subnets secure outbound access.
- **Route Tables**:
  - Public subnet → IGW.
  - Private subnet → NAT Gateway.

---

### 2️⃣ **DNS and TLS**
- **Amazon Route 53** DNS for custom domains.
- **AWS Certificate Manager (ACM)** manages public SSL/TLS certificates for HTTPS.

---

### 3️⃣ **Security**
- **Security Groups (SGs)**:
  - ALB SG: HTTPS inbound.
  - EC2 SGs: Traffic from ALB or Bastion only.
  - RDS SG: Traffic from App Tier only.
  - Bastion SG: Restricted SSH access.
- **IAM Roles**:
  - Least-privilege permissions for:
    - EC2 instances (CloudWatch Logs, CodeDeploy agent).
    - CodePipeline, CodeBuild, CodeDeploy services.
  - Supports fine-grained security and compliance.

---

### 4️⃣ **Bastion Host and Admin Access**
- Bastion Host in public subnet for secure administration.
- Enables controlled access to private EC2 instances and RDS.

---

## 🖥️ Full‑Stack Application

### 5️⃣ **Presentation Tier**
- ReactJS SPA served via NGINX on private EC2 instances.
- Routed through ALB + CloudFront for performance/security.

### 6️⃣ **Application Tier**
- Node.js API backend with PM2 process manager for production readiness.

### 7️⃣ **Data Tier**
- Amazon RDS MySQL Multi-AZ for resilient storage, automatic failover, encryption.

---

## ⚙️ CI/CD Pipeline Workflow

![CI/CD Pipeline Flow](CICD%20Pipeline%20Flow.png)

A **fully automated CI/CD pipeline** was implemented as a core part of this project.

### App Tier CI/CD:
- CodePipeline triggers on GitHub releases.
- CodeBuild builds Node.js API.
- CodeDeploy deploys to App Tier EC2.

### Presentation Tier CI/CD:
- CodePipeline triggers on GitHub releases.
- CodeBuild builds React frontend.
- CodeDeploy deploys static assets and NGINX config to Presentation Tier EC2.

### Deployment Versioning and Rollback:
- CodeDeploy provides **version control of deployments**.
- Supports automatic rollback to previous stable versions if deployment issues are detected.

---

## 🔔 Monitoring, Logging, Elasticity

- **CloudWatch Logs**:
  - Centralized logs for:
    - NGINX server.
    - Node.js application.
    - System logs across tiers.
  - **Critical for observability and operational diagnostics**.

- **CloudWatch Alarms**:
  - CPU utilization monitoring.
  - Auto triggers ASG scale-out/in for elasticity.

- **Auto Scaling Group (ASG)**:
  - Configured via Launch Templates and Target Groups.
  - Automatically registers new EC2 instances under ALB.

---

## 📦 S3 Buckets and Artifact Management

- S3 buckets used to store:
  - Build artifacts from CodeBuild.
  - Deployment artifacts for CodeDeploy.

---

## 🌐 CloudFront Distribution

- CloudFront as a global CDN:
  - Improves performance via edge caching.
  - SSL termination at edge locations for speed/security.

---

## 🧪 Testing and Validation

End-to-end system testing validated:
- DNS + HTTPS resolution.
- Scaling behavior under load.
- AZ failover for resilience.
- Secure admin workflows.
- Automated deployment pipelines.
- Deployment rollback scenarios tested via CodeDeploy.

---

## 🎯 Key Takeaways

This project provided **comprehensive, real-world, hands-on experience with the following:**

✅ **Automated CI/CD Pipeline Implementation**  
- Full integration with GitHub.
- Automated builds, tests, deployments.
- Separate pipelines for App and Presentation tiers.

✅ **Scalable 3-Tier Architecture Deployment**  
- Secure, modular, cloud-native architecture.
- Multi-AZ redundancy for high availability.

✅ **Deployment Versioning and Rollback Strategies**  
- CodeDeploy enables tracking and rollback to stable versions automatically.

✅ **Monitoring and Logging for Reliability**  
- CloudWatch Logs centralizes observability.
- Alarms and metrics provide proactive monitoring and auto-scaling triggers.

✅ **Best Practices for Production Deployments**  
- Private/public subnet design.
- Security groups + IAM principle of least privilege.
- Bastion host for secure ops.
- Elastic Load Balancer + Auto Scaling + Multi-AZ RDS.
- End-to-end encryption (ACM + CloudFront + HTTPS).

✅ **Full-Stack Development Expertise**  
- React frontend + NGINX for web serving.
- Node.js backend + PM2 for production process management.
- Secure, integrated, cloud-native application deployment.

---
