# ☁️ Day 31 – Provision a Private MySQL RDS Instance

## 📌 Task Overview

The Nautilus Development Team required a reliable and scalable database solution for application development and testing.

The task was to provision a **private Amazon RDS instance** using the AWS Free Tier with **MySQL 8.4.x**.

The database needed to be configured with cost-effective resources while maintaining the required storage and availability settings.

---

## 🎯 Objectives

The following requirements were completed:

- Create a private RDS instance named `devops-rds`
- Use the **Full configuration** database creation method
- Select the **Free tier** template
- Use **MySQL 8.4.x**
- Use instance type `db.t3.micro`
- Use `General Purpose SSD (gp2)`
- Allocate `20 GiB` storage
- Enable storage autoscaling
- Set maximum storage threshold to `22 GiB`
- Disable public access
- Ensure the RDS instance reaches the `Available` state

---

# 🏗️ Architecture

```text
                    AWS VPC
                       │
                       │
              ┌────────▼────────┐
              │   Private RDS   │
              │                 │
              │   devops-rds    │
              │                 │
              │    MySQL 8.4    │
              │   db.t3.micro   │
              │                 │
              │    20 GiB gp2    │
              │                 │
              │ Public Access    │
              │      No          │
              └─────────────────┘
                       ▲
                       │
                Private Network
                       │
              Application Servers
