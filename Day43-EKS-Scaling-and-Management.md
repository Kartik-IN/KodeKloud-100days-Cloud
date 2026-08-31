# ☁️ Day 43 – Amazon EKS Private Cluster Setup

---

## 📌 Task Overview

The Nautilus DevOps team required an Amazon EKS cluster configured for security and high availability.

The cluster was created using a **Custom configuration**, the latest stable Kubernetes version available during deployment, the specified IAM role, a private Kubernetes API endpoint, and the default VPC across three Availability Zones.

---

## 🏗️ AWS Configuration

- AWS Service: Amazon EKS
- Region: `us-east-1`
- Cluster Name: `datacenter-eks`
- Configuration Mode: `Custom`
- Kubernetes Version: Latest stable available
- Cluster IAM Role: `eksClusterRole`
- EKS Auto Mode: Disabled
- VPC: Default VPC
- Endpoint Access: Private

---

## 🌐 Network Configuration

The EKS cluster uses the **default VPC** with subnets distributed across three Availability Zones:

```text
Default VPC
│
├── us-east-1a
│
├── us-east-1b
│
└── us-east-1c
```

This provides availability across multiple Availability Zones.

---

## 🔐 Private Cluster Endpoint

The EKS Kubernetes API endpoint was configured as:

```text
Endpoint Access: Private
```

This prevents the Kubernetes API endpoint from being publicly accessible from the internet.

Architecture:

```text
              Default VPC
                   │
       ┌───────────┼───────────┐
       │           │           │
       ▼           ▼           ▼
   us-east-1a  us-east-1b  us-east-1c
       │           │           │
       └───────────┼───────────┘
                   │
                   ▼
             EKS Control Plane
                   │
             Private Endpoint
                   🔒
```

---

## 👤 IAM Role

The cluster uses the required IAM role:

```text
eksClusterRole
```

The role provides the permissions required by Amazon EKS to manage the Kubernetes control plane.

---

## ⚙️ EKS Auto Mode

EKS Auto Mode was explicitly disabled as required:

```text
EKS Auto Mode: Disabled
```

The cluster therefore uses the specified custom configuration instead of EKS Auto Mode.

---

## ☸️ Cluster Creation

The EKS cluster was created with:

```text
Cluster Name: datacenter-eks
Configuration: Custom
IAM Role: eksClusterRole
VPC: Default VPC
Availability Zones: us-east-1a, us-east-1b, us-east-1c
Endpoint: Private
EKS Auto Mode: Disabled
```

---

## ✅ Verification

After provisioning completed, the cluster was verified in the AWS EKS console.

Expected final status:

```text
datacenter-eks
Status: ACTIVE
```

The following configuration was verified:

- Cluster name `datacenter-eks`: ✅
- Custom configuration: ✅
- Latest stable Kubernetes version: ✅
- IAM role `eksClusterRole`: ✅
- EKS Auto Mode disabled: ✅
- Default VPC: ✅
- `us-east-1a` configured: ✅
- `us-east-1b` configured: ✅
- `us-east-1c` configured: ✅
- Kubernetes endpoint private: ✅
- Cluster status `ACTIVE`: ✅

---

## 🏆 Final Architecture

```text
                         AWS
                          │
                          ▼
                   Default VPC
                          │
          ┌───────────────┼───────────────┐
          │               │               │
          ▼               ▼               ▼
     us-east-1a      us-east-1b      us-east-1c
          │               │               │
          └───────────────┼───────────────┘
                          │
                          ▼
                  ┌───────────────┐
                  │ datacenter-eks│
                  │               │
                  │ Custom        │
                  │ Auto Mode OFF │
                  │ Private API   │
                  └───────────────┘
                          │
                          ▼
                   eksClusterRole
```

---

## 🎯 Key AWS Concepts Learned

- Amazon EKS cluster creation
- EKS custom configuration
- Kubernetes version selection
- EKS IAM cluster roles
- EKS Auto Mode
- Private Kubernetes API endpoints
- AWS VPC networking
- Availability Zones
- High-availability EKS control plane design
- EKS cluster verification

---

## 📝 Conclusion

The `datacenter-eks` Amazon EKS cluster was successfully provisioned in `us-east-1` using the **default VPC**, with networking across **us-east-1a, us-east-1b, and us-east-1c**.

The cluster uses the `eksClusterRole` IAM role, has **EKS Auto Mode disabled**, and exposes the Kubernetes API through a **private endpoint**.

```text
✅ datacenter-eks — ACTIVE
🔒 Private endpoint
☸️ Latest stable Kubernetes version
🌐 Default VPC
📍 3 Availability Zones
👤 eksClusterRole
⚙️ EKS Auto Mode disabled
```

---

## 📅 Cloud 100 Days Progress

**Day 43 – Amazon EKS Private Cluster Setup**

Status: ✅ **Completed**
