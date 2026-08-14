# ☁️ Day 30 – Configure a NAT Instance for Private EC2 Internet Access

---

## 📌 Task Overview

The Nautilus DevOps team needed to provide internet access to an EC2 instance running inside a private subnet.

To minimize costs, a **NAT Instance** was used instead of a managed NAT Gateway.

The private EC2 instance was already configured with a cron job that periodically uploads a test file to an S3 bucket. Successful appearance of the file in S3 confirms that internet access from the private subnet is working through the NAT Instance.

---

## 🎯 Objectives

- Use the existing VPC `devops-priv-vpc`.
- Use the existing private subnet `devops-priv-subnet`.
- Use the existing private EC2 instance `devops-priv-ec2`.
- Create a public subnet named `devops-pub-subnet`.
- Launch a NAT EC2 instance named `devops-nat-instance`.
- Use Amazon Linux 2023 for the NAT instance.
- Configure the instance as a NAT Instance using `iptables`.
- Enable IP forwarding.
- Disable EC2 Source/Destination Check.
- Configure route tables for private-to-public traffic.
- Verify internet access through an S3 upload.

---

# 🏗️ Architecture

```text
                         Internet
                            │
                            ▼
                   ┌─────────────────┐
                   │ Internet        │
                   │ Gateway         │
                   │ devops-igw      │
                   └────────┬────────┘
                            │
                            │
                    Public Subnet
                  devops-pub-subnet
                            │
                            ▼
                 ┌────────────────────┐
                 │ devops-nat-instance│
                 │                    │
                 │ Amazon Linux 2023  │
                 │ IP Forwarding      │
                 │ iptables NAT       │
                 └──────────┬─────────┘
                            │
                            │
                   Private Subnet
                  devops-priv-subnet
                            │
                            ▼
                  ┌──────────────────┐
                  │ devops-priv-ec2  │
                  │                  │
                  │ Private EC2      │
                  └────────┬─────────┘
                           │
                           ▼
                    Internet / S3
                           │
                           ▼
                  devops-nat-25627
                           │
                           ▼
                  devops-test.txt
