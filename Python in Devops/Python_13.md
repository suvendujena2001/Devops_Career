# 🚀 Python for DevOps Day 13: Boto3, AWS Lambda & Cloud Cost Optimization

## Complete Study Notes (Beginner → Advanced)

---

# 📖 Table of Contents

1. Introduction to Boto3
2. Why Boto3 is Important for DevOps
3. AWS CLI vs Boto3 vs Terraform vs CloudFormation
4. Prerequisites for Learning Boto3
5. Boto3 Architecture
6. Authentication with AWS
7. Working with AWS Services using Boto3
8. Boto3 Client vs Resource
9. Botocore & Exception Handling
10. AWS Lambda Fundamentals
11. Serverless Architecture
12. EC2 vs Lambda
13. Event-Driven Computing
14. Lambda Components
15. Cost Optimization in AWS
16. Real-World DevOps Project
17. EBS Snapshot Cleanup Automation
18. CloudWatch + Lambda Integration
19. Interview Questions
20. Quick Revision Sheet

---

# 1️⃣ Introduction to Boto3

## What is Boto3?

**Boto3** is the official Python SDK for AWS.

It enables Python applications to:

* Create AWS resources
* Delete AWS resources
* Modify AWS resources
* Retrieve AWS information

without manually calling AWS APIs.

---

## Simple Definition

> Boto3 is a Python package used to interact with AWS services programmatically.

---

## Real-world Example

Instead of:

```text
AWS Console
   ↓
Click Create EC2
   ↓
Select AMI
   ↓
Choose Instance Type
   ↓
Create Instance
```

You can write:

```python
import boto3
ec2 = boto3.client("ec2")
```

and create resources automatically.

---

# 2️⃣ Why Boto3 Matters for DevOps Engineers

DevOps engineers automate infrastructure.

Without Boto3:

```text
Manual AWS Operations
     ↓
Slow
     ↓
Error-Prone
```

With Boto3:

```text
Python Script
     ↓
Boto3
     ↓
AWS APIs
     ↓
AWS Resources
```

---

## Common DevOps Use Cases

| Use Case          | Example                 |
| ----------------- | ----------------------- |
| Resource Creation | EC2, S3                 |
| Monitoring        | CloudWatch              |
| Cost Optimization | Cleanup stale resources |
| Security Audits   | IAM checks              |
| Notifications     | SNS integration         |
| Automation        | Scheduled jobs          |

---

# 3️⃣ AWS CLI vs Boto3 vs Terraform vs CloudFormation

## Comparison Chart

| Feature           | AWS CLI     | Boto3      | Terraform                   | CloudFormation |
| ----------------- | ----------- | ---------- | --------------------------- | -------------- |
| Type              | CLI         | Python SDK | IaC                         | IaC            |
| Language          | Commands    | Python     | HCL                         | YAML/JSON      |
| Best For          | Quick tasks | Automation | Infrastructure provisioning | AWS-native IaC |
| Programming Logic | Limited     | Extensive  | Limited                     | Limited        |
| Lambda Support    | No          | Yes        | Indirect                    | Indirect       |

---

## Decision Flow

```text
Need Infrastructure?
        │
        ▼
Terraform / CloudFormation

Need Logic + Automation?
        │
        ▼
Boto3

Need Quick Command?
        │
        ▼
AWS CLI
```

---

# 4️⃣ Prerequisites Before Learning Boto3

## Golden Rule

> Never automate what you don't understand manually.

Before using Boto3:

✅ Create EC2 manually

✅ Create S3 manually

✅ Create IAM manually

✅ Understand AWS services

---

## Why?

When creating EC2:

You must know:

```text
AMI
Instance Type
Key Pair
Security Group
Subnet
```

Otherwise automation becomes difficult.

---

# 5️⃣ Boto3 Architecture

## Internal Workflow

```text
Python Script
       │
       ▼
Boto3 SDK
       │
       ▼
AWS API Calls
       │
       ▼
AWS Services
```

---

## Detailed Architecture

```text
Developer
    │
    ▼
Python Program
    │
    ▼
Boto3 Client
    │
    ▼
AWS Authentication
    │
    ▼
AWS APIs
    │
    ▼
AWS Services
(S3, EC2, IAM, Lambda)
```

---

# 6️⃣ AWS Authentication

## Step 1: Install AWS CLI

```bash
aws --version
```

---

## Step 2: Configure AWS

```bash
aws configure
```

Provide:

```text
Access Key
Secret Key
Region
Output Format
```

---

## Authentication Flow

```text
AWS Configure
       │
       ▼
Credentials Stored
       │
       ▼
Boto3 Uses Credentials
       │
       ▼
Access AWS APIs
```

