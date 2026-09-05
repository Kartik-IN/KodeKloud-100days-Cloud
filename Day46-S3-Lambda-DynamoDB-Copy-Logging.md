# ☁️ Day 46 – S3 to S3 Lambda Copy with DynamoDB Logging

## 📌 Task Overview

Created an automated workflow where a file uploaded to a public S3 bucket is copied to a private S3 bucket using AWS Lambda. The Lambda function also records the copy operation details in DynamoDB.

## 🪣 S3 Buckets

Created the following buckets:

```text
datacenter-public-3315
datacenter-private-7171
```

### Public Bucket

`datacenter-public-3315` was configured to allow public access to objects and used as the source bucket.

### Private Bucket

`datacenter-private-7171` keeps public access blocked and is used as the destination bucket.

## 🗄️ DynamoDB Table

Created the DynamoDB table:

```text
datacenter-S3CopyLogs
```

Primary key:

```text
LogID
```

Type:

```text
String
```

The table stores copy-operation information, including:

- `LogID`
- `SourceBucket`
- `DestinationBucket`
- `ObjectKey`
- `Status`

## 🔐 IAM Role

Created the Lambda execution role:

```text
lambda_execution_role
```

The role provides permissions to:

- Read objects from the public S3 bucket
- Write objects to the private S3 bucket
- Write operation logs to DynamoDB
- Write logs to CloudWatch

## ⚡ Lambda Function

Created the Lambda function:

```text
datacenter-copyfunction
```

Configuration:

```text
Memory: 128 MB
Timeout: 10 seconds
Runtime: Python
Execution Role: lambda_execution_role
```

The function was configured using:

```text
/root/lambda-function.py
```

The required values were updated as follows:

```text
REPLACE-WITH-YOUR-DYNAMODB-TABLE
        ↓
datacenter-S3CopyLogs
```

```text
REPLACE-WITH-YOUR-PRIVATE-BUCKET
        ↓
datacenter-private-7171
```

## 🔔 S3 Trigger

Configured the public S3 bucket to trigger the Lambda function whenever an object is uploaded.

```text
Upload Object
     ↓
datacenter-public-3315
     ↓
S3 Event
     ↓
datacenter-copyfunction
```

## 🧪 Test the Lambda Function

Uploaded the test file:

```text
sample.zip
```

Source file:

```text
/root/sample.zip
```

The file was uploaded to:

```text
datacenter-public-3315
```

The S3 event triggered the Lambda function automatically.

## ✅ Verify the File Copy

Checked the destination bucket:

```text
datacenter-private-7171
```

The following object was copied successfully:

```text
sample.zip
```

Copy status:

```text
Success
```

## 📋 Verify DynamoDB Logging

Opened the table:

```text
datacenter-S3CopyLogs
```

The table returned three items. The verified `sample.zip` entry contained:

```text
ObjectKey: sample.zip
Status: Success
SourceBucket: datacenter-public-3315
DestinationBucket: datacenter-private-7171
```

This confirms that Lambda successfully performed the copy and recorded the operation in DynamoDB.

## 🏗️ Final Architecture

```text
                 ┌─────────────────────────┐
                 │  Public S3 Bucket       │
                 │ datacenter-public-3315  │
                 └────────────┬────────────┘
                              │
                         S3 Upload
                              │
                              ▼
                 ┌─────────────────────────┐
                 │  Lambda Function        │
                 │ datacenter-copyfunction │
                 └────────────┬────────────┘
                              │
                  ┌───────────┴───────────┐
                  │                       │
                  ▼                       ▼
       ┌─────────────────────┐  ┌──────────────────────┐
       │ Private S3 Bucket   │  │ DynamoDB Table       │
       │ datacenter-private  │  │ datacenter-S3CopyLogs│
       │ -7171               │  │                      │
       └─────────────────────┘  └──────────────────────┘
```

## 🧾 Final Verification

| Component | Status |
| --- | --- |
| Public S3 bucket | ✅ Created |
| Private S3 bucket | ✅ Created |
| Lambda function | ✅ Created |
| IAM role | ✅ Configured |
| S3 trigger | ✅ Working |
| `sample.zip` copied | ✅ Success |
| DynamoDB table | ✅ Created |
| DynamoDB log | ✅ Success |
| Overall task | ✅ Completed |
