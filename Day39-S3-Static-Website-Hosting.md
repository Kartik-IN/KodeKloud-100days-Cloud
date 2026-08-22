# ☁️ Day 39 – S3 Static Website Hosting

---

## 📌 Task Overview

This task demonstrates how to host a static website using an Amazon S3 bucket.

An `index.html` file was uploaded to a public S3 bucket, static website hosting was enabled, and a bucket policy was configured to allow public read access.

> S3 website endpoints use **HTTP**, not HTTPS, for classic static website hosting.

---

## 🎯 Objectives

- Create an S3 bucket named `datacenter-web-286272606`
- Use the `us-east-1` AWS region
- Upload `/root/index.html`
- Enable static website hosting
- Configure `index.html` as the index document
- Allow public read access to objects
- Verify the website through the S3 website endpoint

---

## 🏗️ Architecture

```text
Internet
   │
   ▼
S3 Static Website
   │
   ▼
datacenter-web-286272606
   │
   └── index.html
```

---

## ☁️ AWS Resources

- S3 Bucket: `datacenter-web-286272606`
- Region: `us-east-1`
- Website File: `index.html`
- Hosting Type: Static website hosting
- Public Access: Enabled for object reads

---

## 🛠️ Implementation

### Step 1: Create the S3 Bucket

Navigate to:

```text
AWS Console → S3 → Buckets → Create bucket
```

Configure the bucket with:

```text
Bucket name: datacenter-web-286272606
Region: us-east-1
```

Keep the other settings at their defaults except for Block Public Access.

Under **Block Public Access**, uncheck:

```text
Block all public access
```

Acknowledge the warning that the current settings might result in the bucket and objects becoming public, then select **Create bucket**.

---

### Step 2: Upload `index.html`

On the AWS client terminal, verify that the file exists:

```bash
ls -l /root/index.html
```

Upload the file to S3:

```bash
aws s3 cp /root/index.html \
  s3://datacenter-web-286272606/index.html \
  --region us-east-1
```

Verify the uploaded object:

```bash
aws s3 ls s3://datacenter-web-286272606/ --region us-east-1
```

Expected output includes:

```text
index.html
```

---

### Step 3: Enable Static Website Hosting

Navigate to:

```text
S3 → datacenter-web-286272606 → Properties
```

Find **Static website hosting** and select **Edit**.

Configure:

```text
Static website hosting: Enable
Hosting type: Host a static website
Index document: index.html
```

Leave the error document empty unless the task requires one, then select **Save changes**.

---

### Step 4: Add the Bucket Policy

Navigate to:

```text
S3 → datacenter-web-286272606 → Permissions
```

Find **Bucket policy**, select **Edit**, and add the following policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::datacenter-web-286272606/*"
    }
  ]
}
```

Save the policy.

This policy allows anyone to read objects from the bucket.

---

### Step 5: Check Public Access Block

Return to:

```text
Permissions → Block public access
```

Verify that bucket-level blocking is not preventing the public policy.

The expected setting is:

```text
Block all public access: OFF
```

If it is still enabled, select **Edit**, disable it, acknowledge the warning, and save the changes.

---

### Step 6: Get the Website URL

Navigate to:

```text
Properties → Static website hosting
```

Copy the **Bucket website endpoint**.

The URL will look similar to:

```text
http://datacenter-web-286272606.s3-website-us-east-1.amazonaws.com
```

Open the endpoint in a browser using HTTP.

---

## ✅ Verification

The website should display the contents of:

```text
/root/index.html
```

Final checklist:

- Bucket created: ✅
- Bucket name `datacenter-web-286272606`: ✅
- Region `us-east-1`: ✅
- Public access allowed: ✅
- Static website hosting enabled: ✅
- Index document set to `index.html`: ✅
- `/root/index.html` uploaded: ✅
- Bucket policy allows `s3:GetObject`: ✅
- Website URL opens successfully: ✅

---

## 🧠 Key Concepts Learned

### ✅ Amazon S3 Static Website Hosting

Amazon S3 can host static websites containing files such as HTML, CSS, JavaScript, and images without requiring a web server.

---

### ✅ S3 Bucket Policy

A bucket policy controls access to resources in an S3 bucket. The policy used in this task grants public `s3:GetObject` access to objects.

---

### ✅ Block Public Access

S3 Block Public Access settings can prevent public bucket policies from taking effect. Static website hosting with public objects requires these settings to be disabled appropriately.

---

### ✅ S3 Website Endpoint

The S3 website endpoint provides browser access to the hosted static content. Classic S3 website endpoints use HTTP.

---

## 🔐 Security Considerations

- Only disable Block Public Access when public website content is required
- Grant only the permissions needed by the website
- Do not upload secrets or private data to a public bucket
- Review bucket policies regularly
- Consider CloudFront and HTTPS for production websites
- Use Origin Access Control when serving private S3 content through CloudFront

---

## 📚 AWS Services & Concepts Practiced

- Amazon S3
- S3 Buckets
- S3 Static Website Hosting
- S3 Bucket Policies
- S3 Block Public Access
- AWS CLI
- `aws s3 cp`
- `aws s3 ls`
- Public Object Access
- Website Endpoints

---

## 📅 Cloud 100 Days Progress

**Day 39 – S3 Static Website Hosting**

Status: ✅ **Completed**

---

## 🎯 Result

The static website was successfully hosted using the S3 bucket:

```text
datacenter-web-286272606
```

The `index.html` file was uploaded, public object access was configured, and the website was made available through the S3 website endpoint.
