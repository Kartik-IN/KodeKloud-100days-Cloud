# ☁️ Day 03 – Create Subnet in Default VPC

## 📌 Challenge Description

The objective of this task was to create a subnet under the default VPC to support incremental cloud migration and proper network segmentation.

Subnets allow cloud resources to be organized into logical network boundaries.

---

## 🎯 Objective

- Create a subnet with a specific name.
- Associate the subnet with the default VPC.
- Ensure correct region placement.
- Validate subnet creation.

---

## 🧠 Key Concepts Learned

### ✅ What is a Subnet?

A subnet is a logical subdivision of a VPC network.

It defines a range of IP addresses where cloud resources such as EC2 instances are deployed.

---

### ✅ Why Subnets Are Important

- Network isolation and segmentation
- Improved security control
- High availability across availability zones
- Efficient IP address management
- Controlled routing and traffic flow

---

### ✅ Default VPC Usage

The default VPC provides a ready-to-use networking environment with predefined routing and internet access.

Using default VPC simplifies lab and testing environments.

---

### ✅ CIDR Block Understanding

A CIDR block defines the IP address range for a subnet.

The CIDR block must:
- Be within the VPC CIDR range
- Not overlap with other subnets

---

### ✅ Availability Zone Placement

Subnets belong to a single availability zone.

Distributing subnets across zones improves fault tolerance.

---

## ❌ Issues and Learning

- Verified region carefully before creation.
- Ensured CIDR range did not overlap existing subnets.
- Confirmed correct VPC selection.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to VPC service.
- Selected Subnets section.
- Created subnet under default VPC.
- Assigned valid CIDR block.
- Verified subnet availability.

---

## 🔍 Verification Summary

- Subnet exists with correct name.
- Associated with default VPC.
- Region validated.
- CIDR block verified.

---

## 💡 Key Takeaways

- Subnets organize cloud networks logically.
- Proper CIDR planning avoids conflicts.
- Region and VPC awareness is critical.
- Subnet design
