# ☁️ Day 19 – Attach IAM Policy to IAM User
---
## 📌 Challenge Description

The objective of this task was to attach an IAM policy to a specific IAM user in order to grant controlled access to AWS resources.

Attaching policies directly defines what actions a user can perform.

---

## 🎯 Objective

- Identify the IAM user.
- Select the appropriate IAM policy.
- Attach the policy to the user.
- Validate permission assignment.

---

## 🧠 Key Concepts Learned

### ✅ What is an IAM Policy Attachment?

Policy attachment links permission rules to identities such as:
- Users
- Groups
- Roles

The policy defines allowed or denied actions.

---

### ✅ Why Attach Policies to Users?

- Grant specific permissions.
- Control access securely.
- Enforce least privilege.
- Enable accountability.

---

### ✅ Direct User Policy vs Group Policy

Best practice is attaching policies to groups rather than individual users for scalability.

---

### ✅ Permission Evaluation Order

AWS evaluates:
- Explicit deny
- Explicit allow
- Default deny

---

### ✅ IAM Auditing

IAM allows auditing permissions using:
- IAM Access Analyzer
- CloudTrail

---

## ❌ Issues and Learning

- Verified correct user selection.
- Ensured correct policy attachment.
- Confirmed permission inheritance behavior.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to IAM → Users.
- Selected the target user.
- Opened Permissions tab.
- Attached the required policy.
- Verified successful attachment.

---

## 🔍 Verification Summary

- Policy attached successfully.
- User permissions updated correctly.
- Access validated.

---

## 💡 Key Takeaways

- Policies define permissions.
- Least privilege reduces risk.
- Groups simplify management.
- Auditing improves security posture.

---

## 🎤 Interview Preparation Notes

### Q1: What is an IAM policy?
A JSON document defining permissions.

---

### Q2: Can multiple policies be attached to one user?
Yes.

---

### Q3: Which takes priority – Allow or Deny?
Explicit deny always wins.

---

### Q4: Why attach policies to groups instead of users?
Easier management and scalability.

---

### Q5: How to audit user permissions?
Use IAM Access Analyzer or CloudTrail.

---

## 📅 Progress Log

Day 19 : IAM Policy Attached to User  ✅ Completed

---

