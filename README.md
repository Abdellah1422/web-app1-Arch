# web-app1-Arch
# 3-Tier Web Application on AWS with Terraform

> **Note:** This project uses Terraform to deploy a scalable and highly available 3-tier application architecture on AWS.  
>  



## 🧩 Solution Overview

This project implements a scalable and resilient **3-tier web application architecture** on AWS . 
It is designed for workloads that need separation of concerns (frontend, backend, database), scalability, high availability, and automated provisioning.

This solution automatically provisions networking, compute, storage, monitoring, and CDN resources in a modular fashion. The infrastructure follows best practices and supports the following capabilities:

- Frontend and backend tiers run on EC2 instances in Auto Scaling Groups (ASGs).
- Compute resources are split across multiple Availability Zones to improve fault tolerance.
- Amazon RDS is used to host a managed, persistent database layer.
- Amazon CloudFront is used to serve static assets and frontend content globally, reducing latency.
- AWS CloudWatch is used to monitor CPU metrics and trigger alarms.
- Security Groups provide strict access control between components.
- Scripts (UserData) allow automatic installation of web server software (e.g., NGINX, Apache, etc.).

This solution can serve as a production-ready base infrastructure for hosting any modern web application, such as an e-commerce platform, content management system (CMS), or microservice-based backend API.

## 🖼 Architecture Diagram

The 3-Tier architecture consists of the following components:

1. **Frontend Tier**
   - Deployed in a public subnet.
   - Uses Auto Scaling Group with EC2 instances.
   - UserData installs a web server and optionally connects to the backend API.
   - Content is accelerated globally via Amazon CloudFront.

2. **Backend Tier**
   - Deployed in a private subnet.
   - Backend EC2 instances handle business logic and connect securely to the database.
   - ASG ensures dynamic scaling based on load.

3. **Database Tier**
   - Amazon RDS is provisioned in a private subnet.
   - Accessible only via backend Security Group.

4. **Networking**
   - VPC with 2 or more Availability Zones.
   - Public and private subnets separated via route tables.
   - Internet Gateway for frontend traffic, and NAT Gateway for backend EC2 outbound internet.

5. **Monitoring and Scaling**
   - CloudWatch monitors CPU, triggers alarms, and can be extended with custom metrics.
   - Auto Scaling policies respond to traffic spikes.

6. **Security**
   - Each tier has its own Security Group with least-privilege access.
   - SSH access is managed using a key pair created via Terraform.

The overall infrastructure is highly modular, making it easy to extend with services like AWS WAF, ALB, or S3 static hosting.

Key Features:
- Highly Available across multiple Availability Zones
- Automatically Scalable using AWS Auto Scaling Groups
- Modular Design using Terraform modules for reusability
- Monitoring with CloudWatch and log collection
- Fast Global Content Delivery via CloudFront


## 🧩 Component Breakdown

### VPC & Networking
- Custom VPC with public and private subnets across 2 AZs
- Internet Gateway and NAT Gateway for routing

### Auto Scaling Groups
- Separate ASGs for frontend and backend
- Configured with Launch Templates and health checks

### EC2 & User Data
- Scripts in `UserData` directories configure the web servers
- Includes installation of web servers, app code, and dependencies

### Database Layer
- RDS MySQL/PostgreSQL deployed in private subnet
- Accessible only from backend security group

### CloudWatch & Monitoring
- Alarms for CPU usage and instance health
- Logging available for further analysis

### CloudFront CDN
- Distributes frontend content globally with low latency

