# ☁️ Day 40 – AWS VPC Troubleshooting: Internet Access

---

## 📌 Task Overview

The Nautilus DevOps team had an EC2 instance running an Nginx web server inside the `devops-vpc`. Although the security group allowed HTTP traffic on port `80`, the application was not accessible from the internet.

The issue was identified as a **detached Internet Gateway**. The Internet Gateway was reattached to the VPC, restoring internet connectivity.

---

## 🏗️ Environment

- VPC: `devops-vpc`
- EC2 Instance: `devops-ec2`
- Security Group: `devops-sg`
- Web Server: Nginx
- Application Port: `80`
- Internet Access: Internet Gateway
- Region: `us-east-1`

---

## 🔍 Problem

The EC2 instance and security group were already configured for HTTP access, but the application could not be reached externally.

The investigation focused on the VPC networking path:

```text
Internet
   │
   ▼
Internet Gateway
   │
   ▼
VPC
   │
   ▼
Public Subnet
   │
   ▼
Route Table
   │
   ▼
EC2 Instance
   │
   ▼
Nginx :80
```

The **Internet Gateway was detached from `devops-vpc`**, preventing the VPC from communicating with the internet.

---

## 🛠️ Troubleshooting Steps

### Step 1: Verify the VPC

Located:

```text
devops-vpc
```

Verified that the EC2 instance was deployed within this VPC.

---

### Step 2: Check the Internet Gateway

Navigated to:

```text
VPC → Internet Gateways
```

The Internet Gateway was found to be **detached** from `devops-vpc`.

This was identified as the root cause.

---

### Step 3: Reattach the Internet Gateway

Selected the Internet Gateway and used:

```text
Actions
→ Attach to a VPC
```

Selected:

```text
devops-vpc
```

After attaching it, the gateway showed:

```text
State: Attached
```

---

### Step 4: Verify the Route Table

The EC2 subnet's route table was checked to ensure internet traffic had a route through the Internet Gateway.

Required routes:

```text
Destination        Target
--------------------------------
VPC CIDR           local
0.0.0.0/0          Internet Gateway
```

The `0.0.0.0/0` route allows internet-bound traffic to leave through the Internet Gateway.

---

### Step 5: Verify the Security Group

The security group:

```text
devops-sg
```

was checked for an HTTP inbound rule:

```text
Type:     HTTP
Protocol: TCP
Port:     80
Source:   0.0.0.0/0
```

This allows external HTTP traffic to reach the EC2 instance.

---

### Step 6: Verify Nginx

The Nginx service was checked on the EC2 instance:

```bash
sudo systemctl status nginx
```

Nginx was running and listening on port `80`.

Local testing can be performed with:

```bash
curl http://localhost
```

---

## ✅ Resolution

The root cause was:

```text
Internet Gateway
      ↓
Detached from devops-vpc ❌
```

After reattaching:

```text
Internet
   │
   ▼
Internet Gateway
   │
   ▼
devops-vpc
   │
   ▼
Public Subnet
   │
   ▼
devops-ec2
   │
   ▼
Nginx :80
```

The application became accessible from the internet.

---

## 🎯 Final Verification

- `devops-vpc` verified: ✅
- Internet Gateway attached: ✅
- Route table provides internet route: ✅
- `devops-ec2` has internet connectivity: ✅
- `devops-sg` allows HTTP port `80`: ✅
- Nginx running: ✅
- Application accessible externally: ✅

---

## 💡 Key Learning

For an EC2 instance in a public VPC to be reachable from the internet, having an open security group rule is not enough.

The complete path must be correctly configured:

```text
Public IP
   +
Security Group
   +
Public Subnet
   +
Route Table
   +
0.0.0.0/0 → Internet Gateway
   +
Internet Gateway attached to VPC
```

In this task, the security group was already configured correctly. The actual issue was the **detached Internet Gateway**.

---

## 🧠 AWS Concepts Practiced

- Amazon VPC
- Internet Gateways
- Route Tables
- Public Subnets
- EC2 Internet Connectivity
- Security Groups
- Nginx
- HTTP port `80`
- VPC troubleshooting
- Network path analysis

---

## 📅 Cloud 100 Days Progress

**Day 40 – AWS VPC Troubleshooting: Internet Access**

Status: ✅ **Completed**
