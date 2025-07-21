# AWS Three‑Tier Web Application with Full‑Stack Development, Multi‑AZ High Availability, Security Best Practices, and CI/CD Automation

This project implements a **secure, scalable, highly available three‑tier architecture on AWS**, showcasing full-stack development combined with modern cloud-native infrastructure and DevOps practices.

It follows best practices for networking, security, scalability, monitoring, and operational excellence, with a fully automated CI/CD workflow integrated with GitHub.

---

## 📐 Architecture Overview

![Architecture Diagram](Architecture%20Diagram.png)

### Key Features:
- **Multi‑AZ deployment across two Availability Zones (AZs)** ensures fault tolerance and high availability.
- **Three-tier architecture**: Presentation (frontend), Application (backend), Data (database).
- **Auto Scaling Groups (ASGs)** with **Launch Templates** ensure elasticity and responsiveness to changing load.
- **Application Load Balancer (ALB)** routes HTTPS traffic securely and balances load across healthy instances.
- **Amazon RDS MySQL Multi‑AZ** provides resilient, highly available data persistence.
- **CloudWatch Logs and Alarms** ensure monitoring and observability at every layer.

---

## 🔹 Detailed Components

### 1️⃣ **VPC and Networking**
- Custom **VPC spanning two AZs** with:
  - **Public subnets**: Bastion Hosts and ALB.
  - **Private subnets**: Application/Presentation EC2 instances and RDS MySQL.
- **Internet Gateway (IGW)** for public subnet outbound/inbound traffic.
- **NAT Gateways in each AZ**: Allow secure outbound access for private subnet instances.
- **Route Tables**:
  - Public subnet → IGW.
  - Private subnet → NAT Gateway.

---

### 2️⃣ **DNS and TLS**
- **Amazon Route 53** provides DNS resolution for custom domains.
- **AWS Certificate Manager (ACM)** issues and manages SSL certificates for HTTPS.

---

### 3️⃣ **Security**
- **Security Groups (SGs)**:
  - ALB SG: Inbound HTTPS (443) from any IP.
  - EC2 SGs: Accept traffic only from ALB or Bastion as appropriate.
  - RDS SG: Allows traffic only from Application Tier EC2 SG.
  - Bastion Host SG: SSH restricted to trusted IP ranges.
- **IAM Roles**:
  - **EC2 Instance Profile**: Enables CodeDeploy agent and CloudWatch Logs access.
  - **CodePipeline Role**: Orchestrates build/deploy workflow.
  - **CodeBuild Role**: S3 read/write and CloudWatch Logs.
  - **CodeDeploy Role**: EC2 deploy permissions.

---

### 4️⃣ **Bastion Host and Secure Administration**
- Bastion Hosts deployed in public subnet for restricted admin access.
- Used for securely accessing private resources such as EC2 and RDS for management.

---

## 🖥️ Full‑Stack Application

### 5️⃣ **Presentation Tier**
- **ReactJS Single Page Application (SPA)**.
- Served via **NGINX on EC2 instances in private subnets**.
- Traffic routed through ALB + CloudFront for performance and security.

### 6️⃣ **Application Tier**
- **Node.js API backend** providing business logic.
- **PM2** ensures uptime, auto‑restart, and centralized logging.

### 7️⃣ **Data Tier**
- **Amazon RDS MySQL Multi-AZ**:
  - Automatic failover from primary to standby during AZ disruption.
  - Encryption at rest and in transit.
  - Private-only access from Application Tier EC2 instances.

---

## ⚙️ CI/CD Pipeline Workflow

![CI/CD Pipeline Flow](CICD%20Pipeline%20Flow.png)

### App Tier CI/CD:
- **CodeBuild for App Tier**:
  - Installs dependencies, runs tests, builds backend artifacts.
  - Stores build artifacts in S3.
- **CodeDeploy for App Tier**:
  - Zero-downtime deployment to App Tier EC2 instances.
- **CodePipeline for App Tier**:
  - GitHub → CodeBuild → CodeDeploy.

### Presentation Tier CI/CD:
- **CodeBuild for Presentation Tier**:
  - `npm install` and `npm run build` for React frontend.
  - Uploads artifacts to S3.
- **CodeDeploy for Presentation Tier**:
  - Deploys built static assets and NGINX config to Presentation Tier EC2.
- **CodePipeline for Presentation Tier**:
  - Fully automated end-to-end workflow.

---

## 🔔 Monitoring, Logging, Elasticity

- **CloudWatch Logs**:
  - Centralized logs for:
    - NGINX server logs.
    - Node.js application logs.
    - System logs for all EC2 instances.
  - Enables diagnostics, observability, and operational excellence.

- **CloudWatch Alarms**:
  - CPU utilization monitoring.
  - Triggering ASG scale-out and scale-in policies automatically.

- **Auto Scaling Group (ASG)**:
  - Launches instances using Launch Templates.
  - Automatically registers instances with ALB Target Groups.
  - Ensures horizontal scalability and resilience.

---

## 📦 S3 Buckets and Artifact Management

- Used for:
  - Storing build artifacts from CodeBuild.
  - Temporary storage of deployment assets for CodeDeploy.

---

## 🌐 CloudFront Distribution

- **Amazon CloudFront**:
  - Global content distribution network (CDN).
  - Edge caching for static content.
  - Integrated with ALB for dynamic content delivery.
  - SSL termination at edge locations for performance and security.

---

## 🧪 Testing and Validation

End-to-end system testing included:
- DNS and HTTPS resolution.
- Load testing for scaling behavior.
- Simulated AZ failure scenarios.
- Verified CI/CD automation pipelines.
- Validated secure RDS connectivity from private subnets.

---

## 🎯 Key Takeaways

This project provided **practical, real-world experience designing and deploying a modern, production-ready AWS application**:

✅ **High Availability Architecture**:
- Multi-AZ deployment ensures fault tolerance and service continuity.
- Auto Scaling and Load Balancer integration ensures elasticity.

✅ **Secure Network Design**:
- Private subnet isolation for application and database layers.
- NAT Gateways and IGW carefully integrated with Route Tables.
- Bastion Host architecture for secure admin workflows.

✅ **Full-Stack Development**:
- ReactJS frontend + Node.js API backend with PM2 + NGINX for production-grade delivery.

✅ **CI/CD Automation and DevOps Proficiency**:
- CodePipeline, CodeBuild, CodeDeploy orchestrated for both App and Presentation tiers.
- GitHub integrated for automated continuous delivery.

✅ **Operational Observability**:
- CloudWatch Logs enabled for centralized logging across tiers.
- CloudWatch Alarms actively monitor key metrics and trigger scaling events.

✅ **IAM and Security Governance**:
- Roles and permissions following least-privilege principle for EC2, CodeBuild, CodeDeploy, and CodePipeline.

✅ **Infrastructure Readiness and Scalability**:
- Launch Templates and Target Groups ensure dynamic instance provisioning and healthy target registration.

---

This README provides a **complete technical overview** of the project, reflecting deep knowledge of AWS architecture, networking, security, DevOps workflows, and full-stack application delivery.
