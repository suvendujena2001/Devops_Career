# AWS IAM Deep Dive (Day 2) – Complete Notes with Practical Insights

> **Topic:** AWS Identity and Access Management (IAM)
> **Objective:** Understand Authentication, Authorization, Users, Groups, Policies, and Roles in AWS with practical examples.

---

# 📖 Recap of Day 1

Before diving into IAM, let's recall the foundational concepts:

```text
Cloud Computing
     │
     ├── Private Cloud
     │      └── Used internally by organizations
     │
     └── Public Cloud
            └── Services provided over the internet
```

### Topics Covered Previously

* What is Cloud Computing?
* Private Cloud vs Public Cloud
* Why Public Cloud is important
* Why AWS is the leading cloud provider

---

# 🎯 What is AWS IAM?

## IAM = Identity and Access Management

IAM is an AWS service that helps you:

1. **Authenticate** users
2. **Authorize** users

In simple words:

> IAM determines **WHO** can access AWS and **WHAT** they can do inside AWS.

---

# 🏦 Real-Life Example: Bank Security System

Imagine a bank.

```text
+------------------------------------------------+
|                    BANK                        |
+------------------------------------------------+
|                                                |
|  Service Desk       Employee Area              |
|                                                |
|                                                |
|                 Sensitive Area                 |
|       (Documents, Cash, Financial Data)        |
|                                                |
+------------------------------------------------+
```

The bank follows two security checks:

## Step 1: Authentication

Before entering the bank:

> "Who are you?"

The bank verifies your identity.

Examples:

* ATM Card
* Account Number
* Identity Proof

---

## Step 2: Authorization

After entering:

> "What are you allowed to do?"

Examples:

| Person   | Allowed Access |
| -------- | -------------- |
| Customer | Service Desk   |
| Employee | Employee Area  |
| Manager  | Sensitive Area |

---

## Why Is This Important?

Without Authentication & Authorization:

```text
Anyone
   │
   ▼
Enter Bank
   │
   ▼
Access Cash/Documents
   │
   ▼
Security Breach 🚨
```

---

# ☁️ AWS Equivalent

Imagine an organization has an AWS account.

```text
AWS Account
│
├── EC2 (Servers)
├── S3 (Storage)
├── Databases
└── Kubernetes Services
```

Without IAM:

```text
AWS Root User
      │
      ├── Shared with Everyone ❌
      │
      ├── Can Delete EC2
      ├── Can Delete Databases
      ├── Can Delete Storage
      └── Can Destroy Entire Infrastructure
```

This is extremely dangerous.

---

# How IAM Solves This Problem

Instead of sharing the Root Account:

```text
AWS Account
      │
      ▼
     IAM
      │
      ├── Users
      ├── Groups
      ├── Policies
      └── Roles
```

---

# IAM Core Components

```text
IAM
│
├── Users
├── Policies
├── Groups
└── Roles
```

Let's understand each one.

---

# 👤 IAM Users

## What is a User?

A User represents:

> A person who needs access to AWS.

Examples:

* Developer
* QA Engineer
* DevOps Engineer
* Database Administrator

---

### User Creation Flow

```text
Employee Joins Company
          │
          ▼
Requests AWS Access
          │
          ▼
DevOps Team Creates IAM User
          │
          ▼
Username + Password Shared
```

Example:

```text
test-user-501
```

---

# 🔐 Authentication Using Users

Users solve:

## Authentication

```text
WHO can log in?
```

Example:

```text
AWS Account
      │
      ▼
 IAM User
      │
      ▼
Login Allowed
```

Without permissions, the user can log in but cannot do anything.

---

# 📜 IAM Policies

## What is a Policy?

A Policy defines:

> What actions a user is allowed to perform.

---

### Policies Handle Authorization

```text
User Logs In
      │
      ▼
Policy Checks Permissions
      │
      ▼
Allowed / Denied
```

---

### Examples

| Policy          | Permission                 |
| --------------- | -------------------------- |
| S3 Read Only    | View buckets               |
| S3 Full Access  | Create/Delete/View buckets |
| EC2 Full Access | Manage EC2 instances       |

