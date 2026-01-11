# ☁️ Day 01 – AWS EC2 Key Pair Creation (RSA)

## 📌 Challenge Description

The objective of this task was to create an EC2 key pair that will be used to securely access virtual machines during cloud migration and infrastructure setup.

Key pairs enable secure authentication instead of password-based login and are critical for cloud security and automation.

---

## 🎯 Objective

- Create an EC2 key pair with a specific name.
- Use RSA as the encryption type.
- Ensure the key is created in the correct AWS region.
- Validate that the key exists and is usable.

---

## 🧠 Key Concepts Learned

### ✅ What is an EC2 Key Pair?

An EC2 key pair consists of:

- Public Key  → Stored in AWS and injected into the EC2 instance.
- Private Key → Downloaded by the user and used for SSH access.

Only the holder of the private key can access the instance.

This eliminates the need for password-based authentication.

---

### ✅ Why Key Pairs Are Important

- Strong security using cryptographic authentication.
- No password exposure.
- Mandatory for Linux EC2 SSH access.
- Used in automation and provisioning workflows.
- Required for disaster recovery access.

---

### ✅ RSA Key Type

RSA is a widely supported asymmetric encryption algorithm.

Advantages:
- High compatibility across systems.
- Secure and trusted.
- Supported by most SSH clients.

---

### ✅ AWS Region Awareness

AWS resources are region-specific.

If a resource is created in the wrong region:
- It will not be visible where expected.
- Automation and validation may fail.

Always confirm the selected region before creating resources.

---

### ✅ AWS Console vs CLI

AWS resources can be created using:
- AWS Management Console (UI)
- AWS CLI
- Infrastructure as Code tools

In this lab environment, only console access was supported.

---

## ❌ Issues Encountered

- Initial confusion about whether to use CLI or UI.
- Validator mismatch between documentation and backend check.
- AWS backend synchronization delay caused validation failure.
- Multiple retries were required despite correct configuration.

---

## ✅ How the Issue Was Resolved

- Verified key name, type, and region carefully.
- Confirmed the key existed in the AWS Console.
- Identified backend synchronization issue in the lab environment.
- Escalated and waited for sync to complete.

This reflects real-world cloud environments where eventual consistency may cause delays.

---

## 🔍 Verification Summary

- Confirmed key pair exists in EC2 console.
- Verified key name and key type.
- Ensured region was set correctly.
- Confirmed validator eventually passed after synchronization.

---

## 💡 Key Takeaways

- Cloud systems may experience eventual consistency.
- Always validate resources before assuming failure.
- Naming conventions matter in automation.
- Region awareness is critical in cloud deployments.
- CLI access may not always be available in controlled environments.

---

## 🎤 Interview Preparation Notes

### Q1: What is an EC2 key pair?
A key pair is a cryptographic authentication mechanism used to securely access EC2 instances without passwords.

---

### Q2: Why is RSA commonly used for EC2 keys?
RSA provides strong security and wide compatibility across platforms and SSH tools.

---

### Q3: Why must region be selected carefully in AWS?
Resources exist only inside the region where they are created. Incorrect region selection leads to resource visibility issues.

---

### Q4: What is eventual consistency in cloud systems?
Changes may take time to propagate across all backend systems, causing temporary mismatches in visibility.

---

### Q5: Why should private keys be protected?
Loss or exposure of private keys can compromise server access and security.

---

## 📅 Progress Log

Day 01 : AWS EC2 Key Pair Creation (RSA)  ✅ Completed

---

