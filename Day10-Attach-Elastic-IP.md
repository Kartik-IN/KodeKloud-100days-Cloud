# ☁️ Day 10 – Attach Elastic IP to EC2 Instance

---

## 📌 Challenge Description

The objective of this task was to attach an existing Elastic IP address to an EC2 instance to ensure stable and persistent public connectivity.

Elastic IPs provide static public IP addresses that remain unchanged even if the instance is restarted.

---

## 🎯 Objective

- Identify the EC2 instance.
- Locate the allocated Elastic IP.
- Associate the Elastic IP with the EC2 instance.
- Verify successful attachment.

---

## 🧠 Key Concepts Learned

### ✅ What is an Elastic IP?

An Elastic IP (EIP) is a static public IPv4 address provided by AWS.

It allows:
- Persistent public access
- Stable DNS mapping
- Easy reassignment during failover

---

### ✅ Why Elastic IP is Important

- Prevents IP change during instance restart.
- Supports production workloads.
- Simplifies DNS configuration.
- Enables disaster recovery scenarios.

---

### ✅ Association Process

Elastic IP can be associated with:
- EC2 instance
- Network interface

Once associated, the instance uses the Elastic IP as its public address.

---

### ✅ Cost Consideration

- Elastic IPs incur cost when not attached to a running instance.
- Proper cleanup is important to avoid unnecessary charges.

---

### ✅ Security Implications

- Public IP exposure increases attack surface.
- Security groups must be configured properly.
- Access should be restricted in production.

---

## ❌ Issues and Learning

- Verified region carefully before association.
- Confirmed correct instance and EIP selection.
- Ensured association status updated correctly.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to EC2 → Elastic IPs.
- Selected the required Elastic IP.
- Associated it with the target EC2 instance.
- Verified public IP assignment.

---

## 🔍 Verification Summary

- Elastic IP successfully attached.
- Instance public IP updated correctly.
- Connectivity verified.

---

## 💡 Key Takeaways

- Elastic IP ensures stable public connectivity.
- EIP reassignment supports high availability.
- Cost management is important for unused IPs.
- Security must be enforced on public endpoints.

---

## 🎤 Interview Preparation Notes

### Q1: What is an Elastic IP?
A static public IPv4 address in AWS.

---

### Q2: Why do we use Elastic IP instead of default public IP?
Default public IP changes on restart; Elastic IP remains constant.

---

### Q3: Can an Elastic IP be moved between instances?
Yes, it can be reassigned to another instance.

---

### Q4: Is Elastic IP free?
Free only when attached to a running instance; otherwise charged.

---

### Q5: Can Elastic IP be associated with multiple instances?
No, one Elastic IP can be associated with only one resource at a time.

---

## 📅 Progress Log

Day 10 : Elastic IP Attached to EC2 Instance  ✅ Completed

---

