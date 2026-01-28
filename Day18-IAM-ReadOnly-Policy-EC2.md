# ☁️ Day 18 – Create IAM Read-Only Policy for EC2

## 📌 Challenge Description

The objective of this task was to create a custom IAM policy that grants read-only access to Amazon EC2 resources.

The policy allows users to view EC2 instances, AMIs, and snapshots without allowing any modification actions.

---

## 🎯 Objective

- Create a custom IAM policy.
- Allow read-only access to EC2 resources.
- Restrict write or delete permissions.
- Validate policy creation.

---

## 🧠 Key Concepts Learned

### ✅ What is IAM?

Identity and Access Management (IAM) controls:
- Who can access AWS resources
- What actions they can perform
- Which resources they can access

---

### ✅ What is an IAM Policy?

A policy is a JSON document that defines permissions using:
- Actions
- Resources
- Effects (Allow/Deny)

---

### ✅ Principle of Least Privilege

Users should only receive permissions necessary to perform their job.

Read-only access prevents accidental changes.

---

### ✅ EC2 Read-Only Permissions

Key read-only actions include:
- DescribeInstances
- DescribeImages
- DescribeSnapshots

These allow visibility but not modification.

---

### ✅ IAM Policy Scope

Policies can be:
- Attached to users
- Attached to groups
- Attached to roles

---

## ❌ Issues and Learning

- Verified correct region before creation.
- Ensured only read permissions were included.
- Validated policy syntax.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to IAM → Policies.
- Created a new policy using JSON editor.
- Defined EC2 read-only permissions.
- Named the policy correctly.
- Saved and validated policy.

---


---

## 💡 Key Takeaways

- IAM enforces access security.
- Least privilege prevents mistakes.
- Policies control granular permissions.
- Read-only roles are ideal for auditing.

---

## 🎤 Interview Preparation Notes

### Q1: What is IAM?
AWS service for managing access and permissions.

---

### Q2: What is least privilege?
Granting minimal required access.

---

### Q3: Difference between User and Role?
User is permanent identity; Role is assumed.

---

### Q4: Why avoid wildcard permissions?
They increase security risk.

---

### Q5: Can policies be reused?
Yes, policies can be attached to multiple entities.

---

## 📅 Progress Log

Day 18 : IAM Read-Only Policy Created  ✅ Completed

---



