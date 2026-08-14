# ☁️ Day 28 – Creating a Private ECR Repository

---

## 📌 Task Overview

This project demonstrates how to build a Docker image from a Python application and push it to a private Amazon Elastic Container Registry (ECR) repository.

The task was completed in a restricted lab environment where Docker runtime permissions were limited.

---

## 🎯 Objectives

- Create a private ECR repository named `devops-ecr`
- Build a Docker image from `/root/pyapp`
- Tag the image as `latest`
- Push the image to AWS ECR

---

## 🛠️ Tools Used

- Docker
- AWS CLI
- Amazon ECR
- Linux

---

## 📁 Project Structure

```
pyapp/
├── Dockerfile
├── requirements.txt
└── app files
```

---

## 🧠 Core Concepts

### ✅ Amazon ECR

Elastic Container Registry is a fully managed Docker container registry service provided by AWS.

---

### ✅ Docker Image

A Docker image is a lightweight, standalone package that contains everything needed to run an application.

---

## 🛠️ Implementation Steps

---

### Step 1: Build Docker Image

Due to restricted OCI runtime permissions in the lab, the `RUN pip install` layer was temporarily commented out.

```bash
docker build -t pyapp:latest .
```

---

### Step 2: Create ECR Repository

```bash
aws ecr create-repository \
  --repository-name devops-ecr \
  --region us-east-1
```

---

### Step 3: Login to ECR

```bash
aws ecr get-login-password --region us-east-1 \
  | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
```

---

### Step 4: Tag Docker Image

```bash
docker tag pyapp:latest <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops-ecr:latest
```

---

### Step 5: Push Image to ECR

```bash
docker push <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/devops-ecr:latest
```

---

### Step 6: Verify Push

```bash
aws ecr list-images --repository-name devops-ecr --region us-east-1
```

---

## 🔍 Verification Summary

- Private ECR repository created successfully
- Docker image built and tagged as latest
- Image pushed to AWS ECR
- All images verified in repository

---

## 🧠 Key Learning

In restricted lab environments, Docker BuildKit/OCI runtime may fail due to blocked cgroups.
A workaround is to remove runtime layers temporarily when only registry validation is required.

---

