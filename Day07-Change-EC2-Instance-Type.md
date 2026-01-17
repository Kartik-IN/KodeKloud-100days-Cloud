# ☁️ Day 07 – Change EC2 Instance Type

## 📌 Challenge Description

The objective of this task was to modify the instance type of an existing EC2 instance to match updated performance or cost requirements.

Changing instance types allows scaling compute resources without rebuilding infrastructure.

---

## 🎯 Objective

- Identify the target EC2 instance.
- Stop the instance safely.
- Change the instance type.
- Restart the instance and verify changes.

---

## 🧠 Key Concepts Learned

### ✅ What is an EC2 Instance Type?

An instance type defines the hardware configuration of a virtual machine:
- CPU cores
- Memory capacity
- Network performance
- Pricing model

Choosing the correct instance type ensures optimal performance and cost efficiency.

---

### ✅ Why Change Instance Type?

- Scale up for higher workloads.
- Scale down to reduce cost.
- Test performance scenarios.
- Adjust resource utilization.

---

### ✅ Instance State Requirement

To change instance type:
- The instance must be stopped.
- Configuration changes are applied while stopped.
- Instance must be restarted afterward.

---

### ✅ Impact on Public IP

Stopping and starting an instance may change:
- Public IP address (unless Elastic IP is attached).

Private IP remains unchanged.

---

### ✅ Compatibility Considerations

Some instance types require:
- Specific AMI compatibility
- Supported virtualization type
- Availability in the region/AZ

---

## ❌ Issues and Learning

- Ensured instance was fully stopped before modification.
- Verified new instance type compatibility.
- Observed public IP reassignment behavior.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to EC2 Instances.
- Selected the instance.
- Stopped the instance.
- Modified instance type.
- Started the instance again.
- Verified updated configuration.

---

## 🔍 Verification Summary

- Instance type updated successfully.
- Instance restarted without errors.
- Application accessibility confirmed.

---

## 💡 Key Takeaways

- Instance scaling is flexible in cloud environments.
- Stopping is mandatory for configuration changes.
- IP behavior must be considered.
- Right-sizing saves cost.

---

## 🎤 Interview Preparation Notes

### Q1: Why must an EC2 instance be stopped before changing instance type?
Because hardware configuration cannot be modified while running.

---

### Q2: What happens to public IP after stopping an instance?
It may change unless Elastic IP is attached.

---

### Q3: Can all instance types be changed freely?
No, compatibility and availability must be considered.

---

### Q4: What is right-sizing?
Selecting optimal resource size to balance performance and cost.

---

### Q5: Does changing instance type affect data on EBS?
No, EBS data remains intact.

---

## 📅 Progress Log

Day 07 : EC2 Instance Type Changed  ✅ Completed

---

