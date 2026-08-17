# ☁️ Day 34 – Deploy AWS Lambda Using AWS CLI

---

## 📌 Task Overview

The Nautilus DevOps team continued exploring serverless architecture by deploying an AWS Lambda function using the **AWS CLI**.

Unlike the previous Lambda task, where the function code was created directly through the AWS Console, this task involved creating the Python source code locally, packaging it into a ZIP file, and deploying it to AWS using CLI commands.

This demonstrates a more automation-friendly approach to Lambda deployments.

---

## 🎯 Objectives

The following requirements were completed:

- Create a Python script named `lambda_function.py`
- Create a Lambda handler that returns:
  - Status code: `200`
  - Body: `Welcome to KKE AWS Labs!`
- Package the Python script into `function.zip`
- Create a Lambda function named `nautilus-lambda-cli`
- Use Python as the Lambda runtime
- Use the IAM role `lambda_execution_role`
- Deploy the Lambda function using AWS CLI
- Invoke and verify the Lambda function
- Use the `us-east-1` region

---

## 🏗️ Deployment Workflow

```text
                Local AWS Client
                       │
                       ▼
              lambda_function.py
                       │
                       │ ZIP
                       ▼
                  function.zip
                       │
                       │ AWS CLI
                       ▼
              ┌─────────────────────┐
              │    AWS Lambda       │
              │                     │
              │ nautilus-lambda-cli │
              │                     │
              │ Python Runtime      │
              └──────────┬──────────┘
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

- Lambda Function: `nautilus-lambda-cli`
- Runtime: Python
- IAM Role: `lambda_execution_role`
- Python Source: `lambda_function.py`
- Deployment Package: `function.zip`
- Region: `us-east-1`
- Status Code: `200`
- Response Body: `Welcome to KKE AWS Labs!`

---

## 🛠️ Implementation

### Step 1: Verify AWS CLI

The AWS CLI was already configured on the `aws-client` host.

Verified the current AWS identity:

```bash
aws sts get-caller-identity
```

This confirmed that the AWS CLI was authenticated and ready to use.

---

### Step 2: Configure AWS Region

The task required the resources to be created in:

```text
us-east-1
```

Configured the AWS CLI region:

```bash
aws configure set region us-east-1
```

---

### Step 3: Create Python Lambda Script

Created:

```text
lambda_function.py
```

The file contains:

```python
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```

---

### Step 4: Create Lambda Deployment Package

The Python file was packaged into:

```text
function.zip
```

Command used:

```bash
zip function.zip lambda_function.py
```

Verified the ZIP contents:

```bash
unzip -l function.zip
```

Expected structure:

```text
function.zip
└── lambda_function.py
```

The Python file was placed at the root of the ZIP package.

---

### Step 5: Retrieve IAM Role ARN

The Lambda function uses the existing IAM role:

```text
lambda_execution_role
```

Retrieved the role ARN using:

```bash
aws iam get-role \
  --role-name lambda_execution_role \
  --query 'Role.Arn' \
  --output text
```

The returned ARN was used when creating the Lambda function.

---

### Step 6: Create Lambda Function Using AWS CLI

Created the Lambda function:

```text
nautilus-lambda-cli
```

using the AWS CLI.

Command structure:

```bash
aws lambda create-function \
  --function-name nautilus-lambda-cli \
  --runtime python3.x \
  --role <ROLE_ARN> \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip \
  --region us-east-1
```

The deployment package was uploaded directly from:

```text
function.zip
```

---

### Step 7: Configure Lambda Handler

The Lambda handler was:

```text
lambda_function.lambda_handler
```

This means:

```text
lambda_function.py
        │
        └── lambda_handler()
```

Lambda uses this handler as the entry point when the function is invoked.

---

### Step 8: Verify Lambda Function

Verified the created Lambda function using:

```bash
aws lambda get-function \
  --function-name nautilus-lambda-cli \
  --region us-east-1
```

The function was successfully created.

---

### Step 9: Invoke Lambda Function

Created a simple test event:

```bash
echo '{}' > event.json
```

Invoked the Lambda function:

```bash
aws lambda invoke \
  --function-name nautilus-lambda-cli \
  --payload fileb://event.json \
  --region us-east-1 \
  response.json
