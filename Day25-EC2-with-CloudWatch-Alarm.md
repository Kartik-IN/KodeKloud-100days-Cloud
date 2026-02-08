# ☁️ Day 25 – Setting Up an EC2 Instance with CloudWatch Alarm

---

## 📌 Task Overview

The objective of this task was to launch an EC2 instance and configure a CloudWatch alarm to monitor CPU utilization.  
If CPU usage crosses a defined threshold, a notification is sent using an existing SNS topic.

This setup represents a real-world monitoring and alerting workflow used in production systems.

---

## 🎯 Objectives

- Launch an EC2 instance named `datacenter-ec2`.
- Use an Ubuntu (Linux) AMI.
- Create a CloudWatch alarm named `datacenter-alarm`.
- Monitor CPU Utilization (Average).
- Trigger alarm when CPU ≥ 90% for one 5-minute period.
- Send alert notification to existing SNS topic `datacenter-sns-topic`.

---

## 🧠 Core Concepts

### ✅ Amazon EC2
Provides scalable compute capacity in the cloud for running applications.

---

### ✅ Amazon CloudWatch

CloudWatch is AWS monitoring service used to:
- Collect metrics
- Track performance
- Create alarms
- Trigger notifications

---

### ✅ CPU Utilization Metric

Shows percentage of CPU being used by an EC2 instance.

High CPU often indicates:
- Heavy workload
- Application overload
- Scaling requirement

---

### ✅ SNS (Simple Notification Service)

SNS delivers alarm notifications via:
- Email
- SMS
- Other AWS services

---

## 🛠️ Implementation Steps

---

### Step 1: Launch EC2 Instance

- Logged into AWS Console.
- Region: `us-east-1`
- Created EC2 instance:
  - Name: `datacenter-ec2`
  - AMI: Ubuntu Linux
- Waited until instance reached **Running** state.

---

### Step 2: Create CloudWatch Alarm

Navigated to:

CloudWatch → Alarms → Create Alarm

Configured:

- Metric: EC2 → Per-Instance Metrics → CPUUtilization
- Statistic: Average
- Threshold: ≥ 90%
- Period: 5 minutes (1 evaluation period)
- Alarm Name: `datacenter-alarm`

---

### Step 3: Configure Alarm Action

- Selected existing SNS topic:
datacenter-sns-topic


Alarm will publish notification to SNS when threshold is breached.

---

### Step 4: Finalize Alarm

- Reviewed configuration.
- Created alarm successfully.

---

## 🔍 Verification Summary

- EC2 instance created successfully.
- CloudWatch alarm configured.
- CPU threshold defined correctly.
- SNS topic attached.
- Alarm visible in CloudWatch dashboard.

---

## 💡 Key Takeaways

- CloudWatch enables proactive monitoring.
- Alarms help detect performance issues early.
- SNS integrates alerting into operations.
- Monitoring is essential for production readiness.

---

## 🎤 Interview Preparation

**Q1: What is CloudWatch?**  
AWS monitoring service for metrics, logs, and alarms.

---

**Q2: What is CPUUtilization metric?**  
Measures percentage of CPU used by EC2.

---

**Q3: Why use CloudWatch alarms?**  
To detect issues and trigger notifications automatically.

---

**Q4: What role does SNS play here?**  
SNS sends alarm notifications to subscribers.

---

**Q5: Why is monitoring important in DevOps?**  
It enables reliability, quick incident response, and system health visibility.

---

## 📅 Progress Log

Day 25 – EC2 Instance and CloudWatch Alarm Configured ✅ Completed
