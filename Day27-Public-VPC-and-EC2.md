# ☁️ Day 27 – Create Public VPC, Subnet, and EC2 Instance

---

## 📌 Task Overview

The objective of this task was to create a public VPC with a public subnet, enable automatic public IP assignment, and launch an EC2 instance inside this VPC that is accessible over SSH from the internet.

This setup demonstrates foundational AWS networking concepts for hosting public-facing applications.

---

## 🎯 Objectives

- Create a public VPC named `datacenter-pub-vpc`.
- Create a subnet named `datacenter-pub-subnet`.
- Enable auto-assignment of public IPs.
- Launch EC2 instance `datacenter-pub-ec2`.
- Instance type: `t2.micro`.
- Allow SSH (port 22) from the internet.

---

## 🧠 Core Concepts

### ✅ Virtual Private Cloud (VPC)

A VPC is a logically isolated virtual network in AWS where cloud resources are launched.

---

### ✅ Public Subnet

A subnet that routes traffic to the internet through an Internet Gateway and assigns public IPs to instances.

---

### ✅ Auto Assign Public IP

Ensures instances launched in the subnet automatically receive public IPv4 addresses.

---

### ✅ Security Group

Acts as a firewall controlling inbound and outbound traffic to EC2 instances.

---

## 🛠️ Implementation Steps

---

### Step 1: Create Public VPC

- Navigated to VPC → Create VPC.
- Name: `datacenter-pub-vpc`.
- Used default IPv4 CIDR.
- Created VPC successfully.

---

### Step 2: Create Public Subnet

- Created subnet under `datacenter-pub-vpc`.
- Name: `datacenter-pub-subnet`.
- Enabled **Auto-assign Public IPv4 Address**.

---

### Step 3: Attach Internet Gateway

- Created Internet Gateway.
- Attached it to `datacenter-pub-vpc`.
- Updated route table:
  - Destination: `0.0.0.0/0`
  - Target: Internet Gateway

---

### Step 4: Launch EC2 Instance

- Name: `datacenter-pub-ec2`
- AMI: Ubuntu/Linux
- Instance type: `t2.micro`
- Subnet: `datacenter-pub-subnet`
- Enabled public IP.

---

### Step 5: Configure Security Group

Inbound Rule:

- SSH
- Port: 22
- Source: `0.0.0.0/0`

---

### Step 6: Verification

- Instance reached Running state.
- Public IP assigned.
- SSH connectivity confirmed from external host.

---

## 🔍 Verification Summary

- VPC created.
- Subnet created with auto public IP.
- Internet Gateway attached.
- EC2 launched successfully.
- SSH accessible from internet.

---

## 💡 Key Takeaways

- VPC provides network isolation.
- Public subnet enables internet access.
- Internet Gateway connects VPC to internet.
- Security Groups protect instances.
- Public IP assignment is essential for external access.

---

## 🎤 Interview Preparation

**Q1: What is a VPC?**  
A logically isolated virtual network in AWS.

---

**Q2: What makes a subnet public?**  
Route to Internet Gateway + public IP assignment.

---

**Q3: What is Internet Gateway?**  
Allows communication between VPC and internet.

---

**Q4: Why is SSH port 22 required?**  
To allow remote login to EC2.

---

**Q5: Difference between Security Group and NACL?**  
Security Group is stateful; NACL is stateless.

---

## 📅 Progress Log

Day 27 – Public VPC, Subnet, and EC2 Instance Configured ✅ Completed

---
