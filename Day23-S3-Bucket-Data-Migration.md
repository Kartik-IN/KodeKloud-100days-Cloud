# ☁️ Day 23 – S3 Data Migration Using AWS CLI

---

## 📌 Task Overview

This task involved migrating data from an existing Amazon S3 bucket to a newly created private S3 bucket while ensuring data consistency and integrity.

The migration was performed using AWS CLI to simulate a real-world data migration scenario.

---

## 🎯 Objectives

- Create a new private S3 bucket.
- Migrate all data from the existing bucket.
- Ensure data consistency between source and destination.
- Verify successful migration.

---

## 🧠 Key Concepts Explained

### ✅ Amazon S3
Amazon S3 is an object storage service designed for scalability, durability, and availability.

---

### ✅ Why Use AWS CLI for S3 Migration?
- Faster than manual downloads/uploads
- Scriptable and automatable
- Used in production DevOps workflows

---

### ✅ Why `aws s3 sync`?
- Copies all files recursively
- Maintains directory structure
- Transfers only missing or updated files
- Ensures efficient and reliable migration

---

## 🛠️ Implementation Steps

### Step 1: Create New Private S3 Bucket

```bash
aws s3 mb s3://devops-sync-19799 --region us-east-1
``` 
Step 2: Sync Data from Existing Bucket
```bash
aws s3 sync s3://devops-s3-775 s3://devops-sync-19799
``` 

Step 3: Verify Data Consistency
Source bucket:
aws s3 ls s3://devops-s3-775 --recursive

Destination bucket:
aws s3 ls s3://devops-sync-19799 --recursive

Matching outputs confirm successful migration.

🔍 Verification Summary
New S3 bucket created successfully.

All data copied from source bucket.

Data structure preserved.

No data loss observed.

💡 Key Takeaways
AWS CLI is powerful for storage operations.

aws s3 sync is ideal for migrations.

Data verification is critical in production.

S3 buckets are private by default.

🎤 Interview Preparation Notes
Q1: Difference between cp and sync in S3?
sync copies only changed files; cp copies everything.

Q2: Is S3 sync incremental?
Yes, it syncs only differences.

Q3: Are S3 buckets private by default?
Yes.

Q4: How to verify S3 data migration?
By comparing recursive object listings.

📅 Progress Log
Day 23 – S3 Bucket Data Migration Completed ✅
