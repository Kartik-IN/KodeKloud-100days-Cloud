# 🚀 Day 47 – AWS Priority Queue Processing with SNS, SQS & Lambda

## 📌 Overview

Implemented a priority-based message processing system on AWS using:

- Amazon SNS
- Amazon SQS
- AWS Lambda
- AWS IAM
- AWS CloudFormation

Messages published to the SNS topic are routed to the appropriate SQS queue according to the `priority` message attribute. Lambda processes high-priority messages before low-priority messages.

## 🏗️ Architecture

```text
                         Amazon SNS
                    xfusion-Priority-Queues-Topic
                              │
                ┌─────────────┴─────────────┐
                │                           │
          priority=high                priority=low
                │                           │
                ▼                           ▼
      High Priority Queue          Low Priority Queue
      xfusion-High-...             xfusion-Low-...
                \                           /
                 \                         /
                  └──────────┬────────────┘
                             │
                             ▼
                    AWS Lambda Function
              xfusion-priorities-queue-function
                             │
                             ▼
                    High Priority First
                    Then Low Priority
```

## ☁️ CloudFormation Stack

Stack name:

```text
xfusion-priority-stack
```

Template:

```text
/root/xfusion-priority-stack.yml
```

Create the stack:

```bash
aws cloudformation create-stack \
  --stack-name xfusion-priority-stack \
  --template-body file:///root/xfusion-priority-stack.yml \
  --capabilities CAPABILITY_NAMED_IAM
```

Check the stack status:

```bash
aws cloudformation describe-stacks \
  --stack-name xfusion-priority-stack \
  --region us-east-1 \
  --query "Stacks[0].StackStatus"
```

Expected status:

```text
CREATE_COMPLETE
```

## 📦 AWS Resources

### SQS Queues

High-priority queue:

```text
xfusion-High-Priority-Queue
```

Low-priority queue:

```text
xfusion-Low-Priority-Queue
```

Both queues use:

```text
VisibilityTimeout: 60
```

### SNS Topic

```text
xfusion-Priority-Queues-Topic
```

### Lambda Function

```text
xfusion-priorities-queue-function
```

Configuration:

```text
Runtime: Python 3.9
Handler: index.lambda_handler
Memory: 128 MB
Timeout: 5 seconds
```

### IAM Role

```text
lambda_execution_role
```

Attached managed policies:

```text
AWSLambdaBasicExecutionRole
AmazonSQSFullAccess
AmazonSNSFullAccess
```

## 🔀 SNS Message Filtering

The high-priority subscription uses:

```yaml
FilterPolicy:
  priority:
    - high
```

The low-priority subscription uses:

```yaml
FilterPolicy:
  priority:
    - low
```

Routing behavior:

```text
priority=high → High Priority Queue
priority=low  → Low Priority Queue
```

SQS queue policies were configured to allow the SNS topic to deliver messages.

## 🧪 Publish Test Messages

Get the SNS topic ARN:

```bash
topicarn=$(aws sns list-topics \
  --region us-east-1 \
  --query "Topics[?contains(TopicArn, 'xfusion-Priority-Queues-Topic')].TopicArn" \
  --output text)
```

Publish high-priority messages:

```bash
aws sns publish \
  --topic-arn "$topicarn" \
  --message 'High Priority message 1' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"high"}}' \
  --region us-east-1
```

```bash
aws sns publish \
  --topic-arn "$topicarn" \
  --message 'High Priority message 2' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"high"}}' \
  --region us-east-1
```

Publish low-priority messages:

```bash
aws sns publish \
  --topic-arn "$topicarn" \
  --message 'Low Priority message 1' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"low"}}' \
  --region us-east-1
```

```bash
aws sns publish \
  --topic-arn "$topicarn" \
  --message 'Low Priority message 2' \
  --message-attributes '{"priority":{"DataType":"String","StringValue":"low"}}' \
  --region us-east-1
```

## ▶️ Invoke Lambda

Invoke the Lambda function:

```bash
aws lambda invoke \
  --function-name xfusion-priorities-queue-function \
  --region us-east-1 \
  /tmp/out.json && cat /tmp/out.json
```

Repeat the invocation to process all messages.

The Lambda polls the high-priority queue first, then polls the low-priority queue when no high-priority message is available.

Expected processing pattern:

```text
High Priority message 1/2
High Priority message 1/2
Low Priority message 1/2
Low Priority message 1/2
```

The two high-priority messages can appear in either order.

## 🔍 Verify SNS Subscriptions

```bash
aws sns list-subscriptions-by-topic \
  --topic-arn "$topicarn" \
  --region us-east-1 \
  --output table
```

Expected subscriptions:

```text
xfusion-High-Priority-Queue
xfusion-Low-Priority-Queue
```

## 📊 Verify Lambda Configuration

```bash
aws lambda get-function-configuration \
  --function-name xfusion-priorities-queue-function \
  --region us-east-1 \
  --query '[MemorySize,Timeout,Role]' \
  --output table
```

Expected values:

```text
128
5
arn:aws:iam::<ACCOUNT_ID>:role/lambda_execution_role
```

## 📝 Verify CloudWatch Logs

```bash
aws logs filter-log-events \
  --log-group-name /aws/lambda/xfusion-priorities-queue-function \
  --region us-east-1 \
  --query "events[].message" \
  --output text
```

The logs confirm that messages were processed by Lambda.

## ✅ Final Checklist

- CloudFormation stack created
- High-priority SQS queue created
- Low-priority SQS queue created
- SNS topic created
- SNS high-priority filter configured
- SNS low-priority filter configured
- SQS queue policies configured
- Lambda execution role created
- Lambda function deployed
- Lambda configured with queue environment variables
- High-priority messages processed first
- Low-priority messages processed after high-priority messages
- CloudFormation status verified as `CREATE_COMPLETE`

## 🎯 Result

Successfully implemented a priority-based messaging architecture using AWS CloudFormation, SNS, SQS, Lambda, and IAM. High-priority messages are routed to the high-priority queue and processed before low-priority messages.
