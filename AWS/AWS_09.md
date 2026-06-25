# Day 10 — AWS CLI Deep Dive

## Concept, Installation, Configuration & Practical Demo

> **Topic:** AWS Command Line Interface (AWS CLI)

---

# 📖 Table of Contents

1. Introduction to AWS CLI
2. Why AWS CLI is Needed
3. UI vs API Approach
4. What is an API?
5. AWS Automation Tools Overview
6. Understanding AWS CLI Architecture
7. AWS CLI Installation
8. AWS CLI Authentication
9. AWS CLI Commands & Practical Examples
10. AWS CLI Documentation & Command Reference
11. AWS CLI vs Terraform vs CloudFormation
12. Real-World DevOps Usage
13. Key Takeaways

---

# 1️⃣ Introduction to AWS CLI

Until now, AWS resources were created using:

* AWS Console (UI)
* Manual clicks and configurations

Examples:

* EC2 Instances
* VPCs
* S3 Buckets
* Networking Components

### Traditional Approach

```text
User
  │
  ▼
AWS Console (UI)
  │
  ▼
AWS Resources
```

While useful for learning, this approach becomes inefficient in real-world DevOps environments.

---

# 2️⃣ Why AWS CLI is Needed

## Problem with AWS Console (UI)

Imagine receiving requests like:

| Task                 | Quantity |
| -------------------- | -------- |
| Create VPCs          | 10       |
| Create EC2 Instances | 15       |
| Create S3 Buckets    | 20       |

### Using UI

```text
Login
  ↓
Create Resource
  ↓
Repeat
  ↓
Repeat
  ↓
Repeat
```

This process becomes:

* Time-consuming
* Error-prone
* Difficult to scale
* Not automation-friendly

---

## DevOps Reality

A DevOps Engineer's responsibility includes:

### Infrastructure Management

and

### Infrastructure Automation

Creating resources manually every day is not practical.

---

# 3️⃣ UI vs API Approach

## UI-Based Access

```text
User
 │
 ▼
AWS Console
 │
 ▼
AWS Services
```

Characteristics:

✅ Easy for beginners

❌ Slow

❌ Manual

❌ Not scalable

❌ Difficult to automate

---

## API-Based Access

```text
User Program
      │
      ▼
 AWS API
      │
      ▼
 AWS Services
```

Characteristics:

✅ Fast

✅ Automation Friendly

✅ Scalable

✅ Used in Real Projects

---

# 4️⃣ What is an API?

## API = Application Programming Interface

```text
A → Application

P → Programming

I → Interface
```

An API allows applications to communicate programmatically.

---

## Example

Suppose AWS exposes an API:

```text
POST /s3/create
```

Input:

```json
{
  "bucketName": "demo-bucket",
  "versioning": true
}
```

AWS creates the bucket and returns a response.

---

## Programmatic Access

Instead of:

```text
Login → Click → Create
```

We do:

```text
Code → API Call → Resource Created
```

Possible languages:

* Python
* Shell Script
* Java
* Go
* PowerShell

---

# 5️⃣ AWS Automation Tools Overview

AWS APIs can be consumed through multiple tools.

```text
AWS APIs
   │
   ├── AWS CLI
   ├── Terraform
   ├── CloudFormation
   └── AWS CDK
```

---

## Popular Tools

| Tool           | Purpose                                    |
| -------------- | ------------------------------------------ |
| AWS CLI        | Quick commands                             |
| Terraform      | Infrastructure as Code                     |
| CloudFormation | AWS Native IaC                             |
| AWS CDK        | Infrastructure using Programming Languages |

---

# 6️⃣ Understanding AWS CLI Architecture

AWS CLI is essentially:

> A Python utility developed and maintained by AWS.

---

## Internal Working

### Without AWS CLI

```text
Developer
   │
   ▼
Write Code
   │
   ▼
Call AWS APIs
   │
   ▼
Pass JSON Requests
   │
   ▼
AWS
```

Complex and requires programming knowledge.

---

### With AWS CLI

```text
Developer
     │
     ▼
 AWS CLI Command
     │
     ▼
 AWS CLI
     │
     ▼
 AWS API
     │
     ▼
 AWS Resources
```

