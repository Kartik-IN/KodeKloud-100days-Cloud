# ☁️ Day 22 – Configure Passwordless SSH Access to EC2 Instance

---

## 📌 Task Overview

The goal of this task was to securely access an EC2 instance from a client machine using **SSH key-based authentication**, without relying on passwords.

This setup is a core DevOps practice and is widely used in automation, CI/CD pipelines, and production environments.

---

## 🎯 Objectives

- Launch an EC2 instance using AWS Console (UI).
- Generate an SSH key pair on the client (aws-client).
- Copy the public key to the EC2 instance.
- Enable passwordless SSH login.
- Verify secure access.

---

## 🧠 Core Concepts Explained (Simple)

### 🔑 What is Passwordless SSH?

Passwordless SSH uses **asymmetric encryption**:
- A **private key** stays on the client machine.
- A **public key** is placed on the server.
- SSH validates the key pair and allows access without passwords.

This is more secure and automation-friendly than password-based login.

---

## 📂 SSH Directory Structure

~/.ssh/
├── id_rsa (Private Key – stays on client)
├── id_rsa.pub (Public Key – copied to server)
└── authorized_keys (Server-side list of allowed public keys)


---

## 🛠️ Step-by-Step Implementation

### 🔹 Step 1: Launch EC2 Instance (AWS Console)

- Login to AWS Console.
- Region: `us-east-1`
- Launch a Linux EC2 instance.
- Instance name: `datacenter-ec2`
- Instance type: `t2.micro`
- Allow SSH (port 22) in Security Group.
- Launch instance.
- Copy **Public IPv4 address**.

---

### 🔹 Step 2: Generate SSH Key on Client Machine

On the **aws-client** host:

```bash
sudo -i
cd /root/.ssh
If directory doesn’t exist:

mkdir -p /root/.ssh
chmod 700 /root/.ssh
Generate SSH key:

ssh-keygen -t rsa -b 2048 -f /root/.ssh/id_rsa -N ""
This creates:

id_rsa (private key)

id_rsa.pub (public key)

🔹 Step 3: Copy Public Key to EC2
Display public key:

cat /root/.ssh/id_rsa.pub
Copy the entire output.

SSH into EC2 (initial access):

ssh root@<EC2_PUBLIC_IP>
On EC2, configure SSH:

mkdir -p /root/.ssh
chmod 700 /root/.ssh
nano /root/.ssh/authorized_keys
Paste the public key → save → exit.

Set correct permissions:

chmod 600 /root/.ssh/authorized_keys
Exit EC2:

exit
🔹 Step 4: Verify Passwordless SSH
From aws-client:

ssh root@<EC2_PUBLIC_IP>
✅ Login happens without password
➡️ Passwordless SSH is successfully configured.

🔍 Verification Checklist
SSH key pair created ✔️

Public key copied to EC2 ✔️

Correct permissions set ✔️

Passwordless login verified ✔️

💡 Key Takeaways
SSH keys are more secure than passwords.

File permissions are critical for SSH.

Passwordless SSH enables automation.

This setup is standard in real DevOps workflows.

🎤 Interview Preparation (Very Important)
Q1: How does passwordless SSH work?
Public key is stored on the server; private key stays on the client. SSH validates them during login.

Q2: Where is the public key stored on server?
~/.ssh/authorized_keys

Q3: Required permissions?

.ssh → 700

authorized_keys → 600

Q4: Why passwordless SSH is used in DevOps?
For secure automation, CI/CD, and remote access without manual input.
