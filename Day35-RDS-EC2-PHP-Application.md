# ☁️ Day 35 – AWS RDS + EC2 PHP Application Deployment

---

## 📌 Task Overview

In this task, an AWS RDS MySQL database was created and connected to an existing EC2 instance running a PHP application.

The objective was to establish secure communication between the EC2 instance and private RDS database and verify the connection through a web browser.

---

## 🏗️ Architecture

```text
                    Internet
                       │
                       │ HTTP :80
                       ▼
              ┌─────────────────┐
              │    devops-ec2   │
              │                 │
              │  Apache / PHP   │
              │    index.php    │
              └────────┬────────┘
                       │
                       │ MySQL :3306
                       ▼
              ┌─────────────────┐
              │   devops-rds    │
              │                 │
              │  MySQL Database │
              │   devops_db     │
              └─────────────────┘
```

---

## ☁️ AWS Resources

- EC2 Instance: `devops-ec2`
- RDS Instance: `devops-rds`
- Database Engine: MySQL
- Database: `devops_db`
- DB Username: `devops_admin`
- Instance Type: `db.t3.micro`
- Storage Type: `gp2`
- RDS Access Port: `3306`
- Web Server Port: `80`

---

## 🔐 Security Configuration

### ✅ RDS Security Group

Configured inbound access to allow the EC2 instance to connect to MySQL:

```text
Protocol: TCP
Port: 3306
Source: devops-ec2 Security Group
```

The RDS instance was configured as a private database so that database access is restricted to resources within the VPC.

---

### ✅ EC2 Security Group

HTTP access was enabled:

```text
Protocol: TCP
Port: 80
Source: 0.0.0.0/0
```

This allows the PHP application to be accessed through the EC2 instance's public IP.

---

## 🛠️ Implementation

### Step 1: Create RDS MySQL Instance

Created an RDS MySQL database instance:

```text
Instance: devops-rds
Engine: MySQL
Instance Type: db.t3.micro
Storage: gp2
Database: devops_db
Master Username: devops_admin
```

Configured the RDS security group to allow inbound traffic on port `3306` from the `devops-ec2` security group.

---

### Step 2: SSH Key Configuration

An RSA SSH key was created on the `aws-client` host:

```bash
ssh-keygen -t rsa -b 4096 -f /root/.ssh/id_rsa
```

The public key was added to the root user's authorized keys on `devops-ec2`.

The SSH connection was then tested using:

```bash
ssh -i /root/.ssh/id_rsa root@<EC2_PUBLIC_IP>
```

This enabled password-less SSH access.

---

### Step 3: Deploy PHP Application

The application file was available on the `aws-client` host:

```text
/root/index.php
```

It was copied to the EC2 web server:

```bash
scp -i /root/.ssh/id_rsa /root/index.php \
  root@<EC2_PUBLIC_IP>:/var/www/html/
```

The deployed file was located at:

```text
/var/www/html/index.php
```

---

### Step 4: Configure RDS Connection

The PHP application was configured to connect to the RDS MySQL endpoint.

Example configuration:

```php
$servername = "RDS_ENDPOINT";
$username = "devops_admin";
$password = "RDS_PASSWORD";
$dbname = "devops_db";
```

The RDS endpoint was used instead of `localhost` because the database is hosted on Amazon RDS rather than directly on the EC2 instance.

---

### Step 5: Test RDS Connectivity

RDS connectivity from EC2 was tested using:

```bash
nc -zv <RDS_ENDPOINT> 3306
```

MySQL connectivity could also be verified with:

```bash
mysql -h <RDS_ENDPOINT> -u devops_admin -p
```

After confirming database connectivity, the Apache/PHP application was accessed using:

```bash
http://<EC2_PUBLIC_IP>
```

---

## ✅ Verification

The application successfully connected to the RDS database.

Expected browser output:

```text
Connected successfully
```

This confirmed that:

- EC2 was reachable over HTTP
- Apache/PHP was running correctly
- EC2 could communicate with RDS
- Port `3306` was correctly configured
- The PHP application successfully authenticated with MySQL
- The `devops_db` database was accessible

---

## 🧠 Key Concepts Learned

### ✅ Amazon RDS

Amazon Relational Database Service is a managed database service that handles provisioning, patching, and backups automatically.

---

### ✅ VPC Networking

Resources within a VPC can communicate privately without exposing the database to the internet.

---