AWS CLI acts as an **abstraction layer**.

---

## AWS CLI as Translator

```text
User Command
      │
      ▼
 AWS CLI
      │ Converts
      ▼
 AWS API Request
      │
      ▼
 AWS Service
```

---

# 7️⃣ Installing AWS CLI

---

## Linux

Follow AWS official documentation.

---

## macOS

### Command Line Installer

```bash
curl "<AWS CLI URL>" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /
```

---

## Windows

Recommended:

### Option 1

AWS CLI MSI Installer

### Option 2

Git Bash

```text
Windows
   │
   └── Git Bash
```

Useful for:

* SSH
* Linux Commands
* AWS CLI

---

## Alternative for Windows Users

Install:

```text
Oracle VirtualBox
      │
      ▼
Ubuntu/Linux VM
      │
      ▼
Practice DevOps Tools
```

---

## Verify Installation

```bash
aws --version
```

Example:

```bash
aws-cli/2.x.x
```

---

### Important Requirement

AWS CLI v2 requires Python.

Verify:

```bash
python --version
```

---

# 8️⃣ AWS CLI Authentication

Installing CLI alone is not enough.

AWS CLI must know:

> Which AWS account should it connect to?

---

## Configure AWS CLI

```bash
aws configure
```

---

AWS asks for:

```text
AWS Access Key ID
AWS Secret Access Key
Default Region
Output Format
```

---

## Authentication Flow

```text
AWS CLI
    │
    ▼
Access Key
    +
Secret Key
    │
    ▼
AWS Account
```

---

## Access Keys

Equivalent of:

```text
Username + Password
```

for API access.

---

## Location in AWS Console

```text
Profile
   │
   ▼
Security Credentials
   │
   ▼
Access Keys
```

---

### Important Security Note

⚠️ Never share:

* Access Key
* Secret Access Key

Anyone possessing them may gain access to your AWS account.

---

## Recommended Practice

### Learning

```text
Root User (acceptable)
```

### Organizations

```text
IAM Users (recommended)
```

Never use Root User in production environments.

---

## Example Configuration

```bash
aws configure
```

Input:

```text
AWS Access Key ID: XXXXXXXXX
AWS Secret Access Key: XXXXXXXXX
Default Region: us-east-1
Output Format: json
```

---

# 9️⃣ Practical AWS CLI Commands

---

## List S3 Buckets

```bash
aws s3 ls
```

Output:

```text
2024-01-01 demo-bucket
```

---

### Flow

```text
aws s3 ls
     │
     ▼
 AWS CLI
     │
     ▼
 AWS S3 API
     │
     ▼
 Bucket List Returned
```

---

## Create EC2 Instance

Example:

```bash
aws ec2 run-instances \
--image-id ami-xxxxxx \
--instance-type t2.micro \
--subnet-id subnet-xxxxxx \
--security-group-ids sg-xxxxxx
```

---

## What Happens?

```text
CLI Command
      │
      ▼
AWS CLI
      │
      ▼
EC2 API
      │
      ▼
Instance Created
```

---

## Output Format

Since output format is configured as JSON:

```json
{
  "Instances": [
    {
      "InstanceId": "i-123456"
    }
  ]
}
```

---

# 1️⃣0️⃣ AWS CLI Documentation & Reference

One of the most important skills:

> Learning how to read AWS CLI documentation.

---

## AWS CLI Reference

Search:

```text
AWS CLI S3 Reference
```

or

```text
AWS CLI EC2 Reference
```

---

## S3 Commands

Examples:

```bash
aws s3 ls
```

List buckets.

---

```bash
aws s3 mb s3://bucket-name
```

Create bucket.

---

### S3 Command Structure

```text
aws
 │
 ├── s3
 │
 ├── ls
 │
 └── mb
```

---

## EC2 Reference

Search:

```text
Run Instances
```

AWS documentation provides:

* Required parameters
* Optional parameters
* Examples
* Outputs

---

# Error Handling

Missing parameter example:

```bash
aws ec2 run-instances
```

Output:

```text
MissingParameter:
The request must contain parameter imageId
```

This helps identify configuration mistakes.

---

# 1️⃣1️⃣ AWS CLI vs Terraform vs CloudFormation

