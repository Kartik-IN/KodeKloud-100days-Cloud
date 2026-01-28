# ☁️ Day 17 – Create IAM Group

## 📌 Challenge Description

The objective of this task was to create an IAM group to manage permissions efficiently for multiple users.

IAM groups simplify permission management by applying policies to multiple users at once.

---

## 🎯 Objective

- Create a new IAM group.
- Attach appropriate policies.
- Add users to the group.
- Validate group configuration.

---

## 🧠 Key Concepts Learned

### ✅ What is an IAM Group?

An IAM group is a collection of IAM users.

Permissions are assigned to the group instead of individual users.

---

### ✅ Why Use IAM Groups?

- Centralized permission management.
- Consistency across users.
- Easy onboarding and offboarding.
- Reduced administrative overhead.

---

### ✅ Policy Inheritance

Users inherit permissions from all groups they belong to.

---

### ✅ Security Best Practices

- Apply least privilege policies.
- Avoid assigning permissions directly to users.
- Audit group memberships regularly.

---

### ✅ Scalability

Groups scale well when managing large teams.

---

## ❌ Issues and Learning

- Verified group name consistency.
- Ensured correct policies were attached.
- Confirmed users were added properly.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to IAM → Groups.
- Created a new group.
- Attached required IAM policies.
- Added users to the group.
- Verified permissions.

---

## 🔍 Verification Summary

- IAM group created successfully.
- Policies attached correctly.
- Users inherited expected permissions.

---

## 💡 Key Takeaways

- Groups simplify access control.
- Policy management is centralized.
- Least privilege improves security.
- Groups support scalable governance.

---

## 🎤 Interview Preparation Notes

### Q1: What is an IAM Group?
A collection of users sharing the same permissions.

---

### Q2: Can groups have access keys?
No, groups only manage permissions.

---

### Q3: Can a user belong to multiple groups?
Yes.

---

### Q4: Why not assign permissions directly to users?
Harder to manage and audit.

---

### Q5: What happens when a policy is updated on a group?
All users inherit the change immediately.

---

## 📅 Progress Log

Day 17 : IAM Group Created  ✅ Completed

---

