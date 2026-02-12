# ☁️ Day 29 – VPC Peering Between Public and Private VPCs

---

## 📌 Task Overview

The objective of this task was to establish communication between two VPCs using **VPC Peering**.

A public EC2 instance located in the default VPC needed to communicate with a private EC2 instance located inside a private VPC. This was achieved by creating a VPC peering connection and updating route tables and security groups accordingly.

---

## 🎯 Objectives

- Create VPC peering connection `devops-vpc-peering`
- Connect Default VPC with `devops-private-vpc`
- Configure route tables on both VPCs
- Ensure communication between:
  - `devops-public-ec2`
  - `devops-private-ec2`
- Configure SSH access to public EC2
- Ping private EC2 from public EC2

---

## 🧠 Core Concepts

### ✅ VPC Peering

VPC Peering allows private network connectivity between two VPCs using AWS backbone infrastructure.

---

### ✅ Public vs Private VPC

- Public VPC: Has internet access
- Private VPC: No direct internet access

Peering allows them to communicate securely.

---

### ✅ Route Tables

Routing rules tell AWS where traffic should go.

Without route updates, peering will not work.

---

### ✅ Security Groups

Must allow traffic explicitly (ICMP / SSH).

---

## 🛠️ Existing Resources

### Public Side
- EC2: `devops-public-ec2`
- VPC: Default

### Private Side
- VPC: `devops-private-vpc`
- CIDR: `10.1.0.0/16`
- Subnet: `devops-private-subnet` (`10.1.1.0/24`)
- EC2: `devops-private-ec2`

---

## 🛠️ Implementation Steps

---

### Step 1: Create VPC Peering Connection

- VPC → Peering Connections → Create
- Name: `devops-vpc-peering`
- Requester VPC: Default VPC
- Accepter VPC: `devops-private-vpc`
- Accepted peering request.

---

### Step 2: Update Route Tables

#### Default VPC Route Table

Added:

Destination:
10.1.0.0/16


Target:
devops-vpc-peering


---

#### Private VPC Route Table

Added:

Destination:
Default VPC CIDR


Target:
devops-vpc-peering


---

### Step 3: Configure Security Groups

#### Private EC2 Security Group

Allowed inbound:

- ICMP (All)
- Source: Default VPC CIDR

---

### Step 4: Configure SSH Access to Public EC2

From aws-client:

```bash
cat /root/.ssh/id_rsa.pub
```
Copied public key.

Logged into public EC2 and added to:

nano /home/ec2-user/.ssh/authorized_keys
Fixed permissions:

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
Step 5: Test Connectivity
SSH into public EC2:

ssh ec2-user@<PUBLIC_EC2_IP>
Ping private EC2:

ping <PRIVATE_EC2_PRIVATE_IP>
✅ Ping successful.

🔍 Verification Summary
VPC peering created and active.

Route tables updated on both VPCs.

Security groups configured.

SSH access to public EC2 works.

Private EC2 reachable via ping.

💡 Key Takeaways
VPC Peering enables private communication between VPCs.

Route tables are mandatory for connectivity.

Security groups control traffic flow.

No transitive peering supported.

Used for multi-VPC architectures.

🎤 Interview Preparation
Q1: What is VPC Peering?
Private connectivity between two VPCs.

Q2: Does VPC Peering support transitive routing?
No.

Q3: What must be configured for peering to work?
Route tables + security groups.

Q4: Is traffic encrypted?
Yes, AWS backbone encryption.

Q5: Use cases for VPC Peering?
Multi-tier apps, shared services, isolated environments.

📅 Progress Log
Day 29 – VPC Peering Between Public and Private EC2 Instances ✅ Completed
