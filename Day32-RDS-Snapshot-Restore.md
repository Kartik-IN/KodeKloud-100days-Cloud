# ☁️ Day 32 – RDS Snapshot and Restore

---

## 📌 Task Overview

The Nautilus DevOps team needed to safely test a major update to their database infrastructure.

To protect the existing RDS database and validate the backup process, a snapshot of the existing RDS instance was created and then restored into a new RDS instance.

This demonstrates an important DevOps practice:

```text
Backup → Restore → Validate
```

---

## 🎯 Objectives

The following requirements were completed:

- Take a snapshot of the existing RDS instance `devops-rds`
- Name the snapshot `devops-snapshot`
- Restore the snapshot into a new RDS instance
- Name the restored instance `devops-snapshot-restore`
- Configure the restored instance with `db.t3.micro`
- Verify that the restored RDS instance reaches the `Available` state

---

## 🏗️ Architecture

```text
                Existing RDS
              ┌──────────────┐
              │  devops-rds  │
              │              │
              │ MySQL 8.4.x  │
              └──────┬───────┘
                     │
                     │ Take Snapshot
                     ▼
             ┌─────────────────┐
             │ devops-snapshot │
             │                 │
             │     Backup      │
             └────────┬────────┘
                      │
                      │ Restore
                      ▼
       ┌──────────────────────────────┐
       │  devops-snapshot-restore     │
       │                              │
       │  Instance: db.t3.micro       │
       │                              │
       │  Status: Available ✅        │
       └──────────────────────────────┘
```

---

## ☁️ AWS Resources

- RDS Instance: `devops-rds`
- RDS Snapshot: `devops-snapshot`
- Restored RDS Instance: `devops-snapshot-restore`
- Restored Instance Class: `db.t3.micro`

---

## 🛠️ Implementation

### Step 1: Verify Existing RDS Instance

Navigated to:

```text
RDS → Databases
```

Selected:

```text
devops-rds
```

Before creating the snapshot, verified that the RDS instance was in:

```text
Available
```

state.

---

### Step 2: Create RDS Snapshot

Selected the existing RDS instance:

```text
devops-rds
```

Then:

```text
Actions → Take snapshot
```

Snapshot identifier:

```text
devops-snapshot
```

The snapshot was created successfully.

---

### Step 3: Wait for Snapshot Availability

Navigated to:

```text
RDS → Snapshots → Manual snapshots
```

Waited for:

```text
devops-snapshot
```

to become:

```text
Available
```

Only after the snapshot became available was it used for restoration.

---

### Step 4: Restore Snapshot

Selected:

```text
devops-snapshot
```

Then:

```text
Actions → Restore snapshot
```

Configured the new DB instance identifier as:

```text
devops-snapshot-restore
```

---

### Step 5: Configure Instance Class

During restoration, configured the DB instance class as:

```text
db.t3.micro
```

This satisfies the required instance configuration for the restored database.

---

### Step 6: Database Configuration

The restored RDS instance was configured using the required networking and database settings.

The purpose of the restored instance is to provide a separate environment for testing and validation without modifying the original:

```text
devops-rds
```

---

### Step 7: Wait for Restoration

After starting the restore process, the new RDS instance initially showed a provisioning state.

AWS then completed the restoration process.

The final required state was:

```text
Available
```

The restored database reached the required:

```text
devops-snapshot-restore
        ↓
db.t3.micro
        ↓
Available ✅
```

---

## 🔍 Verification

- Source RDS `devops-rds` available: ✅
- Snapshot created: ✅
- Snapshot name `devops-snapshot`: ✅
- Snapshot restored: ✅
- Restored DB `devops-snapshot-restore`: ✅
- Instance class `db.t3.micro`: ✅
- Restored instance `Available`: ✅

---

## 🧠 Key Concepts Learned

### ✅ RDS Snapshot

An RDS snapshot is a backup of an RDS database that can be used to restore the database later.

---

### ✅ Backup and Restore

The workflow used in this task was:

```text
Running RDS
     ↓
Create Snapshot
     ↓
Snapshot Available
     ↓
Restore Snapshot
     ↓
New RDS Instance
     ↓
Available
```

---

### ✅ Why Restore a Snapshot?

Restoring a snapshot allows teams to:

- Test backups
- Validate disaster recovery
- Create test environments
- Perform database testing
- Recover from failures
- Validate application compatibility

---

## 🆚 Snapshot vs RDS Instance

- RDS Instance: running database, can be connected to, consumes compute resources
- RDS Snapshot: backup of the database, used for restoration, no active instance class

---

## 🎯 Real-World DevOps Use Case

A production database may need a major upgrade.

Instead of directly experimenting with production:

```text
Production RDS
     │
     ▼
Create Snapshot
     │
     ▼
Restore Snapshot
     │
     ▼
Test RDS
     │
     ▼
Test Application
     │
     ▼
Validate
     │
     ▼
Production Update
```

This reduces the risk of making changes directly to production.

---

## 🎤 Interview Questions

### 1. What is an RDS snapshot?

An RDS snapshot is a backup of an RDS database that can be used to restore a database instance.

### 2. Why take an RDS snapshot before a major update?

It provides a recovery point that can be used to restore the database if the update causes problems.

### 3. Can you connect directly to an RDS snapshot?

No. A snapshot is a backup. It must be restored into an RDS instance before applications can connect to the restored database.

### 4. What is the difference between snapshot and restore?

Snapshot creates a backup of the database, while restore uses that backup to create a new RDS database instance.

### 5. Why restore a snapshot into another RDS instance?

It allows teams to test the backup, validate data recovery, and create a separate testing environment without affecting the original database.

### 6. What does the `Available` state mean?

It means the RDS instance has finished provisioning and is ready for use.

---

## 🔐 Best Practices

- Take snapshots before major database changes
- Test backups by performing actual restores
- Keep production and testing databases separate
- Protect database credentials
- Do not store database passwords in GitHub
- Verify restored databases before relying on them for disaster recovery
- Regularly test the recovery process rather than assuming backups work

---

## 📚 AWS Services & Concepts Practiced

- Amazon RDS
- RDS Snapshots
- RDS Restore
- MySQL
- Database Backup
- Disaster Recovery
- Database Testing
- AWS Console
- RDS Instance Classes

---

## 📅 Cloud 100 Days Progress

**Day 32 – RDS Snapshot and Restore**

Status: ✅ **Completed**

---

## 🚀 Learning Outcome

This task provided hands-on experience with the AWS RDS backup and recovery workflow.

The complete process was:

```text
devops-rds
     │
     ▼
devops-snapshot
     │
     ▼
devops-snapshot-restore
     │
     ▼
db.t3.micro
     │
     ▼
Available ✅
```

The exercise demonstrated how RDS snapshots can be used to create a recovery point and restore a database into a separate instance for testing and validation.

---

## ⭐ Summary

Successfully:

```text
Created Snapshot:
devops-snapshot

Restored RDS:
devops-snapshot-restore

Instance Class:
db.t3.micro

Final Status:
Available ✅
```

This completes **Cloud Day 32**. 🎯