---

# 7️⃣ Working with AWS Services using Boto3

## Install Boto3

```bash
pip install boto3
```

---

## Import

```python
import boto3
```

---

## Create Client

```python
client = boto3.client("s3")
```

---

## Create S3 Bucket

```python
client.create_bucket(
    Bucket="my-demo-bucket"
)
```

---

## Workflow

```text
Create Client
      │
      ▼
Call AWS API
      │
      ▼
AWS Executes Request
      │
      ▼
Returns JSON Response
```

---

# 8️⃣ Client vs Resource

Boto3 offers two approaches.

---

## Client

```python
boto3.client("s3")
```

### Characteristics

* Low-level
* Direct API access
* Recommended
* Supports all services

---

## Resource

```python
boto3.resource("s3")
```

### Characteristics

* Higher abstraction
* Easier syntax
* Limited future support

---

## Comparison

| Feature           | Client | Resource |
| ----------------- | ------ | -------- |
| Future Support    | ✅      | ❌        |
| Full AWS Coverage | ✅      | ❌        |
| Recommended       | ✅      | ❌        |

---

# 9️⃣ Botocore & Exception Handling

## What is Botocore?

Foundation layer behind Boto3.

Provides:

* AWS request handling
* Authentication
* Exception classes

---

## Example

```python
from botocore.exceptions import ClientError
```

---

# 🔟 AWS Lambda Fundamentals

## What is Lambda?

AWS Lambda is a serverless compute service.

---

## Key Idea

You provide:

```text
Code
```

AWS provides:

```text
Servers
Scaling
Availability
Maintenance
```

---

# Lambda Workflow

```text
Event
   │
   ▼
Lambda Triggered
   │
   ▼
AWS Creates Compute
   │
   ▼
Code Executes
   │
   ▼
Compute Destroyed
```

---

# 1️⃣1️⃣ Serverless Architecture

## Traditional Architecture

```text
Application
     │
     ▼
Server
     │
     ▼
Maintenance
Updates
Scaling
Monitoring
```

---

## Serverless Architecture

```text
Application Code
       │
       ▼
AWS Lambda
       │
       ▼
AWS Handles Everything
```

---

# Benefits

✅ Auto Scaling

✅ Pay Per Execution

✅ No Server Management

✅ Event Driven

---

# 1️⃣2️⃣ EC2 vs Lambda

## High-Level Comparison

| Feature           | EC2               | Lambda            |
| ----------------- | ----------------- | ----------------- |
| Server Management | Required          | None              |
| Scaling           | Manual            | Automatic         |
| Pricing           | Running Time      | Execution Time    |
| Maintenance       | Required          | AWS Managed       |
| Best For          | Long-running apps | Event-driven apps |

---

## Visual Comparison

```text
EC2

Create Server
      │
      ▼
Keep Running
      │
      ▼
Pay Continuously


Lambda

Trigger Event
      │
      ▼
Run Function
      │
      ▼
Destroy Compute
      │
      ▼
Pay Only For Execution
```

---

# 1️⃣3️⃣ Event-Driven Computing

Lambda runs when events occur.

---

## Common Triggers

```text
CloudWatch
S3
SNS
API Gateway
EventBridge
DynamoDB
```

---

## Architecture

```text
AWS Event
      │
      ▼
Lambda Triggered
      │
      ▼
Execute Code
      │
      ▼
Perform Action
```

---

# 1️⃣4️⃣ Lambda Components

## Lambda Handler

Entry point of Lambda.

```python
def lambda_handler(event, context):
```

Equivalent to:

```python
main()
```

in traditional programs.

---

## Environment Variables

Store configuration.

```text
DB_HOST
API_KEY
BUCKET_NAME
```

---

## IAM Role

Allows Lambda to access AWS services.

Example:

```text
Lambda
    │
    ▼
IAM Role
    │
    ▼
S3
EC2
SNS
CloudWatch
```

---

# 1️⃣5️⃣ Cloud Cost Optimization

## Why Organizations Move to Cloud

### Reason 1

Reduce Infrastructure Management

### Reason 2

Reduce Infrastructure Cost

---

## Problem

Developers create resources.

Resources remain unused.

AWS still charges.

---

# Examples of Stale Resources

```text
Unused EC2
Unused EBS
Old Snapshots
Unused Elastic IP
Unused Load Balancers
Unused S3 Buckets
Unused EKS Clusters
```

---

# Cost Optimization Lifecycle

```text
Resources Created
       │
       ▼
Become Unused
       │
       ▼
Still Generate Cost
       │
       ▼
Identify
       │
       ▼
Notify / Delete
```