```

The response was stored in:

```text
response.json
```

---

### Step 10: Verify Lambda Response

Displayed the response:

```bash
cat response.json
```

Expected response:

```json
{
    "statusCode": 200,
    "body": "Welcome to KKE AWS Labs!"
}
```

The successful response confirmed that the Lambda function was correctly deployed and executed.

---

## 🔍 Verification

- AWS CLI configured: ✅
- Region `us-east-1`: ✅
- `lambda_function.py` created: ✅
- Lambda handler configured: ✅
- Status code `200`: ✅
- Greeting configured: ✅
- `function.zip` created: ✅
- Lambda function created: ✅
- Function name `nautilus-lambda-cli`: ✅
- Python runtime: ✅
- IAM role `lambda_execution_role`: ✅
- Lambda invocation successful: ✅
- Response verified: ✅

---

## 🧠 Key Concepts Learned

### ✅ AWS Lambda

AWS Lambda is a serverless compute service that executes code without requiring users to manage the underlying servers.

---

### ✅ Lambda Deployment Package

A Lambda deployment package contains the application code and required files needed by the Lambda function.

In this task:

```text
function.zip
└── lambda_function.py
```

was used as the deployment package.

---

### ✅ Lambda Handler

The handler identifies the function Lambda should execute.

Configured handler:

```text
lambda_function.lambda_handler
```

It follows:

```text
<filename_without_extension>.<function_name>
```

---

### ✅ AWS CLI Lambda Deployment

Instead of manually entering code in the AWS Console, the function was deployed using:

```bash
aws lambda create-function
```

This approach is useful for automation and CI/CD pipelines.

---

### ✅ IAM Execution Role

Lambda requires an IAM execution role.

The existing role:

```text
lambda_execution_role
```

was used during deployment.

The role provides Lambda with the permissions required during execution.

---

## 🆚 Console vs AWS CLI Deployment

| Feature | AWS Console | AWS CLI |
| --- | --- | --- |
| Code creation | Browser editor | Local file |
| Packaging | AWS managed | User creates ZIP |
| Deployment | Manual | CLI command |
| Automation | Limited | Easy to automate |
| CI/CD integration | Less direct | Excellent |
| Repeatability | Manual | High |

---

## 🔄 Deployment Process

The complete deployment process was:

```text
Create Python Code
       ↓
lambda_function.py
       ↓
Create ZIP
       ↓
function.zip
       ↓
Get IAM Role ARN
       ↓
AWS CLI create-function
       ↓
nautilus-lambda-cli
       ↓
Invoke Function
       ↓
Verify Response
```

---

## 🎤 Interview Questions

### 1. Why package Lambda code into a ZIP file?

ZIP packaging allows the Lambda function code and required files to be uploaded together as a deployment package.

### 2. What is the Lambda handler?

The handler is the function that AWS Lambda invokes when the function runs.

Example:

```text
lambda_function.lambda_handler
```

### 3. What does `fileb://function.zip` mean?

It tells the AWS CLI to read the specified local file as binary data for the upload.

### 4. Why use AWS CLI instead of the Console?

CLI-based deployments are easier to automate, reproduce, script, and integrate into CI/CD pipelines.

### 5. What is the purpose of the IAM execution role?

It grants the Lambda function permissions to interact with AWS services during execution.

### 6. What does status code `200` indicate?

It indicates that the Lambda function executed successfully and returned a successful response.

### 7. What is the difference between `event` and `context`?

`event` contains input data passed to the Lambda function, while `context` provides information about the current Lambda execution environment.

---

## 🔐 Security Best Practices

- Use IAM roles instead of hard-coded AWS credentials
- Follow the principle of least privilege
- Never store AWS access keys inside Python source code
- Never commit sensitive credentials to GitHub
- Restrict IAM permissions to only what the Lambda function requires
- Use environment variables or AWS Secrets Manager for sensitive configuration

---

## 📚 AWS Services & Tools Practiced

- AWS Lambda
- AWS CLI
- IAM
- IAM Roles
- Python
- ZIP Packaging
- Serverless Computing
- Lambda Invocation
- AWS Console
- CloudWatch Logs

---

## 📅 Cloud 100 Days Progress

**Day 34 – Deploy AWS Lambda Using AWS CLI**

Status: ✅ **Completed**

---

## 🚀 Learning Outcome

This task provided hands-on experience deploying AWS Lambda using the AWS CLI instead of manually entering code through the AWS Console.

The complete workflow was:

```text
Python Source Code
        ↓
ZIP Packaging
        ↓
AWS CLI
        ↓
Lambda Deployment
        ↓
Lambda Invocation
        ↓
Response Verification
```

The function successfully returned:

```text
Status Code: 200
Body: Welcome to KKE AWS Labs!
```

This workflow is an important foundation for eventually deploying Lambda functions through automated CI/CD pipelines.

---

## ⭐ Summary

Successfully deployed:

```text
Lambda Function:
nautilus-lambda-cli

Runtime:
Python

Source File:
lambda_function.py

Deployment Package:
function.zip

IAM Role:
lambda_execution_role

Region:
us-east-1

Response:
200

Body:
Welcome to KKE AWS Labs!

Status:
Completed ✅
```

This completes **Cloud Day 34**. 🎯
