# ☁️ Day 44 – AWS Auto Scaling Group + Application Load Balancer

---

## 📌 Task Overview

The Nautilus DevOps team required a highly available web application using AWS **EC2 Auto Scaling** and an **Application Load Balancer (ALB)**.

The infrastructure automatically maintains the required number of EC2 instances, distributes incoming HTTP traffic across healthy instances, and scales based on **CPU utilization**.

Each EC2 instance automatically installs and runs **Nginx** using EC2 User Data.

---

## 🏗️ AWS Architecture

```text
                         Internet
                            │
                            │ HTTP :80
                            ▼
                 ┌─────────────────────┐
                 │   datacenter-alb    │
                 │ Application LB      │
                 └──────────┬──────────┘
                            │
                            │ HTTP :80
                            ▼
                 ┌─────────────────────┐
                 │    datacenter-tg    │
                 │   Health Checks /   │
                 └──────────┬──────────┘
                            │
                 ┌──────────┴──────────┐
                 │                     │
                 ▼                     ▼
          ┌──────────────┐      ┌──────────────┐
          │ EC2 Instance │      │ EC2 Instance │
          │    Nginx     │      │    Nginx     │
          │     :80      │      │     :80      │
          └──────────────┘      └──────────────┘
                 ▲                     ▲
                 │                     │
                 └──────────┬──────────┘
                            │
                    datacenter-asg
                     Min: 1
                     Desired: 1
                     Max: 2
                            │
                     CPU Target: 50%
```

---

## ☁️ AWS Resources

- Launch Template: `datacenter-launch-template`
- AMI: Amazon Linux 2023
- Instance Type: `t2.micro`
- Web Server: Nginx
- Security Group: HTTP port `80`
- Auto Scaling Group: `datacenter-asg`
- Minimum Instances: `1`
- Desired Instances: `1`
- Maximum Instances: `2`
- Scaling Metric: Average CPU Utilization
- CPU Target: `50%`
- Target Group: `datacenter-tg`
- Load Balancer: `datacenter-alb`
- Listener: HTTP `80`
- Health Check: HTTP `/`
- Region: `us-east-1`

---

## 🔐 Security Group

A security group was created for the web servers.

The security group allows HTTP traffic:

```text
Type: HTTP
Protocol: TCP
Port: 80
Source: 0.0.0.0/0
```

Outbound traffic was left as the default configuration.

---

## 🏗️ Launch Template

A launch template named:

```text
datacenter-launch-template
```

was created.

### Configuration

```text
AMI: Amazon Linux 2023
Instance Type: t2.micro
Security Group: HTTP :80
```

The launch template provides a consistent configuration for every EC2 instance launched by the Auto Scaling Group.

---

## 🧩 User Data Configuration

User Data was configured in the launch template so that Nginx is automatically installed whenever a new EC2 instance is launched.

```bash
#!/bin/bash

dnf install -y nginx
systemctl start nginx
systemctl enable nginx
```

### User Data Workflow

```text
EC2 launched
     │
     ▼
Install Nginx
     │
     ▼
Start Nginx
     │
     ▼
Enable Nginx at boot
     │
     ▼
Web server ready
```

---

## 🎯 Target Group

A target group named:

```text
datacenter-tg
```

was created.

### Configuration

```text
Target Type: Instances
Protocol: HTTP
Port: 80
```

### Health Check

```text
Protocol: HTTP
Path: /
Port: Traffic port
```

The health check ensures that the ALB sends traffic only to healthy EC2 instances.

---

## ⚖️ Application Load Balancer

An Application Load Balancer named:

```text
datacenter-alb
```

was created.

### Configuration

```text
Type: Application Load Balancer
Scheme: Internet-facing
Protocol: HTTP
Port: 80
```

The ALB forwards incoming requests to:

```text
datacenter-tg
```

Traffic flow:

```text
Client
  │
  │ HTTP :80
  ▼
datacenter-alb
  │
  │ HTTP :80
  ▼
datacenter-tg
  │
  ▼
Healthy EC2 instance
  │
  ▼
Nginx :80
```

---

## 🚀 Auto Scaling Group

An Auto Scaling Group named:

```text
datacenter-asg
```

was created using:

```text
datacenter-launch-template
```

### Capacity Configuration

```text
Minimum: 1
Desired: 1
Maximum: 2
```

This ensures that at least one EC2 instance is running.

The ASG can increase the number of instances up to two when required.

---

## 📈 CPU-Based Scaling

A **Target Tracking Scaling Policy** was configured.

Metric:

```text
Average CPU Utilization
```

Target:

```text
50%
```

The ASG attempts to maintain average CPU utilization around:

```text
50%
```

Scaling behavior:

```text
High CPU
   │
   ▼
ASG launches additional instance
   │
   ▼
Maximum = 2
```

When demand decreases:

