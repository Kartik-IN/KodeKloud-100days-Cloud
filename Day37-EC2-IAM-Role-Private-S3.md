# ☁️ Day 37 – EC2 to Private S3 Access Using IAM Role

---

## 📌 Task Overview

This task demonstrates how to securely provide an EC2 instance with access to a private Amazon S3 bucket using an IAM role.

The EC2 instance does not use hard-coded AWS access keys. Instead, it obtains temporary credentials through an IAM instance role.

---

## 🏗️ Architecture

```text
                     AWS
                      │
             ┌──────────────────┐
             │   datacenter-ec2 │
             │                  │
             │ IAM Role        │
             │ datacenter-role  │
             └────────┬─────────┘
                      │
                      │ S3 Permissions
                      ▼
        ┌───────────────────────────────┐
        │ datacenter-s3-403751978661     │
        │                               │
        │ Private S3 Bucket             │
        └───────────────────────────────┘
```

---

## ☁️ AWS Resources

- Region: `us-east-1`
- EC2 Instance: `datacenter-ec2`
- S3 Bucket: `datacenter-s3-403751978661`
- IAM Role: `datacenter-role`
- IAM Policy: `datacenter-s3-policy`

---

## 🛠️ Implementation

### Step 1: Use the EC2 Instance

An existing EC2 instance named:

```text
datacenter-ec2
```

was used for this task.

The instance requires permission to upload, download, and list objects in the private S3 bucket.

---

### Step 2: Configure SSH Key Access

A new RSA SSH key pair was created on the `aws-client` host:

```bash
ssh-keygen -t rsa -b 2048 -f /root/.ssh/id_rsa
```

The generated files are:

```text
/root/.ssh/id_rsa
/root/.ssh/id_rsa.pub
```

The public key was added to the root user's SSH authorized keys on the EC2 instance:

```text
/root/.ssh/authorized_keys
```

Permissions were configured using:

```bash
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
```

The EC2 instance can then be accessed using:

```bash
ssh -i /root/.ssh/id_rsa root@<EC2-PUBLIC-IP>
```

This enabled passwordless SSH access.

---

### Step 3: Create the Private S3 Bucket

Created an S3 bucket named:

```text
datacenter-s3-403751978661
```

Region:

```text
us-east-1
```

The bucket remains private.

S3 Block Public Access was enabled, and no public bucket policy was configured.

---

### Step 4: Create the IAM Policy

Created an IAM policy named:

```text
datacenter-s3-policy
```

The policy grants only the permissions required to upload, download, and list objects:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::datacenter-s3-403751978661/*"
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListBucket",
      "Resource": "arn:aws:s3:::datacenter-s3-403751978661"
    }
  ]
}
```

Permissions provided:

| Permission | Purpose |
| --- | --- |
| `s3:PutObject` | Upload objects |
| `s3:GetObject` | Download objects |
| `s3:ListBucket` | List bucket contents |

---

### Step 5: Create the IAM Role

Created an IAM role named:

```text
datacenter-role
```

The policy below was attached:

```text
datacenter-s3-policy
```

The access relationship is:

```text
datacenter-ec2
      │
      ▼
datacenter-role
      │
      ▼
datacenter-s3-policy
      │
      ▼
datacenter-s3-403751978661
```

---

### Step 6: Attach the IAM Role to EC2

The IAM role:

```text
datacenter-role
```

was attached to:

```text
datacenter-ec2
```

This allows the EC2 instance to obtain temporary AWS credentials automatically.

No permanent AWS access keys were required on the EC2 instance.

---

### Step 7: Create a Test File

The following commands were executed inside `datacenter-ec2`.

Created a test file:

```bash
echo "Hello from datacenter-ec2" > /tmp/test.txt
```

Verified the file:

```bash
cat /tmp/test.txt
```

Expected output:

```text
Hello from datacenter-ec2
```

---

### Step 8: Upload the File to S3

From the EC2 instance:

```bash
aws s3 cp /tmp/test.txt s3://datacenter-s3-403751978661/
```

Expected result:

```text
upload: /tmp/test.txt to s3://datacenter-s3-403751978661/test.txt
```

---

### Step 9: List the S3 Bucket

Verified that the object was uploaded:

```bash
aws s3 ls s3://datacenter-s3-403751978661/
```

Expected output contains:

```text
test.txt
```

---

### Step 10: Test Download Access

Tested the `GetObject` permission:

```bash
aws s3 cp s3://datacenter-s3-403751978661/test.txt /tmp/downloaded.txt
```

Verified the downloaded file:

```bash
cat /tmp/downloaded.txt
```

Expected output:

```text
Hello from datacenter-ec2
```

---

### Step 11: Verify the IAM Role

From the EC2 instance:

```bash
aws sts get-caller-identity
```

The AWS CLI should obtain credentials through the EC2 instance role.

The EC2 instance does not need manually configured access keys.

---

## 🔐 Security Model

The final security model is:

```text
                 AWS Client
                     │
                     │ SSH
                     ▼
              datacenter-ec2
                     │
                     │ IAM Instance Role
                     ▼
              datacenter-role
                     │
                     │ Attached Policy
                     ▼
            datacenter-s3-policy
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
       PutObject  GetObject  ListBucket
                     │
                     ▼
       datacenter-s3-403751978661
```

The S3 bucket remains private and is accessed through IAM authorization.

---

## 🧪 Verification Checklist

- `datacenter-ec2` exists
- SSH key pair created on `aws-client`
- `id_rsa` created
- `id_rsa.pub` created
- Public key added to EC2
- Passwordless SSH configured
- Private S3 bucket created
- Bucket name is `datacenter-s3-403751978661`
- S3 Block Public Access enabled
- IAM policy created
- `s3:PutObject` permission configured
- `s3:GetObject` permission configured
- `s3:ListBucket` permission configured
- IAM role `datacenter-role` created
- Policy attached to IAM role
- IAM role attached to EC2
- File uploaded from EC2 to S3
- Uploaded file listed successfully
- File downloaded successfully from S3

---

## 🎯 Key AWS Concepts Learned

- EC2 SSH key authentication
- AWS IAM roles
- IAM policies
- EC2 instance profiles
- Amazon S3
- S3 bucket security
- S3 object permissions
- `aws s3 cp`
- `aws s3 ls`
- Temporary credentials through IAM roles
- Least-privilege access

---

## ✅ Final Result

The `datacenter-ec2` instance successfully accessed the private S3 bucket:

```text
datacenter-s3-403751978661
```

using the IAM role:

```text
datacenter-role
```

without requiring hard-coded AWS access keys.

The upload test:

```bash
aws s3 cp /tmp/test.txt s3://datacenter-s3-403751978661/
```

and listing test:

```bash
aws s3 ls s3://datacenter-s3-403751978661/
```

were successfully completed.

---

## 📅 Cloud 100 Days Progress

**Day 37 – EC2 to Private S3 Access Using IAM Role**

Status: ✅ **Completed**