---

## AWS CLI

### Best For

* Quick commands
* Resource inspection
* Fast troubleshooting

Example:

```bash
aws s3 ls
```

---

## Terraform

### Best For

* Large infrastructures
* Repeatable deployments
* Infrastructure as Code

---

## CloudFormation

### Best For

* AWS-native infrastructure automation

---

## Comparison Chart

| Feature              | AWS CLI | Terraform | CloudFormation |
| -------------------- | ------- | --------- | -------------- |
| Easy Commands        | ✅       | ❌         | ❌              |
| Quick Operations     | ✅       | ❌         | ❌              |
| Large Infrastructure | ❌       | ✅         | ✅              |
| IaC Support          | ❌       | ✅         | ✅              |
| Code Review          | ❌       | ✅         | ✅              |
| Stack Management     | ❌       | ✅         | ✅              |

---

# Decision Matrix

```text
Need quick information?
        │
        ▼
      AWS CLI

Need full infrastructure?
        │
        ▼
Terraform / CloudFormation
```

---

# 1️⃣2️⃣ Real-World DevOps Usage

A DevOps Engineer frequently uses CLI for:

---

## Listing Resources

```bash
aws s3 ls
```

```bash
aws ec2 describe-instances
```

```bash
aws iam list-users
```

---

## Troubleshooting

```bash
aws logs
```

```bash
aws cloudwatch
```

---

## Quick Automation

```bash
Shell Script
      │
      ▼
AWS CLI Commands
      │
      ▼
AWS Resources
```

---

## Why Engineers Love CLI

Because:

```text
Speed = Productivity
```

Instead of:

```text
Login
 → Navigate
 → Search
 → Click
```

You can simply run:

```bash
aws s3 ls
```

and get results instantly.

---

# 1️⃣3️⃣ Key Takeaways

## AWS CLI Summary

✅ AWS CLI is a Python-based utility.

✅ AWS CLI acts as a bridge between users and AWS APIs.

✅ AWS CLI converts commands into API calls.

✅ AWS CLI enables automation.

✅ Configure CLI using:

```bash
aws configure
```

✅ Verify installation:

```bash
aws --version
```

✅ List buckets:

```bash
aws s3 ls
```

✅ Create resources using service-specific commands.

✅ Learn commands from AWS CLI Reference Documentation.

✅ Use AWS CLI for quick tasks.

✅ Use Terraform or CloudFormation for large-scale infrastructure.

---

# Final Architecture Diagram

```text
                    AWS CLI Ecosystem

        ┌─────────────────────────────┐
        │         DevOps User         │
        └─────────────┬───────────────┘
                      │
                      ▼
        ┌─────────────────────────────┐
        │      AWS CLI Commands       │
        │  aws s3 ls                  │
        │  aws ec2 run-instances      │
        └─────────────┬───────────────┘
                      │
                      ▼
        ┌─────────────────────────────┐
        │         AWS CLI             │
        │ (Python-based Utility)      │
        └─────────────┬───────────────┘
                      │
                      ▼
        ┌─────────────────────────────┐
        │         AWS APIs            │
        └─────────────┬───────────────┘
                      │
                      ▼
        ┌─────────────────────────────┐
        │       AWS Services          │
        │ EC2 │ S3 │ VPC │ IAM │ RDS │
        └─────────────────────────────┘
```

---

## Interview Questions

### Q1. What is AWS CLI?

AWS CLI is a Python-based command-line utility that allows users to interact with AWS services through commands instead of the AWS Console.

---

### Q2. Why is AWS CLI important?

It enables automation, faster operations, scripting, and efficient infrastructure management.

---

### Q3. What command is used to configure AWS CLI?

```bash
aws configure
```

---

### Q4. Which credentials are required?

* Access Key ID
* Secret Access Key

---

### Q5. What is the difference between AWS CLI and Terraform?

AWS CLI is best for quick operations, while Terraform is designed for Infrastructure as Code and large-scale deployments.

---

# Golden Rule for DevOps Engineers

> **Use AWS CLI when you need speed. Use Terraform/CloudFormation when you need repeatability, scalability, and infrastructure as code.** 🚀
