# ☁️ Day 21 – Launch EC2 Instance with Elastic IP for Application Hosting

---

## 📌 Challenge Description

The objective of this task was to launch a new EC2 instance and associate an Elastic IP with it to provide a stable and consistent public IP address for application hosting.

This setup ensures reliable access to the application even after instance restarts.

---

## 🎯 Objective

- Launch an EC2 instance with a specific name.
- Use a Linux-based AMI.
- Select the required instance type.
- Allocate and associate an Elastic IP.
- Verify stable public connectivity.

---

## 🧠 Key Concepts Learned

### ✅ EC2 Instance for Application Hosting

Amazon EC2 provides scalable compute capacity for running applications in the cloud.

Proper instance configuration ensures performance and reliability.

---

### ✅ Instance Type – t2.micro

- Low-cost, general-purpose instance
- Suitable for testing and small workloads
- Eligible for free-tier (in many regions)

---

### ✅ What is an Elastic IP?

An Elastic IP (EIP) is a static public IPv4 address provided by AWS.

Unlike default public IPs:
- Elastic IP remains unchanged across restarts
- Can be reassigned to another instance if needed

---

### ✅ Why Elastic IP is Important

- Stable public access
- Reliable DNS mapping
- Production-ready networking
- Failover capability

---

### ✅ Resource Naming

Consistent naming improves:
- Resource management
- Cost tracking
- Automation
- Troubleshooting

---

## ❌ Issues and Learning

- Ensured instance was in the correct region.
- Verified instance type selection.
- Confirmed Elastic IP association.
- Checked public IP persistence.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Launched a new EC2 instance:
  - Name: datacenter-ec2
  - AMI: Linux (Ubuntu or similar)
  - Instance Type: t2.micro
- Allocated a new Elastic IP.
- Associated Elastic IP with the EC2 instance.
- Verified public IP assignment.

---

## 🔍 Verification Summary

- EC2 instance launched successfully.
- Elastic IP associated correctly.
- Public IP remains consistent.
- Instance reachable using Elastic IP.

---

## 💡 Key Takeaways

- Elastic IP provides stable public access.
- EC2 instances require proper networking configuration.
- Resource naming improves cloud hygiene.
- Combining EC2 and EIP is common in production setups.

---

## 🎤 Interview Preparation Notes

### Q1: Why use Elastic IP instead of default public IP?
Default public IP changes on restart; Elastic IP remains constant.

---

### Q2: Can one Elastic IP be attached to multiple instances?
No, only one resource at a time.

---

### Q3: What happens to Elastic IP if instance is terminated?
Elastic IP remains allocated until manually released.

---

### Q4: Is Elastic IP free?
Free only when attached to a running instance.

---

### Q5: Can Elastic IP be moved to another instance?
Yes, it supports reassignment for failover.

---

## 📅 Progress Log

Day 21 : EC2 Instance Launched with Elastic IP  ✅ Completed

---