### ✅ Security Groups

Security groups act as virtual firewalls controlling inbound and outbound traffic between AWS resources.

---

### ✅ Private Database Deployment

The RDS instance was configured as private so that database access is restricted to resources within the VPC, improving security.

---

### ✅ Application-to-Database Communication

The PHP application on EC2 communicates with the RDS MySQL database over the internal VPC network on port `3306`.

---

## 🏗️ Architecture Pattern

This architecture demonstrates the separation of concerns:

```text
┌──────────────────┐
│   EC2 Instance   │
│  (Web Tier)      │
│                  │
│  Apache + PHP    │
└────────┬─────────┘
         │
         │ Internal VPC
         │ Communication
         │
┌────────▼─────────┐
│  RDS Instance    │
│  (Data Tier)     │
│                  │
│  MySQL Database  │
└──────────────────┘
```

Benefits:

- Database is not exposed to the internet
- EC2 handles web traffic
- RDS handles database operations
- Each layer can scale independently
- Security group rules enforce restricted access

---

## 🔄 Connection Flow

The complete connection flow was:

```text
Browser
   ↓
HTTP Request :80
   ↓
EC2 Instance
   ↓
Apache/PHP
   ↓
PHP Application
   ↓
MySQL Connection :3306
   ↓
RDS Instance
   ↓
MySQL Database
   ↓
Query Results
   ↓
PHP Response
   ↓
Browser Display
```

---

## 🎤 Interview Questions

### 1. Why is the RDS database private?

A private RDS database is not exposed to the internet, reducing the attack surface. Access is restricted to resources within the VPC via Security Group rules.

### 2. How does the EC2 instance communicate with RDS?

EC2 communicates with RDS through the internal VPC network using the RDS endpoint and port `3306` (MySQL default).

### 3. What is the purpose of Security Groups?

Security Groups act as virtual firewalls, controlling which resources can communicate with each other based on port and protocol rules.

### 4. Why separate the web server and database server?

Separation of tiers allows independent scaling, improves security by keeping the database private, and follows the principle of separation of concerns.

### 5. What information does the PHP application need to connect to RDS?

- RDS Endpoint (hostname)
- Database Username
- Database Password
- Database Name
- Port (default 3306 for MySQL)

### 6. How was the SSH key used in this task?

The SSH key enabled password-less authentication from the `aws-client` to the EC2 instance, allowing secure file transfer via SCP.

### 7. Why use SCP instead of SSH?

SCP (Secure Copy) is used for transferring files securely from the client to the EC2 instance using the same SSH credentials.

---

## 🔐 Security Best Practices

- Keep the RDS database private and not publicly accessible
- Use Security Groups to restrict access to only required resources
- Use strong passwords for database users
- Never store database passwords in application source code
- Use AWS Secrets Manager or environment variables for credentials
- Enable RDS encryption at rest and in transit
- Use SSL/TLS for database connections when possible
- Regularly backup RDS databases
- Monitor RDS activity using CloudWatch and AWS CloudTrail

---

## 📚 AWS Services & Concepts Practiced

- Amazon EC2
- Amazon RDS
- MySQL Database
- VPC Networking
- Security Groups
- SSH Key-based Authentication
- SCP File Transfer
- Apache Web Server
- PHP Application
- MySQL Connections
- AWS Console
- Database Management

---

## 📅 Cloud 100 Days Progress

**Day 35 – AWS RDS + EC2 PHP Application Deployment**

Status: ✅ **Completed**

---

## 🚀 Learning Outcome

This task provided hands-on experience deploying a real-world application architecture combining:

- EC2 for application hosting
- RDS for database management
- VPC networking for secure communication
- Security Groups for access control
- SSH for secure administration

The complete workflow demonstrated:

```text
Create RDS Database
        ↓
Configure Security Groups
        ↓
Generate SSH Keys
        ↓
Copy PHP Application to EC2
        ↓
Configure Application
        ↓
Test Connectivity
        ↓
Access Web Application
        ↓
Verify Database Connection
```

---

## ⭐ Summary

Successfully deployed:

```text
Application Server:
devops-ec2

Web Server:
Apache + PHP

Database Server:
devops-rds (RDS MySQL)

Database:
devops_db

Database User:
devops_admin

Application:
index.php

Web Port:
80

Database Port:
3306

Status:
Connected successfully ✅
```

This completes **Cloud Day 35**. 🎯
