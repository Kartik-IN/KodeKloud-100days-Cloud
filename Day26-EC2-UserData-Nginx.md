# ☁️ Day 26 – Launch EC2 with User Data Script (Auto Install Nginx)

---

## 📌 Task Overview

The goal of this task was to launch an EC2 instance using an Ubuntu AMI and configure it with a **user data script** so that Nginx is automatically installed and started during the first boot.

This demonstrates cloud automation using EC2 user data.

---

## 🎯 Objectives

- Launch EC2 instance named `nautilus-ec2`
- Use Ubuntu AMI
- Configure User Data to:
  - Install Nginx
  - Start Nginx service
- Allow HTTP traffic on port 80
- Verify Nginx via browser

---

## 🧠 Core Concepts

### ✅ EC2 User Data

User Data is a startup script that runs automatically when an EC2 instance launches.  
It is commonly used to:

- Install packages
- Configure services
- Bootstrap applications

---

### ✅ Why Use User Data?

- Enables automation
- Removes manual server setup
- Used in DevOps & Infrastructure as Code
- Ideal for initial provisioning

---

## 🛠️ Implementation Steps

---

### Step 1: Launch EC2 Instance

- Logged into AWS Console
- Region: `us-east-1`
- Created EC2 instance:
  - Name: `nautilus-ec2`
  - AMI: Ubuntu
- Configured Security Group:
  - Allow HTTP (port 80) from `0.0.0.0/0`

---

### Step 2: Add User Data Script

While creating EC2, added the following **User Data**:

```bash
#!/bin/bash
apt update -y
apt install nginx -y
systemctl start nginx
systemctl enable nginx
```

### Step 3: Launch Instance

Completed EC2 creation and waited until instance reached Running state.

---

### Step 4: Verify Nginx

Copied Public IPv4 address and opened in browser:

```bash
http://<EC2_PUBLIC_IP>
```

Default Nginx page loaded successfully.

---

## 🔍 Verification Summary

- EC2 instance created successfully
- User data executed during boot
- Nginx installed automatically
- HTTP traffic allowed
- Web page accessible

---

## 💡 Key Takeaways

- User data automates server initialization
- Nginx can be installed without manual SSH
- Security Groups control web access

This is foundational for cloud automation.

🎤 Interview Preparation
Q1: What is EC2 User Data?
Startup script executed during first boot of EC2.

Q2: When does user data run?
Only during initial launch unless manually re-triggered.

Q3: Why enable Nginx service?
To ensure it starts automatically on reboot.

Q4: Where can you see user data logs?
/var/log/cloud-init-output.log

Q5: Why is user data important in DevOps?
It supports automated provisioning and Infrastructure as Code.

📅 Progress Log
Day 26 – EC2 Created with User Data Nginx Setup ✅ Completed