---

### Example Scenario

User:

```text
test-user-501
```

Permissions:

```text
Read Database
Access Kubernetes
```

Not Allowed:

```text
Delete Database
```

---

# Authentication vs Authorization

| Authentication | Authorization      |
| -------------- | ------------------ |
| Who are you?   | What can you do?   |
| IAM User       | IAM Policy         |
| Login Process  | Permission Process |

---

## Visualization

```text
             IAM
              │
              ▼

      +------------------+
      | Authentication   |
      | (IAM User)       |
      +------------------+
               │
               ▼
      +------------------+
      | Authorization    |
      | (IAM Policy)     |
      +------------------+
               │
               ▼
       AWS Resource Access
```

---

# 👥 IAM Groups

## Why Groups Are Needed?

Imagine:

```text
User 501
User 502
User 503
User 504
```

All are Developers.

If permissions are assigned individually:

```text
User 501 → Policy
User 502 → Policy
User 503 → Policy
User 504 → Policy
```

Lots of manual work.

---

# Better Solution: Groups

Create a Group:

```text
Developers Group
```

Attach permissions once.

```text
Developers Group
        │
        ├── User 501
        ├── User 502
        ├── User 503
        └── User 504
```

---

# Group Architecture

```text
                   IAM Group
                         │
      +------------------+------------------+
      │                                     │
      ▼                                     ▼
 S3 Full Access                     EC2 Read Access

      │
      ▼

+---------+---------+---------+---------+
| User501 | User502 | User503 | User504 |
+---------+---------+---------+---------+
```

---

# Benefits of Groups

### Without Groups

```text
Attach Policy
      │
      ├── User501
      ├── User502
      ├── User503
      └── User504
```

Need to update every user separately.

---

### With Groups

```text
Update Group Policy Once
            │
            ▼
All Users Updated Automatically
```

---

# Recommended Organizational Structure

```text
Company
│
├── Developers
├── QA Engineers
├── DB Admins
└── Others
```

Corresponding IAM Groups:

```text
IAM Groups
│
├── Dev
├── QA
├── DBA
└── Others
```

---

# 🎭 IAM Roles

Roles are often confusing initially.

Let's simplify.

---

## Users vs Roles

### User

```text
Human Being
      │
      ▼
AWS Access
```

Examples:

* Developer
* QA Engineer
* DevOps Engineer

---

### Role

```text
Application
     │
     ▼
AWS Access
```

Roles are generally used by:

* Applications
* AWS Services
* External Services
* Other AWS Accounts

---

# Example: Application Accessing AWS

```text
Customer
    │
    ▼
Application
    │
    ▼
AWS Database
```

The application needs AWS access.

Should we create an IAM User?

❌ No

Instead:

✅ Create an IAM Role

---

## Role Flow

```text
Application
      │
      ▼
Assumes IAM Role
      │
      ▼
Gets Temporary Credentials
      │
      ▼
Access AWS Resources
```

---

# Why Roles?

### Security Benefits

Roles provide:

* Temporary Credentials
* Better Security
* No hardcoded passwords
* Easier AWS integrations

---

# Role Use Cases

## 1. Applications Accessing AWS

```text
Application
    │
    ▼
IAM Role
    │
    ▼
S3 / Database
```

---

## 2. AWS Service-to-Service Communication

```text
EC2
 │
 ▼
IAM Role
 │
 ▼
S3
```

---

## 3. Cross-Account Access

```text
AWS Account A
       │
       ▼
     Role
       │
       ▼
AWS Account B
```

---

# Practical Demonstration Summary

---

# Step 1: Login Using Root User

```text
AWS Console
     │
     ▼
Root Account Login
```

Root User has:

```text
Full Permissions
```

Can:

* Create Resources
* Delete Resources
* Manage Everything

---

# Step 2: Create IAM User

Navigate:

```text
IAM
  │
  ▼
Users
  │
  ▼
Create User
```

Example:

```text
test-user-501
```

Enable:

✅ AWS Console Access

Select:

✅ Auto Generated Password

