# ☁️ Day 15 – Create EBS Volume Snapshot

## 📌 Challenge Description

The objective of this task was to create a snapshot of an existing Amazon EBS volume to ensure data backup and recovery readiness.

Snapshots are point-in-time backups stored in Amazon S3 and are used for disaster recovery and data protection.

---

## 🎯 Objective

- Identify the target EBS volume.
- Create a snapshot with specified name and description.
- Monitor snapshot progress.
- Verify snapshot reaches completed state.

---

## 🧠 Key Concepts Learned

### ✅ What is an EBS Snapshot?

An EBS snapshot is a point-in-time backup of an EBS volume.

It captures the current data state and stores it securely in Amazon S3.

---

### ✅ Why Snapshots Are Important

- Disaster recovery
- Backup automation
- Data migration
- Volume cloning
- Compliance

---

### ✅ Incremental Backup Behavior

Snapshots are incremental:
- Only changed blocks are stored after first snapshot.
- Saves storage cost and improves performance.

---

### ✅ Snapshot Lifecycle

Snapshot states:
- Pending
- Completed
- Error

Only completed snapshots are safe for restore.

---

### ✅ Restore Options

Snapshots can be used to:
- Create new volumes
- Copy across regions
- Share with other accounts

---

## ❌ Issues and Learning

- Verified correct volume selection.
- Monitored snapshot status carefully.
- Confirmed snapshot naming consistency.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to EC2 → Volumes.
- Selected the target volume.
- Created snapshot with required name and description.
- Monitored until snapshot reached completed state.

---

## 🔍 Verification Summary

- Snapshot created successfully.
- Snapshot status shows Completed.
- Name and description match requirements.

---

## 💡 Key Takeaways

- Snapshots protect against data loss.
- Incremental backups reduce cost.
- Monitoring status ensures reliability.
- Backup strategy is critical in production.

---

## 🎤 Interview Preparation Notes

### Q1: What is an EBS snapshot?
A point-in-time backup of an EBS volume stored in S3.

---

### Q2: Are EBS snapshots incremental?
Yes, only changed data blocks are stored.

---

### Q3: Can snapshots be copied across regions?
Yes, snapshots can be copied between regions.

---

### Q4: Can you create volume from snapshot?
Yes, snapshots can create new volumes.

---

### Q5: Are snapshots encrypted?
They inherit encryption from source volume.

---

## 📅 Progress Log

Day 15 : EBS Snapshot Created  ✅ Completed

---

