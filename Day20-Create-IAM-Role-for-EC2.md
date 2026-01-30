# ☁️ Day 20 – Create IAM Role for EC2 with Policy Attachment

## 📌 Challenge Description

The objective of this task was to create an IAM role for EC2 and attach the required IAM policy to allow EC2 instances to securely access AWS services without using access keys.

IAM roles provide temporary credentials and follow best security practices.

---

## 🎯 Objective

- Create an IAM role.
- Select EC2 as the trusted service.
- Attach the required IAM policy.
- Validate role creation and configuration.

---

## 🧠 Key Concepts Learned

### ✅ What is an IAM Role?

An IAM role is an AWS identity that:
- Has no permanent credentials
- Provides temporary security credentials
- Is assumed by trusted entities like EC2, Lambda, or users

---

### ✅ Why Use IAM Roles for EC2?

- No hardcoded access keys
- Automatic credential rotation
- Improved security
- Simplified access management

---

### ✅ Trust Relationship

The trust policy defines **who can assume the role**.

For EC2 roles:

---

### ✅ Policy Attachment

Policies define **what actions EC2 can perform** once it assumes the role.

---

### ✅ Role vs User

| IAM User | IAM Role |
|--------|----------|
| Permanent credentials | Temporary credentials |
| Manual key rotation | Automatic rotation |
| Assigned to people | Assigned to services |

---

## ❌ Issues and Learning

- Verified trusted entity configuration.
- Ensured correct policy attachment.
- Understood role assumption flow.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to IAM → Roles.
- Created a new role.
- Selected EC2 as trusted service.
- Attached required IAM policy.
- Reviewed and created role.

---

## 🔍 Verification Summary

- IAM role created successfully.
- Policy attached correctly.
- Role available for EC2 attachment.

---

## 💡 Key Takeaways

- IAM roles improve cloud security.
- EC2 uses temporary credentials.
- Trust policies control role assumption.
- Roles are essential for production workloads.

---

## 🎤 Interview Preparation Notes

### Q1: What is an IAM role?
An AWS identity that provides temporary permissions.

---

### Q2: Why use IAM role instead of access keys on EC2?
For better security and automatic credential rotation.

---

### Q3: What is a trust policy?
Defines who can assume the role.

---

### Q4: Can one role be attached to multiple EC2 instances?
Yes.

---

### Q5: How does EC2 access AWS services using roles?
Via temporary credentials from AWS metadata service.

---

## 📅 Progress Log

Day 20 : IAM Role Created and Policy Attached for EC2  ✅ Completed

---

