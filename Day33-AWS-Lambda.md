# ☁️ Day 33 – Create an AWS Lambda Function

---

## 📌 Task Overview

The Nautilus DevOps team is exploring **serverless architecture** using AWS Lambda.

The objective was to create a simple Python-based Lambda function that returns a custom greeting with a successful HTTP-style status code.

This task demonstrates the basic concepts of:

- AWS Lambda
- Serverless computing
- Python functions
- IAM execution roles
- Lambda deployment
- Lambda testing

---

## 🎯 Objectives

The following requirements were completed:

- Create a Lambda function named `devops-lambda`
- Use Python as the runtime
- Create and use an IAM role named `lambda_execution_role`
- Return the message `Welcome to KKE AWS Labs!`
- Return status code `200`
- Deploy and test the Lambda function
- Use the `us-east-1` AWS region

---

## 🏗️ Architecture

```text
                    AWS Lambda
                         │
                         ▼
                ┌─────────────────┐
                │  devops-lambda  │
                │                 │
                │  Python Runtime │
                │                 │
                │ lambda_handler  │
                └────────┬────────┘
                         │
                         ▼
                  Lambda Response
                         │
                ┌────────┴────────┐
                │                 │
          Status Code           Body
              200        Welcome to KKE AWS Labs!
```

---

## ☁️ AWS Resources

- Lambda Function: `devops-lambda`
- Runtime: Python
- IAM Role: `lambda_execution_role`
- Region: `us-east-1`
- Status Code: `200`
- Response Body: `Welcome to KKE AWS Labs!`

---

## 🛠️ Implementation

### Step 1: Open AWS Lambda

Opened the AWS Management Console and navigated to:

```text
Lambda → Functions → Create function
```

Region used:

```text
us-east-1
```

---

### Step 2: Create Lambda Function

Selected:

```text
Author from scratch
```

Function name:

```text
devops-lambda
```

Runtime:

```text
Python
```

---

### Step 3: Configure IAM Execution Role

The Lambda function requires an IAM execution role to interact with AWS services and write execution logs.

Created/used the IAM role:

```text
lambda_execution_role
```

The role uses the AWS managed policy:

```text
AWSLambdaBasicExecutionRole
```

This allows Lambda to write logs to Amazon CloudWatch Logs.

---

### Step 4: Configure Lambda Code

The Lambda function was configured with the following Python code:

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```

---

### Step 5: Deploy the Function

After entering the Python code, the function was deployed using the:

```text
Deploy
```

option in the Lambda console.

---

### Step 6: Test the Lambda Function

A test event was created in the Lambda console.

The function was then executed using:

```text
Test
```

The execution completed successfully.

---

### Step 7: Expected Response

The Lambda function returned:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

This confirms that the Lambda function is correctly configured and deployed.

---

## 🔍 Verification

- Lambda function created: ✅
- Function name `devops-lambda`: ✅
- Python runtime: ✅
- IAM role `lambda_execution_role`: ✅
- `AWSLambdaBasicExecutionRole` attached: ✅
- Status code `200`: ✅
- Body `Welcome to KKE AWS Labs!`: ✅
- Function deployed: ✅
- Function tested successfully: ✅
- Region `us-east-1`: ✅

---

## 🧠 Key Concepts Learned

### ✅ AWS Lambda

AWS Lambda is a **serverless compute service** that allows code to run without directly managing servers.

Instead of provisioning an EC2 instance, installing an operating system, and managing the server, we can simply deploy a function.

---

### ✅ Serverless Computing

With EC2:

```text
Create EC2
     ↓
Choose OS
     ↓
Configure Server
     ↓
Install Software
     ↓
Deploy Application
     ↓
Manage Infrastructure
```

With Lambda:

```text
Write Function
     ↓
Deploy Function
     ↓
