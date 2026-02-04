# ☁️ Day 24 – Setting Up an Application Load Balancer for EC2 Instance

---

## 📌 Task Overview

The objective of this task was to configure an Application Load Balancer (ALB) in front of an EC2 instance running Nginx, enabling external HTTP traffic to be routed through the load balancer to the backend instance.

This setup simulates a real-world production architecture where traffic is managed through a load balancer.

---

## 🎯 Objectives

- Create an Application Load Balancer named `devops-alb`.
- Create a Target Group named `devops-tg`.
- Create a Security Group `devops-sg` allowing HTTP (port 80).
- Attach the security group to the ALB.
- Register EC2 instance `devops-ec2` with the target group.
- Route traffic from ALB port 80 to EC2 port 80.
- Modify EC2 security group to allow ALB traffic.

---

## 🧠 Core Concepts

### ✅ Application Load Balancer (ALB)

ALB distributes incoming HTTP traffic across backend targets and provides:

- High availability
- Scalability
- Health checks
- Layer 7 routing

---

### ✅ Target Group

Defines backend targets (EC2 instances) and health check rules.

---

### ✅ Security Groups

Firewall rules controlling inbound and outbound traffic.

---

### ✅ Listener

Listens on port 80 and forwards requests to target group.

---

## 🛠️ Implementation Steps

### Step 1: Create Security Group

- Name: `devops-sg`
- Inbound Rule:
  - HTTP
  - Port: 80
  - Source: `0.0.0.0/0`

---

### Step 2: Create Target Group

- Name: `devops-tg`
- Target type: Instance
- Protocol: HTTP
- Port: 80
- Register EC2 instance `devops-ec2`

---

### Step 3: Create Application Load Balancer

- Name: `devops-alb`
- Type: Application
- Scheme: Internet-facing
- Attach security group `devops-sg`
- Select public subnets

---

### Step 4: Configure Listener

- Protocol: HTTP
- Port: 80
- Forward traffic to `devops-tg`

---

### Step 5: Update EC2 Security Group

Allow inbound HTTP traffic from ALB security group.

---

### Step 6: Verify Setup

- Copy ALB DNS name
- Open in browser:
http://<ALB-DNS>

Nginx sample page loads successfully.

---

## 🔍 Verification Summary

- ALB created successfully.
- Target group registered.
- Security groups configured.
- EC2 receiving traffic.
- Application accessible via ALB DNS.

---

## 💡 Key Takeaways

- ALB improves availability and scalability.
- Target groups manage backend instances.
- Security groups control traffic flow.
- Load balancers are critical for production architectures.

---

## 🎤 Interview Preparation

**Q1: What is an Application Load Balancer?**  
Distributes HTTP/HTTPS traffic at Layer 7.

---

**Q2: Difference between ALB and NLB?**  
ALB is Layer 7; NLB is Layer 4.

---

**Q3: What is a Target Group?**  
Collection of backend targets receiving traffic.

---

**Q4: Why use ALB?**  
High availability, scaling, and fault tolerance.

---

**Q5: Can one ALB serve multiple applications?**  
Yes, using listener rules and path-based routing.

---

## 📅 Progress Log

Day 24 – Application Load Balancer Configured for EC2 ✅ Completed

---
