# ☁️ Day 42 – DynamoDB To-Do Application

---

## 📌 Task Overview

The Nautilus DevOps team required a DynamoDB table for a simple To-Do application. The table stores tasks identified by a unique `taskId`, along with their description and current status.

The DynamoDB table was created and populated with the required tasks.

---

## 🏗️ AWS Configuration

- AWS Service: Amazon DynamoDB
- Region: `us-east-1`
- Table Name: `devops-tasks`
- Primary Key: `taskId`
- Key Type: String (`S`)
- Billing Mode: Pay-per-request

---

## 🛠️ Implementation

### Step 1: Create the DynamoDB Table

The table was created using:

```bash
aws dynamodb create-table \
  --table-name devops-tasks \
  --attribute-definitions AttributeName=taskId,AttributeType=S \
  --key-schema AttributeName=taskId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region us-east-1
```

The table was then verified:

```bash
aws dynamodb describe-table \
  --table-name devops-tasks \
  --region us-east-1 \
  --query 'Table.TableStatus'
```

Expected status:

```text
ACTIVE
```

---

### Step 2: Insert Task 1

Task details:

- Task ID: `1`
- Description: `Learn DynamoDB`
- Status: `completed`

Inserted the task using:

```bash
aws dynamodb put-item \
  --table-name devops-tasks \
  --item '{
    "taskId": {"S": "1"},
    "description": {"S": "Learn DynamoDB"},
    "status": {"S": "completed"}
  }' \
  --region us-east-1
```

---

### Step 3: Insert Task 2

Task details:

- Task ID: `2`
- Description: `Build To-Do App`
- Status: `in-progress`

Inserted the task using:

```bash
aws dynamodb put-item \
  --table-name devops-tasks \
  --item '{
    "taskId": {"S": "2"},
    "description": {"S": "Build To-Do App"},
    "status": {"S": "in-progress"}
  }' \
  --region us-east-1
```

---

### Step 4: Verify Task 1

```bash
aws dynamodb get-item \
  --table-name devops-tasks \
  --key '{"taskId":{"S":"1"}}' \
  --region us-east-1
```

Expected data:

```text
taskId: 1
description: Learn DynamoDB
status: completed
```

---

### Step 5: Verify Task 2

```bash
aws dynamodb get-item \
  --table-name devops-tasks \
  --key '{"taskId":{"S":"2"}}' \
  --region us-east-1
```

Expected data:

```text
taskId: 2
description: Build To-Do App
status: in-progress
```

---

### Step 6: Verify All Items

The complete table was checked using:

```bash
aws dynamodb scan \
  --table-name devops-tasks \
  --region us-east-1
```

Expected data:

| taskId | description | status |
| --- | --- | --- |
| `1` | Learn DynamoDB | `completed` |
| `2` | Build To-Do App | `in-progress` |

---

## 🏆 Final Result

```text
Amazon DynamoDB
└── devops-tasks
    │
    ├── Primary Key: taskId (String)
    │
    ├── Task 1
    │   ├── taskId: 1
    │   ├── description: Learn DynamoDB
    │   └── status: completed
    │
    └── Task 2
        ├── taskId: 2
        ├── description: Build To-Do App
        └── status: in-progress
```

---

## ✅ Final Checklist

- DynamoDB table created: ✅
- Table name `devops-tasks`: ✅
- Primary key `taskId`: ✅
- Primary key type String: ✅
- Table status `ACTIVE`: ✅
- Task 1 inserted: ✅
- Task 1 status `completed`: ✅
- Task 2 inserted: ✅
- Task 2 status `in-progress`: ✅
- Both tasks verified: ✅

---

## 🧠 Key Concepts Learned

- Amazon DynamoDB
- DynamoDB tables
- Partition keys
- String attribute types
- Pay-per-request billing
- `aws dynamodb create-table`
- `aws dynamodb put-item`
- `aws dynamodb get-item`
- `aws dynamodb scan`
- NoSQL data modeling

---

## 📅 Cloud 100 Days Progress

**Day 42 – DynamoDB To-Do Application**

Status: ✅ **Completed**

---

The DynamoDB To-Do application lab was completed successfully. 🚀
