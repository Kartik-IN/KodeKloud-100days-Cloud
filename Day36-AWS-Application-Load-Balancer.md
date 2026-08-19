# ☁️ Day 36 – AWS Application Load Balancer with EC2 and Nginx

---

## 📌 Task Overview

This project sets up an AWS Application Load Balancer (ALB) in front of an Ubuntu EC2 instance running Nginx.

The ALB receives HTTP traffic on port `80` and forwards requests to the EC2 instance through a target group.

---

## 🏗️ Architecture

```text
                    Internet
                       │
                       │ HTTP :80
                       ▼
              ┌────────────────---┐
              │  datacenter-alb   │
              │   default SG      │
              └---------+---------┘
                        │
                        │ HTTP :80
                        ▼
              ┌────────────────---┐
              │  datacenter-tg    │
              └---------+---------┘
                        │
                        ▼
              ┌────────────────---┐
              │  datacenter-ec2   │
              │  datacenter-sg     │
              │     Nginx :80     │
              └────────────────---┘
```

---

## ☁️ AWS Resources Created

- Region: `us-east-1`
- Security Group: `datacenter-sg`
- EC2 Instance: `datacenter-ec2`
- OS: Ubuntu
- Web Server: Nginx on port `80`
- Target Group: `datacenter-tg`
- Load Balancer: `datacenter-alb`
- ALB Security Group: `default`

---

## 🔐 Security Group Configuration

### ✅ `datacenter-sg` — EC2 Security Group

Inbound rule:

```text
Type:     HTTP
Protocol: TCP
Port:     80
Source:   default Security Group
```

Outbound rules were kept at their default configuration.

---

### ✅ `default` — ALB Security Group

Inbound rule:

```text
Type:     HTTP
Protocol: TCP
Port:     80
Source:   0.0.0.0/0
```

Outbound traffic was left at the default configuration.

---

### ✅ Traffic Flow

```text
Internet
   |
   | TCP 80
   v
default SG
   |
   | TCP 80
   v
datacenter-sg
   |
   v
datacenter-ec2
```

This ensures that the EC2 instance accepts HTTP traffic from the ALB rather than directly exposing port 80 to the internet.

---

## 💻 EC2 Configuration

### Instance

```text
Name: datacenter-ec2
OS: Ubuntu
Web Server: Nginx
Port: 80
Security Group: datacenter-sg
```

---

### ✅ User Data Script

The EC2 instance was configured with the following user data:

```bash
#!/bin/bash
apt-get update -y
apt-get install -y nginx
systemctl enable nginx
systemctl start nginx
```

The script:

1. Updates Ubuntu package repositories
2. Installs Nginx
3. Enables Nginx to start automatically
4. Starts the Nginx service

---

### ✅ Verify Nginx

```bash
sudo systemctl status nginx
```

Expected:

```text
Active: active (running)
```

Test locally:

```bash
curl http://localhost
```

The command should return the Nginx welcome page HTML.

---

## 🎯 Target Group Configuration

Target group:

```text
Name:         datacenter-tg
Target type:  Instances
Protocol:     HTTP
Port:         80
VPC:          Same VPC as datacenter-ec2
```

---

### ✅ Health Check

```text
Protocol: HTTP
Path: /
Port: Traffic port
```

Target:

```text
datacenter-ec2 : 80
```

The target should eventually show:

```text
Healthy
```

---

### ✅ Availability Zone Requirement

The Availability Zone containing `datacenter-ec2` must be enabled for `datacenter-alb`.

Otherwise the target can show:

```text
Unused
```

with a message similar to:

```text
Target is in an Availability Zone that is not enabled for the load balancer
```

---

## ⚖️ Application Load Balancer Configuration

```text
Name:             datacenter-alb
Type:             Application Load Balancer
Scheme:           Internet-facing
IP Address Type:  IPv4
Security Group:   default
```

---

### ✅ Listener

```text
Protocol: HTTP
Port:     80
Action:   Forward
Target:   datacenter-tg
```

Traffic is therefore routed as:

```text
ALB :80
   |
   v
datacenter-tg :80
   |
   v
datacenter-ec2 :80
   |
   v
Nginx
```

---

## 🌐 Testing

After the target becomes healthy:

1. Go to **EC2 → Load Balancers**
2. Select `datacenter-alb`
3. Copy the **DNS name**
4. Open it in a browser using HTTP:

```text
http://<ALB-DNS-NAME>
```

Expected result:

```text
Welcome to nginx!
```

---

## 🛠️ Troubleshooting

### 1. Nginx is not installed

Run:

```bash
sudo apt-get update -y
sudo apt-get install -y nginx
sudo systemctl enable --now nginx
```

Verify:

```bash
sudo systemctl status nginx
```

---

### 2. APT lock error

If you see:

```text
Could not get lock /var/lib/apt/lists/lock
```

check the running package process:

```bash
ps aux | grep -E 'apt|dpkg' | grep -v grep
```

Do not manually delete APT or dpkg lock files.

If a stuck `apt-get` process is confirmed, terminate that process and then repair dpkg:

```bash
sudo dpkg --configure -a
```

---

### 3. Target shows `Unhealthy`

Check Nginx:

```bash
sudo systemctl status nginx
```

Test:

```bash
curl http://localhost
```

Check that `datacenter-sg` allows:

```text
HTTP :80
Source: default SG
```

---

### 4. Target shows `Unused`

Check the Availability Zones configured for `datacenter-alb`.

The ALB must have a subnet enabled in the same Availability Zone as `datacenter-ec2`.

---

### 5. ALB cannot be accessed

Verify the `default` security group allows:

```text
HTTP :80
Source: 0.0.0.0/0
```

Also verify that:

```text
HTTP :80 → datacenter-tg
```

is configured on the ALB listener.

---

## ✅ Final Validation Checklist

- AWS region set to `us-east-1`
- `datacenter-sg` created
- EC2 port 80 allowed from `default` SG
- `datacenter-ec2` created using Ubuntu
- Nginx installed
- Nginx service started and enabled
- `datacenter-tg` created
- `datacenter-ec2` registered on port 80
- Target health check configured for `/`
- Target is healthy
- `datacenter-alb` created as an internet-facing ALB
- `default` SG attached to ALB
- ALB listener configured on HTTP port 80
- Listener forwards traffic to `datacenter-tg`
- EC2 Availability Zone enabled on the ALB
- ALB DNS successfully serves the Nginx page

---

## 🎓 Key AWS Concepts Practiced

- EC2 instance deployment
- Ubuntu User Data
- Nginx installation and service management
- AWS Security Groups
- Application Load Balancers
- Target Groups
- ALB Listeners
- HTTP health checks
- Availability Zones
- ALB-to-EC2 traffic flow
- AWS networking and security
