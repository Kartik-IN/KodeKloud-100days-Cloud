# ☁️ Day 13 – Create AMI from EC2 Instance

## 📌 Challenge Description

The objective of this task was to create an Amazon Machine Image (AMI) from an existing EC2 instance to enable fast provisioning, backup, and disaster recovery.

An AMI captures the complete system state of an instance.

---

## 🎯 Objective

- Identify the source EC2 instance.
- Create an AMI with a specific name.
- Ensure the AMI reaches the available state.
- Validate successful image creation.

---

## 🧠 Key Concepts Learned

### ✅ What is an AMI?

An Amazon Machine Image (AMI) is a template that contains:
- Operating system
- Installed software
- System configuration
- Attached snapshots

AMI enables rapid cloning of servers.

---

### ✅ Why Create an AMI?

- Backup and recovery
- Auto-scaling deployments
- Environment consistency
- Disaster recovery
- Fast server provisioning

---

### ✅ AMI Creation Process

When an AMI is created:
- EBS snapshots are taken automatically.
- Instance state is captured.
- Image becomes available after snapshot completion.

---

### ✅ AMI State Lifecycle

AMI states:
- Pending → Snapshot creation
- Available → Ready for use
- Failed → Creation error

Only available AMIs can be launched.

---

### ✅ Storage and Cost Consideration

- AMI itself is metadata.
- Storage cost applies to underlying snapshots.

---

## ❌ Issues and Learning

- Verified instance selection carefully.
- Monitored AMI state until available.
- Understood snapshot dependency.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to EC2 → Instances.
- Selected the source instance.
- Created image from instance.
- Assigned required name.
- Monitored image creation.
- Verified available status.

---

## 🔍 Verification Summary

- AMI created successfully.
- AMI state shows Available.
- Name matches requirement.

---

## 💡 Key Takeaways

- AMIs enable rapid scaling.
- Snapshots provide persistence.
- Image lifecycle must be monitored.
- Cost planning matters.

---

## 🎤 Interview Preparation Notes

### Q1: What is an AMI?
A template used to launch EC2 instances.

---

### Q2: What happens during AMI creation?
EBS snapshots are taken and system state is captured.

---

### Q3: Can AMIs be shared across accounts?
Yes, AMIs can be shared or copied.

---

### Q4: Can an AMI be used across regions?
Yes, but it must be copied to the target region.

---

### Q5: Does creating AMI cause downtime?
Brief I/O pause may occur depending on configuration.

---

## 📅 Progress Log

Day 13 : AMI Created from EC2 Instance  ✅ Completed

---