AWS Runs the Function
```

AWS manages the underlying compute infrastructure.

---

### ✅ IAM Execution Role

Lambda functions use an IAM execution role to obtain permissions required during execution.

For this task:

```text
lambda_execution_role
```

was used.

The role included:

```text
AWSLambdaBasicExecutionRole
```

which provides permissions required for Lambda to send logs to CloudWatch Logs.

---

### ✅ Lambda Handler

The function uses:

```python
def lambda_handler(event, context):
```

The two parameters are:

#### `event`
Contains information passed to the Lambda function.

#### `context`
Provides information about the Lambda execution environment and invocation.

---

### ✅ Lambda Response

The function returns:

```json
{
    "statusCode": 200,
    "body": "Welcome to KKE AWS Labs!"
}
```

#### `statusCode`

```text
200
```

represents a successful response.

#### `body`

```text
Welcome to KKE AWS Labs!
```

is the custom greeting returned by the function.

---

## 🆚 Lambda vs EC2

| Feature | AWS Lambda | EC2 |
| --- | --- | --- |
| Server management | AWS managed | User managed |
| Infrastructure | Serverless | Virtual machine |
| OS management | AWS managed | User managed |
| Scaling | Automatic | User configured |
| Billing model | Pay per execution/duration | Instance running time |
| Best suited for | Event-driven workloads | Long-running applications |
| Infrastructure setup | Minimal | More configuration |

---

## 🎤 Interview Questions

### 1. What is AWS Lambda?

AWS Lambda is a serverless compute service that runs code in response to events without requiring users to manage servers.

### 2. What is serverless computing?

Serverless computing allows developers to run applications without directly managing the underlying servers and infrastructure.

### 3. What is a Lambda execution role?

An execution role is an IAM role that grants a Lambda function permission to access AWS resources and perform required actions during execution.

### 4. What is a Lambda handler?

The handler is the function that AWS Lambda invokes when the Lambda function runs.

Example:

```python
def lambda_handler(event, context):
```

### 5. What is the purpose of `event`?

The `event` parameter contains data passed to the Lambda function during invocation.

### 6. What is the purpose of `context`?

The `context` parameter provides information about the current Lambda execution environment and invocation.

### 7. Why is `AWSLambdaBasicExecutionRole` used?

It provides the Lambda function with basic permissions required to write execution logs to CloudWatch Logs.

### 8. What does HTTP status code 200 mean?

Status code `200` indicates that the request was successfully processed.

---

## 🔐 Security Best Practices

- Follow the principle of least privilege when creating Lambda IAM roles
- Grant only the permissions required by the function
- Never hard-code AWS access keys in Lambda code
- Use IAM roles instead of static credentials
- Store sensitive configuration using appropriate AWS services
- Monitor Lambda executions using CloudWatch Logs

---

## 📚 AWS Services & Concepts Practiced

- AWS Lambda
- IAM
- IAM Roles
- Python
- Serverless Computing
- CloudWatch Logs
- Lambda Deployment
- Lambda Testing
- AWS Console

---

## 📅 Cloud 100 Days Progress

**Day 33 – AWS Lambda**

Status: ✅ **Completed**

---

## 🚀 Learning Outcome

This task provided hands-on experience with AWS Lambda and the fundamentals of serverless computing.

The complete workflow was:

```text
Create Lambda Function
        ↓
Select Python Runtime
        ↓
Configure IAM Execution Role
        ↓
Write Python Handler
        ↓
Deploy
        ↓
Test
        ↓
Successful Response
```

The Lambda function successfully returned:

```text
Status Code: 200
Body: Welcome to KKE AWS Labs!
```

---

## ⭐ Summary

Successfully created and deployed:

```text
Lambda Function:
devops-lambda

Runtime:
Python

IAM Role:
lambda_execution_role

Response:
200

Body:
Welcome to KKE AWS Labs!

Region:
us-east-1

Status:
Completed ✅
```

This completes **Cloud Day 33**. 🎯
