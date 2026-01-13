# ☁️ Day 04 – Enable Versioning for S3 Bucket

## 📌 Challenge Description

The objective of this task was to enable versioning on an existing Amazon S3 bucket to improve data protection, recovery, and audit capabilities.

Versioning ensures that multiple versions of an object are preserved instead of being overwritten or permanently deleted.

---

## 🎯 Objective

- Identify the correct S3 bucket.
- Enable versioning on the bucket.
- Verify that versioning is active.
- Understand how versioning protects data.

---

## 🧠 Key Concepts Learned

### ✅ What is S3 Versioning?

S3 Versioning keeps multiple versions of the same object in a bucket.

If a file is:
- Uploaded again → A new version is created.
- Deleted → A delete marker is added.
- Overwritten → The previous version remains stored.

This allows recovery of older versions.

---

### ✅ Why Versioning Is Important

- Prevents accidental data loss.
- Enables rollback of files.
- Helps with compliance and auditing.
- Supports disaster recovery strategies.
- Protects against overwrites and deletions.

---

### ✅ Delete Marker Behavior

When an object is deleted in a versioned bucket:
- A delete marker hides the object.
- The original data still exists.
- Removing the delete marker restores the object.

---

### ✅ Versioning Lifecycle

Versioning can be:
- Enabled (default active state)
- Suspended (no new versions created)

Once enabled, it cannot be completely disabled.

---

### ✅ Storage Cost Consideration

Each version consumes storage.
Lifecycle policies should be used to manage old versions.

---

## ❌ Issues and Learning

- Carefully verified bucket name and region.
- Confirmed that versioning was enabled under the Properties tab.
- Understood the impact of storage cost when versioning is enabled.

---

## ✅ How the Task Was Completed

- Logged into AWS Console.
- Navigated to S3 service.
- Opened the required bucket.
- Enabled versioning from the Properties tab.
- Saved the configuration and verified status.

---

## 🔍 Verification Summary

- Correct bucket was selected.
- Versioning status shows Enabled.
- No configuration conflicts detected.

---

## 💡 Key Takeaways

- Versioning protects against accidental data loss.
- Deleted objects are not immediately removed.
- Version history enables recovery.
- Storage planning is important when versioning is enabled.

---

## 🎤 Interview Preparation Notes

### Q1: What is S3 versioning?
A feature that maintains multiple versions of objects to protect data.

---

### Q2: What happens when you delete an object in a versioned bucket?
A delete marker is added instead of permanent deletion.

---

### Q3: Can S3 versioning be disabled?
Versioning can be suspended but not fully disabled once enabled.

---

### Q4: Why is versioning useful for backups?
It allows restoring previous versions of data.

---

### Q5: Does versioning increase cost?
Yes, because multiple object versions consume storage.

---

## 📅 Progress Log

Day 04 : S3 Versioning Enabled  ✅ Completed

---

