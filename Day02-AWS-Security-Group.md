# ☁️ Day 02 – AWS Security Group Creation (Network Security Basics)

## 📌 Challenge Description

The objective of this task was to create a security group under the default VPC to control inbound traffic for application servers during a cloud migration process.

The security group acts as a virtual firewall that defines which traffic is allowed to reach cloud resources.

---

## 🎯 Objective

- Create a security group with a specific name and description.
- Attach the security group to the default VPC.
- Configure inbound rules for HTTP and SSH access.
- Validate that the rules are correctly applied.

---

## 🧠 Key Concepts Learned

### ✅ What is a Security Group?

A security group is a virtual firewall that controls inbound and outbound traffic for AWS resources such as EC2 instances.

It allows administrators to define:
- Which protocols are allowed
- Which ports are open
- Which IP ranges can access the resource

All traffic that is not explicitly allowed is blocked.

---

### ✅ Why Security Groups Are Important

- Protect servers from unauthorized access.
- Enforce least-privilege network access.
- Provide centralized security control.
- Easily modifiable without restarting instances.
- Integrated with AWS networking services.

---

### ✅ Understanding Inbound Rules

Inbound rules define which traffic is allowed to enter the resource.

For this task:

HTTP (Port 80):
- Allows web traffic from the internet.
- Used for serving web applications.

SSH (Port 22):
- Allows administrators to connect remotely.
- Used for server management.

Source CIDR:
- `0.0.0.0/0` allows traffic from any IP address.

---

### ✅ Default VPC Usage

Every AWS account has a default VPC which provides a ready-to-use network environment.

Using the default VPC simplifies:
- Network configuration
- Subnet selection
- Routing setup

---

### ✅ Stateful Firewall Behavior

Security groups are stateful:
- If inbound traffic is allowed, return traffic is automatically allowed.
- No need to explicitly allow outbound response traffic.

---

## ❌ Mistakes and Learning

- Initially confused between security group rules and firewall rules.
- Had to carefully verify port numbers and CIDR blocks.
- Learned that naming and descriptions must match exactly for automation validation.

---

## ✅ How the Task Was Completed

- Navigated to EC2 Security Groups in AWS Console.
- Created a new security group with the required name and description.
- Selected the default VPC.
- Added inbound rules for HTTP and SSH with correct ports and CIDR.
- Verified configuration before submitting.

---

## 🔍 Verification Summary

- Security group exists in the correct region.
- Name and description match requirements.
- Default VPC is selected.
- Two inbound rules are configured correctly.
- Ports and CIDR ranges are validated.

---

## 💡 Key Takeaways

- Security groups act as cloud firewalls.
- Always validate network rules before deployment.
- Least privilege is best practice in production.
- Region and VPC awareness is critical.
- Network security is foundational in cloud architecture.

---

## 🎤 Interview Preparation Notes

### Q1: What is a security group in AWS?
A virtual firewall that controls inbound and outbound traffic for AWS resources.

---

### Q2: What does 0.0.0.0/0 mean?
It allows traffic from any IP address on the internet.

---

### Q3: Difference between Security Group and Network ACL?
Security Group is stateful and instance-level, while Network ACL is stateless and subnet-level.

---

### Q4: Why should SSH access not be open to 0.0.0.0/0 in production?
It increases attack surface and should be restricted to trusted IP ranges.

---

### Q5: Can security group rules be modified after creation?
Yes, rules can be modified dynamically without restarting instances.

---

## 📅 Progress Log

Day 02 : AWS Security Group Creation  ✅ Completed

---

