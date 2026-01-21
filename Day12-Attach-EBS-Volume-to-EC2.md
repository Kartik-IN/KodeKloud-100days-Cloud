# ☁️ Day 12 – Attach EBS Volume to EC2 Instance

## 📌 Challenge Description

The objective of this task was to attach an existing Amazon EBS volume to a running EC2 instance to provide additional persistent storage.

This allows applications to store data outside the instance root disk.

---

## 🎯 Objective

- Identify the available EBS volume.
- Select the target EC2 instance.
- Attach the volume correctly.
- Verify successful attachment.

---

## 🧠 Key Concepts Learned

### ✅ What is an EBS Volume?

Amazon EBS provides persistent block storage for EC2 instances.

Characteristics:
- Data persists after instance stop/restart.
- High durability and availability.
- Used for databases, file systems, and applications.

---

### ✅ Why Attach Additional Volumes?

- Increase storage capacity.
- Separate OS and data.
- Improve performance planning.
- Enable backups and snapshots.

---

### ✅ Availability Zone Dependency

EBS volumes can only be attached to EC2 instances within the same Availability Zone.

---

### ✅ Device Naming

When attaching a volume, a device name is assigned (e.g., /dev/xvdf).

The OS uses this device to mount and format storage.

---

### ✅ File System Preparation

After attachment:
- Volume must be formatted.
- Mounted to a directory.
- Permissions configured.

---

## ❌ Issues and Learning

- Verified availability zone alignment.
- Confirmed correct device mapping.
- Understood that OS-level setup is required after attachment.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to EC2 → Volumes.
- Selected the target volume.
- Clicked Attach Volume.
- Selected the EC2 instance.
- Assigned device name.
- Verified attachment status.

---

## 🔍 Verification Summary

- Volume attached successfully.
- Volume state shows In-use.
- Instance recognizes the new volume.

---

## 💡 Key Takeaways

- EBS provides persistent storage independent of compute.
- AZ matching is mandatory.
- Proper mounting is required inside OS.
- Storage planning impacts performance and cost.

---

## 🎤 Interview Preparation Notes

### Q1: What is an EBS volume?
Persistent block storage for EC2 instances.

---

### Q2: Can a volume be attached to multiple instances?
Not simultaneously (except multi-attach volumes).

---

### Q3: Why must AZ match for volume attachment?
Because EBS is AZ-scoped.

---

### Q4: What must be done after attaching a volume inside Linux?
Format, mount, and configure filesystem.

---

### Q5: Difference between EBS and instance store?
EBS is persistent; instance store is temporary.

---

## 📅 Progress Log

Day 12 : EBS Volume Attached to EC2 Instance  ✅ Completed

---

