# ☁️ Day 38 – Deploying Containerized Applications with Amazon ECS

---

## 📌 Project Overview

This task demonstrates how to deploy a containerized Nginx application using **Amazon Elastic Container Registry (ECR)** and **Amazon Elastic Container Service (ECS) with AWS Fargate**.

The Docker image was built from the provided Dockerfile, pushed to a private ECR repository, and deployed as a running ECS service.

---

## 🏗️ Architecture

```text
                    AWS Client
                        │
                        │ Docker Build
                        ▼
                 Docker Image
                        │
                        │ Push
                        ▼
              ┌──────────────────┐
              │   Amazon ECR     │
              │   nautilus-ecr   │
              │      :latest     │
              └────────┬─────────┘
                       │
                       │ Pull Image
                       ▼
              ┌──────────────────┐
              │   ECS Fargate    │
              │                  │
              │ nautilus-cluster │
              │       │          │
              │       ▼          │
              │ nautilus-service │
              │       │          │
              │       ▼          │
              │  Fargate Task    │
              │    Nginx :80     │
              └──────────────────┘
```

---

## 🛠️ Resources Created

- AWS Region: `us-east-1`
- ECR Repository: `nautilus-ecr`
- ECR Visibility: Private
- Docker Image Tag: `latest`
- ECS Cluster: `nautilus-cluster`
- Launch Type: Fargate
- Task Definition: `nautilus-taskdefinition`
- ECS Service: `nautilus-service`
- Desired Tasks: `1`
- Container Port: `80`
- Application: Nginx

---

## 🐳 Docker Configuration

The Dockerfile is located at:

```text
/root/pyapp/Dockerfile
```

Dockerfile:

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

The image uses **Nginx Alpine** as the base image and serves the provided `index.html` file on port `80`.

---

## 📦 Build the Docker Image

Navigate to the application directory:

```bash
cd /root/pyapp
```

Build the image:

```bash
docker build -t nautilus-ecr:latest .
```

Verify the image:

```bash
docker images
```

---

## 🔐 Authenticate with Amazon ECR

Authenticate Docker with the private ECR registry:

```bash
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin \
  203777982394.dkr.ecr.us-east-1.amazonaws.com
```

Expected output:

```text
Login Succeeded
```

---

## 🏷️ Tag the Docker Image

```bash
docker tag nautilus-ecr:latest \
  203777982394.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```

---

## ⬆️ Push the Image to ECR

```bash
docker push \
  203777982394.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```

The `latest` image can then be verified from:

```text
Amazon ECR
    └── nautilus-ecr
          └── latest
```

---

## ☁️ ECS Configuration

### ✅ Create the ECS Cluster

Created an ECS cluster:

```text
nautilus-cluster
```

using:

```text
Launch Type: AWS Fargate
```

---

### ✅ Create the Task Definition

Created:

```text
nautilus-taskdefinition
```

The task definition uses:

```text
Image:
203777982394.dkr.ecr.us-east-1.amazonaws.com/nautilus-ecr:latest
```

Container configuration:

```text
Container Port: 80
Protocol: TCP
Application Protocol: HTTP
```

CPU and memory were configured using suitable Fargate resources.

---

## 🚀 Create the ECS Service

Created the service:

```text
nautilus-service
```

under:

```text
nautilus-cluster
```

Configuration:

```text
Launch Type: FARGATE
Desired Tasks: 1
```

The service was verified to have:

```text
Desired: 1
Running: 1
Pending: 0
```

---

## ✅ Verification

Final deployment state:

```text
ECS Cluster
└── nautilus-cluster
    └── nautilus-service
        └── Fargate Task
            ├── Status: RUNNING
            ├── Container: Nginx
            └── Port: 80
```

The ECS service deployment completed successfully with at least one running Fargate task.

---

## 🔧 Troubleshooting

### ECS Task Not Starting

Check the following:

- The ECR image exists
- The image URI is correct
- The ECS task execution role has permission to pull from ECR
- The subnet has appropriate network connectivity
- The security group allows required traffic

---

### Application Not Accessible

Verify:

```text
Container port = 80
```

Ensure that the ECS task's security group permits HTTP traffic on port `80`.

Also verify that the task is in:

```text
RUNNING
```

state.

---

## 🎯 Result

Successfully deployed a containerized Nginx application using:

```text
Docker → Amazon ECR → ECS Fargate → ECS Service
```

The application image is stored privately in `nautilus-ecr` and deployed through `nautilus-service` with **one running Fargate task**.

---

## 📚 AWS Services & Concepts Practiced

- Docker
- Amazon ECR
- Amazon ECS
- AWS Fargate
- ECS Clusters
- ECS Task Definitions
- ECS Services
- Container Images
- IAM Task Execution Roles
- Security Groups
- Nginx

---

## 📅 Cloud 100 Days Progress

**Day 38 – Deploying Containerized Applications with Amazon ECS**

Status: ✅ **Completed**
