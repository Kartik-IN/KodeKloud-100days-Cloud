# ☁️ Day 22 – Configure Passwordless SSH Access to EC2 Instance

---

## 📌 Challenge Description

The objective of this task was to securely access an EC2 instance from a client machine using **SSH key-based authentication**, without relying on passwords.

This setup is a core DevOps practice and is widely used in automation, CI/CD pipelines, and production environments.

---

## 🎯 Objective

- Launch an EC2 instance using AWS Console (UI).
- Generate an SSH key pair on the client (aws-client).
- Copy the public key to the EC2 instance.
- Enable passwordless SSH login.
- Verify secure access.

---

## 🧠 Key Concepts Learned

### ✅ What is Passwordless SSH?

Passwordless SSH uses **asymmetric encryption**:
- A **private key** stays on the client machine.
- A **public key** is placed on the server.
- SSH validates the key pair and allows access without passwords.

This is more secure and automation-friendly than password-based login.

---

### ✅ SSH Directory Structure

The SSH configuration directory structure:

```
~/.ssh/
├── id_rsa (Private Key – stays on client)
├── id_rsa.pub (Public Key – copied to server)
└── authorized_keys (Server-side list of allowed public keys)
```

---

### ✅ SSH Key Pair Authentication

A key pair consists of two parts that work together for secure authentication.

---

### ✅ Security Best Practices

- Keep private key permissions restrictive (600)
- Keep .ssh directory permissions restricted (700)
- Never share private keys
- Use strong key algorithms (RSA 2048+ or ED25519)

---

## ❌ Issues and Learning

- Verified SSH key permissions were set correctly.
- Ensured authorized_keys file had proper permissions.
- Confirmed passwordless login without password prompts.

---

## ✅ How the Task Was Completed

### Step 1: Launch EC2 Instance (AWS Console)

- Login to AWS Console.
- Region: `us-east-1`
- Launch a Linux EC2 instance.
- Instance name: `datacenter-ec2`
- Instance type: `t2.micro`
- Allow SSH (port 22) in Security Group.
- Launch instance.
- Copy **Public IPv4 address**.

### Step 2: Generate SSH Key on Client Machine

On the **aws-client** host:

```bash
sudo -i
cd /root/.ssh
mkdir -p /root/.ssh
chmod 700 /root/.ssh
ssh-keygen -t rsa -b 2048 -f /root/.ssh/id_rsa -N ""
```

This creates:
- `id_rsa` (private key)
- `id_rsa.pub` (public key)

### Step 3: Copy Public Key to EC2

Display public key:

```bash
cat /root/.ssh/id_rsa.pub
```

Copy the entire output.

SSH into EC2 (initial access):

```bash
ssh root@<EC2_PUBLIC_IP>
```

On EC2, configure SSH:

```bash
mkdir -p /root/.ssh
chmod 700 /root/.ssh
nano /root/.ssh/authorized_keys
```

Paste the public key, save, and exit.

Set correct permissions:

```bash
chmod 600 /root/.ssh/authorized_keys
exit
```

### Step 4: Verify Passwordless SSH

From aws-client:

```bash
ssh root@<EC2_PUBLIC_IP>
```

✅ Login happens without password.

---

## 🔍 Verification Summary

- SSH key pair created successfully.
- Public key copied to EC2 authorized_keys.
- Correct permissions set on SSH directories and files.
- Passwordless login verified and working.

---

## 💡 Key Takeaways

- SSH keys provide more secure authentication than passwords.
- File permissions are critical for SSH functionality.
- Passwordless SSH enables automation and DevOps workflows.
- Key-based authentication is the standard in production environments.

---

## 🎤 Interview Preparation Notes

### Q1: How does passwordless SSH work?
Public key is stored on the server in ~/.ssh/authorized_keys; private key stays on the client. SSH validates them during login.

---

### Q2: Where is the public key stored on the server?
In the ~/.ssh/authorized_keys file.

---

### Q3: What are the required permissions?
- .ssh directory: 700 (rwx------)
- authorized_keys file: 600 (rw-------)

---

### Q4: Why is passwordless SSH used in DevOps?
For secure automation, CI/CD pipelines, and remote access without manual password input.

---

### Q5: What key algorithm is recommended?
RSA (2048-bit or higher) or ED25519 for better performance and security.

---

## 📅 Progress Log

Day 22 : Configure Passwordless SSH Access to EC2  ✅ Completed

---