Enable:

✅ User must reset password at first login

---

# Step 3: Login as IAM User

```text
Account ID
Username
Password
```

Result:

```text
Login Successful
```

---

# Step 4: Observe Permission Errors

Attempt:

```text
Open S3
Open EC2
```

Result:

```text
Access Denied
```

Reason:

```text
User Exists
But No Policies Attached
```

---

# Learning

```text
Authentication ✅
Authorization ❌
```

---

# Step 5: Attach Policy

Navigate:

```text
IAM
 │
 ▼
Users
 │
 ▼
test-user-501
 │
 ▼
Add Permissions
```

Attach:

```text
AmazonS3FullAccess
```

---

# Step 6: Test Again

Login as:

```text
test-user-501
```

Navigate:

```text
S3
```

Now:

✅ View Buckets

✅ Create Buckets

✅ Manage S3 Resources

---

# What Did We Learn?

```text
User + Policy
       │
       ▼
Authentication + Authorization
```

---

# AWS Managed Policies

AWS provides predefined policies.

Examples:

| Policy                 | Description         |
| ---------------------- | ------------------- |
| AmazonS3FullAccess     | Full S3 permissions |
| AmazonS3ReadOnlyAccess | Read-only S3 access |
| AmazonEC2FullAccess    | Full EC2 access     |

---

# Custom Policies

Sometimes AWS managed policies are not enough.

You can create:

```text
Custom Policies
```

Written in:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
  ]
}
```

---

# Policy Structure

```text
Policy
│
├── Version
└── Statement
      │
      ├── Effect
      ├── Action
      └── Resource
```

---

## Components Explained

| Component | Purpose         |
| --------- | --------------- |
| Version   | Policy version  |
| Effect    | Allow / Deny    |
| Action    | AWS actions     |
| Resource  | Target resource |

---

# Complete IAM Architecture

```text
                         AWS Account
                              │
                              ▼
                     IAM Service
                              │
     ┌────────────────────────┼────────────────────────┐
     │                        │                        │
     ▼                        ▼                        ▼
   Users                   Groups                   Roles
     │                        │                        │
     ▼                        ▼                        ▼
 Authentication       Permission Mgmt      Service Access
     │                        │                        │
     └─────────────── Policies ────────────────┘
                              │
                              ▼
                    AWS Resource Access
```

---

# Best Practices

## ✅ Use IAM Users Instead of Root User

Never share root credentials.

---

## ✅ Use Groups

Manage permissions efficiently.

---

## ✅ Follow Least Privilege Principle

Grant only required permissions.

```text
Need Read Access?
      │
      ▼
Give Read Access Only
```

---

## ✅ Use AWS Managed Policies Initially

Simplifies learning and administration.

---

## ✅ Use Roles for Applications

Never hardcode AWS credentials.

---

## ✅ Enable Password Reset

Force users to set secure passwords.

---

# Assignment (Hands-On Practice)

### Task 1

Create an IAM User:

```text
test-user
```

---

### Task 2

Login as the IAM User.

Observe:

```text
Access Denied
```

for most services.

---

### Task 3

Attach:

```text
AmazonS3FullAccess
```

---

### Task 4

Login again.

Verify:

* View Buckets
* Create Bucket

---

### Task 5

Create a Group:

```text
Development-Team
```

Attach:

```text
AmazonS3FullAccess
```

Add users to the group.

---

# Key Takeaways

### IAM Fundamentals

```text
User      = Authentication
Policy    = Authorization
Group     = Permission Management
Role      = Temporary Access for Services
```

---

# Quick Revision Mind Map

```text
AWS IAM
│
├── Users
│     └── Authentication
│
├── Policies
│     └── Authorization
│
├── Groups
│     └── Permission Management
│
└── Roles
      └── Temporary Access
           ├── Applications
           ├── AWS Services
           └── Cross-Account Access
```

---

## One-Line Summary

> **AWS IAM is the security foundation of AWS that enables Authentication (who can access AWS) and Authorization (what they can do) through Users, Policies, Groups, and Roles.** 🚀