```text
Low CPU
   │
   ▼
ASG can terminate extra instance
   │
   ▼
Minimum = 1
```

---

## 🔗 Attach Auto Scaling Group to Target Group

The Auto Scaling Group was associated with:

```text
datacenter-tg
```

This allows instances launched by the ASG to be automatically registered with the target group.

When an instance is terminated, it is automatically removed from the target group.

```text
datacenter-asg
       │
       ├── EC2 Instance 1
       │
       └── EC2 Instance 2
              │
              ▼
        datacenter-tg
              │
              ▼
        datacenter-alb
```

---

## ❤️ Health Checks

The target group performs HTTP health checks on:

```text
HTTP :80
Path: /
```

A healthy target receives traffic from the ALB.

An unhealthy target is removed from traffic until it becomes healthy again.

```text
Healthy EC2
    │
    ▼
Receives traffic ✅

Unhealthy EC2
    │
    ▼
No traffic ❌
```

---

## 🖥️ Verify the EC2 Instance

The ASG automatically launched an EC2 instance using:

```text
Amazon Linux 2023
t2.micro
```

The User Data script installed Nginx.

Nginx can be verified with:

```bash
sudo systemctl status nginx
```

Expected:

```text
Active: active (running)
```

Local testing:

```bash
curl http://localhost
```

The command should return the default Nginx HTML page.

---

## 👀 Verify Target Health

Navigate to:

```text
EC2 → Target Groups → datacenter-tg → Targets
```

The ASG instance should eventually show:

```text
Health status: healthy
```

Expected state:

```text
datacenter-tg
      │
      ▼
EC2 instance
      │
      ▼
healthy ✅
```

---

## 🌐 Verify ALB DNS

Navigate to:

```text
EC2 → Load Balancers → datacenter-alb
```

Copy the ALB DNS name.

It will look similar to:

```text
datacenter-alb-xxxxxxxx.us-east-1.elb.amazonaws.com
```

Open:

```text
http://<ALB-DNS-NAME>
```

The default Nginx page should be displayed.

Expected:

```text
Welcome to nginx!
```

---

## 🔐 Security Configuration

The traffic path is:

```text
Internet
   │
   │ HTTP :80
   ▼
ALB Security Group
   │
   │ HTTP :80
   ▼
EC2 Security Group
   │
   ▼
Nginx
```

For a more secure production configuration, the EC2 security group should allow port 80 **only from the ALB security group**, rather than from the entire internet.

---

## 🔄 High Availability

The Auto Scaling Group and ALB work together to provide availability.

```text
                  Application Load Balancer
                           │
                ┌──────────┴──────────┐
                │                     │
                ▼                     ▼
             EC2 #1                EC2 #2
             Nginx                 Nginx
                │                     │
                └──────────┬──────────┘
                           │
                     Auto Scaling
                     Min = 1
                     Max = 2
```

If CPU utilization increases, the ASG can launch another instance.

If an instance becomes unhealthy, the ASG can replace it.

---

## 🧪 Verification Checklist

- Launch Template created: ✅
- Name `datacenter-launch-template`: ✅
- AMI Amazon Linux 2023: ✅
- Instance type `t2.micro`: ✅
- HTTP port 80 allowed: ✅
- Nginx User Data configured: ✅
- Nginx starts automatically: ✅
- Auto Scaling Group created: ✅
- Name `datacenter-asg`: ✅
- Minimum `1`: ✅
- Desired `1`: ✅
- Maximum `2`: ✅
- CPU target `50%`: ✅
- Target tracking policy configured: ✅
- Target Group created: ✅
- Name `datacenter-tg`: ✅
- Protocol `HTTP`: ✅
- Port `80`: ✅
- Health check `HTTP /`: ✅
- Target becomes healthy: ✅
- ALB created: ✅
- Name `datacenter-alb`: ✅
- Internet-facing: ✅
- Listener `HTTP :80`: ✅
- Forwarding to `datacenter-tg`: ✅
- ALB DNS verified: ✅
- Default Nginx page accessible: ✅

---

## 🎯 Final Result

The highly available web application infrastructure was successfully configured using:

```text
Launch Template
       ↓
Auto Scaling Group
       ↓
Target Group
       ↓
Application Load Balancer
       ↓
Nginx EC2 Instances
```

The system maintains **at least one EC2 instance**, can scale up to **two instances based on 50% CPU utilization**, performs health checks, and distributes HTTP traffic through the ALB.

### Final AWS Setup

```text
datacenter-launch-template
             │
             ▼
       datacenter-asg
       ┌─────┴─────┐
       │           │
     EC2 #1      EC2 #2
       │           │
       └─────┬─────┘
             ▼
       datacenter-tg
             ▲
             │
       datacenter-alb
             │
             ▼
        HTTP :80
             │
             ▼
       Default Nginx Page
```

**Auto Scaling + ALB + Nginx deployment completed successfully.**
