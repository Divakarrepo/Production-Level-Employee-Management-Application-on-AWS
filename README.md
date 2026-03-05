# 🚀 Production-Level Employee Management Application on AWS

## 📌 Project Overview

This project demonstrates a highly available, secure, and scalable 3-tier architecture deployed on Amazon Web Services (AWS).

The application allows users to:

- Add employee details
- Store data in MySQL database
- Retrieve stored employee records

---

## 🏗️ Architecture Diagram

![Architecture Diagram](architecture.png)

---

## 🛠️ AWS Services Used

- Amazon VPC
- Amazon EC2
- Application Load Balancer (ALB)
- EC2 Auto Scaling
- Amazon RDS (MySQL)
- Internet Gateway
- NAT Gateway
- Security Groups
- Route Tables

---

## 🧱 Architecture Components

### 🌐 Networking Layer

- Custom VPC (10.0.0.0/16)
- 1 Public Subnet (Load Balancer + Bastion Host)
- 2 Private App Subnets (Auto Scaling EC2 Instances)
- 1 Private DB Subnet (RDS)
- Internet Gateway for public access
- NAT Gateway for private instance outbound internet

---

### 🔐 Security Layer

Security Groups configured with layered access:

| Component | Allowed Access |
|-----------|---------------|
| Bastion Host | SSH (22) from My IP |
| Load Balancer | HTTP (80) from 0.0.0.0/0 |
| App Servers | HTTP (80) from ALB, SSH (22) from Bastion |
| RDS | MySQL (3306) from App Servers |

Database is NOT publicly accessible.

---

### ⚖️ High Availability & Scaling

- Application Load Balancer distributes traffic
- Auto Scaling Group configuration:
  - Min: 2
  - Desired: 2
  - Max: 4
- Health checks enabled
- Automatic instance replacement on failure

---

## 🗄️ Database Layer

Engine: MySQL  
Hosted on: Amazon RDS  
Deployment: Private Subnet  

### Table Structure

```sql
CREATE TABLE employees(
id INT AUTO_INCREMENT PRIMARY KEY,
name VARCHAR(50),
email VARCHAR(50),
department VARCHAR(50)
);
```

### 🔄 Application Flow

- User accesses Load Balancer DNS
- ALB forwards traffic to healthy EC2 instances
- Application stores/retrieves data from RDS
- Bastion host used for secure SSH access

### 📈 Auto Scaling Test

- Manually terminate one EC2 instance
- Auto Scaling automatically launches replacement
- Zero downtime for end users