---

# 1️⃣6️⃣ Real-World DevOps Project

## Project Goal

Delete stale EBS snapshots automatically.

---

## Problem Statement

Developer:

```text
Creates EC2
Creates Volume
Creates Snapshots
Deletes EC2
Forgets Snapshots
```

Result:

```text
AWS Charges Continue
```

---

# Architecture Diagram

```text
CloudWatch Schedule
          │
          ▼
Lambda Function
          │
          ▼
Python Script
          │
          ▼
Boto3
          │
          ▼
AWS APIs
          │
          ▼
Find Stale Snapshots
          │
          ▼
Delete Snapshots
```

---

# 1️⃣7️⃣ EBS Snapshot Cleanup Logic

## Algorithm

```text
Get Snapshots
      │
      ▼
Get Volumes
      │
      ▼
Get EC2 Instances
      │
      ▼
Check Snapshot Volume
      │
      ▼
Volume Exists?
      │
 ┌────┴─────┐
 │          │
No         Yes
 │          │
 ▼          ▼
Delete   Attached To EC2?
               │
        ┌──────┴──────┐
        │             │
       No            Yes
        │             │
        ▼             ▼
    Delete      Keep Snapshot
```

---

# Project Workflow

```text
Lambda Executes
      │
      ▼
List Instances
      │
      ▼
List Volumes
      │
      ▼
List Snapshots
      │
      ▼
Identify Stale Resources
      │
      ▼
Delete Stale Snapshots
```

---

# 1️⃣8️⃣ CloudWatch + Lambda

## Why CloudWatch?

Lambda is event-driven.

Needs a trigger.

---

## Schedule Example

Every day:

```text
10:00 AM
```

CloudWatch triggers Lambda.

---

## Workflow

```text
CloudWatch Cron
        │
        ▼
Lambda Trigger
        │
        ▼
Boto3 Script
        │
        ▼
Cleanup Resources
```

---

# Cron Scheduling Diagram

```text
Every Day 10 AM
        │
        ▼
CloudWatch Rule
        │
        ▼
Lambda
        │
        ▼
Cost Optimization
```

---

# DevOps Use Cases of Lambda

## Cost Optimization

```text
Unused Snapshots
Unused Volumes
Unused EC2
```

---

## Security Monitoring

```text
Public S3 Buckets
GP2 Volumes
Misconfigured IAM
```

---

## Compliance

```text
Resource Validation
Policy Enforcement
Audit Automation
```

---

## Notifications

```text
SNS
Email Alerts
Slack Alerts
```

---

# 🎯 Interview Questions

---

## What is Boto3?

Python SDK for AWS used to interact with AWS services programmatically.

---

## Why use Boto3 instead of AWS CLI?

Boto3 supports:

* Complex automation
* Python logic
* Lambda integration

---

## Difference between Client and Resource?

Client is low-level and recommended.

Resource is higher-level but no longer actively expanded.

---

## What is Lambda?

Serverless compute service that executes code without managing servers.

---

## Difference between EC2 and Lambda?

EC2:

* Persistent server

Lambda:

* Event-driven execution

---

## Why use Lambda for Cost Optimization?

Because:

* Auto-scaling
* Pay per execution
* No infrastructure management

---

## How does Lambda access AWS services?

Through IAM Roles.

---

## What triggers Lambda?

* CloudWatch
* S3
* SNS
* EventBridge
* API Gateway

---

# 📝 Quick Revision Sheet

## Boto3

```text
Python SDK for AWS
```

---

## Install

```bash
pip install boto3
```

---

## Authentication

```bash
aws configure
```

---

## Create Client

```python
boto3.client("service")
```

---

## Lambda

```text
Serverless Compute Service
```

---

## Lambda Handler

```python
def lambda_handler(event, context)
```

---

## Cost Optimization Flow

```text
CloudWatch
    │
    ▼
Lambda
    │
    ▼
Boto3
    │
    ▼
Find Stale Resources
    │
    ▼
Delete Resources
```

---

# 🏆 Key Takeaways

1. Boto3 is the primary Python SDK for AWS automation.
2. Learn AWS manually before automating it.
3. Use Boto3 for scripting and Terraform/CloudFormation for Infrastructure as Code.
4. Lambda is a serverless compute platform.
5. Lambda functions are event-driven.
6. IAM Roles control Lambda permissions.
7. CloudWatch can schedule Lambda execution.
8. Cost optimization is a major DevOps responsibility.
9. Boto3 + Lambda + CloudWatch forms a powerful automation ecosystem.
10. Real-world DevOps engineers frequently automate stale resource cleanup to reduce cloud costs.
