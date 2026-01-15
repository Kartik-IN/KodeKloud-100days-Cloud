# ☁️ Day 05 – Create AWS EBS Volume (gp3)

## 📌 Challenge Description

The objective of this task was to create an Amazon Elastic Block Store (EBS) volume with a specific name, type, and size to support cloud infrastructure storage requirements.

EBS volumes provide persistent block-level storage for EC2 instances.

---

## 🎯 Objective

- Create an EBS volume with a defined name.
- Select the correct volume type.
- Configure the required storage size.
- Validate successful volume creation.

---

## 🧠 Key Concepts Learned

### ✅ What is an EBS Volume?

Amazon EBS provides persistent block storage that can be attached to EC2 instances.

It behaves like a virtual hard disk:
- Data remains even after instance reboot or stop.
- Supports high availability and durability.
- Used for operating systems, databases, and application storage.

---

### ✅ Volume Type – gp3

gp3 is a general-purpose SSD volume type.

Benefits:
- High performance
- Predictable throughput
- Cost optimized
- Independent scaling of performance and size

---

### ✅ Volume Size Planning

Volume size determines:
- Total storage capacity
- Cost impact
- Application scalability

Even small volumes are useful for testing and lab environments.

---

### ✅ Availability Zone Dependency

EBS volumes exist in a specific availability zone.

They can only be attached to EC2 instances in the same zone.

---

### ✅ Tagging and Naming

Naming volumes using tags helps:
- Resource tracking
- Cost management
- Automation consistency
- Operational clarity

---

## ❌ Issues and Learning

- Carefully verified volume type selection.
- Confirmed storage size before creation.
- Ensured naming consistency for validation.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to EC2 service.
- Opened the Volumes section.
- Created a new volume with:
  - Type: gp3
  - Size: 2 GiB
  - Name tag: xfusion-volume
- Verified volume status.

---

## 🔍 Verification Summary

- Volume exists with correct name.
- Volume type is gp3.
- Size is 2 GiB.
- Volume state is available.

---

## 💡 Key Takeaways

- EBS provides persistent block storage.
- Volume type impacts performance and cost.
- AZ alignment is required for attachment.
- Tagging simplifies management and automation.

---

## 🎤 Interview Preparation Notes

### Q1: What is an EBS volume?
A persistent block storage service used with EC2 instances.

---

### Q2: Difference between gp2 and gp3?
gp3 provides independent performance scaling and lower cost compared to gp2.

---

### Q3: Can an EBS volume be attached across availability zones?
No, volumes must be attached within the same AZ.

---

### Q4: What happens to EBS data when instance stops?
Data remains preserved unless the volume is deleted.

---

### Q5: Why is tagging important in cloud resources?
It helps in organization, billing, automation, and governance.

---

## 📅 Progress Log

Day 05 : EBS Volume Creation (gp3)  ✅ Completed

---

