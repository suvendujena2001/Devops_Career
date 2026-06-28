# Day 11 Notes – Infrastructure as Code (IaC) with AWS CloudFormation (CFT)

---

# Agenda

* Introduction to Infrastructure as Code (IaC)
* AWS CloudFormation (CFT)
* AWS CLI vs CloudFormation
* CloudFormation Features
* CloudFormation Template Structure
* Writing CloudFormation Templates
* Drift Detection
* CloudFormation Stacks
* VS Code Tips & Plugins
* CloudFormation vs Terraform
* Assignment

---

# What is AWS CloudFormation (CFT)?

AWS CloudFormation (CFT) is an **Infrastructure as Code (IaC)** service that allows you to:

* Create AWS resources
* Update infrastructure
* Delete infrastructure
* Manage infrastructure consistently

Instead of manually creating resources through the AWS Console, you define infrastructure in a **template**.

Supported template formats:

* YAML (Recommended)
* JSON

---

# What is Infrastructure as Code (IaC)?

Infrastructure as Code means:

> **Writing code to create infrastructure instead of manually creating resources.**

Example:

Instead of manually creating:

* EC2
* S3
* VPC
* Route Tables
* IAM

Write one template and let CloudFormation create everything automatically.

---

# Principles of Infrastructure as Code

An IaC tool acts as a **middle layer** between:

```
User
   │
   ▼
IaC Tool
   │
   ▼
Cloud Provider APIs
```

Workflow:

```
User writes YAML/JSON Template
        │
        ▼
CloudFormation
        │
Converts template into AWS API Calls
        │
        ▼
AWS Resources Created
```

---

# Examples of IaC Tools

## Single Cloud

* AWS CloudFormation → AWS only
* Azure Resource Manager (ARM) → Azure only

## Multi Cloud

* Terraform
* Crossplane

---

# IaC Principles

## 1. Declarative

Declarative means:

> **What you write is what gets created.**

Example:

If the template contains:

* EC2
* VPC
* S3

Then those exact resources will exist after execution.

Benefits:

* Easy to understand
* Easy code review
* Easy auditing

---

## 2. Version Controlled

Templates should be stored in:

* Git
* S3
* Version Control Systems

Benefits:

* Track history
* Rollback changes
* Collaboration
* Code review

---

# AWS CLI vs CloudFormation

## Use AWS CLI When

* Quick commands
* Automation scripts
* Fetch resource information
* Small tasks

Examples:

* List S3 buckets
* Describe EC2 instances
* Delete one resource

---

## Use CloudFormation When

* Creating infrastructure
* Managing infrastructure
* Large deployments
* Repeatable environments

Examples:

* VPC
* EC2
* Load Balancer
* Route Tables
* Auto Scaling
* IAM Roles

---

# YAML vs JSON

## YAML (Recommended)

Advantages:

* Easier to read
* Supports comments
* Uses indentation
* Less complex
* Widely used in DevOps

Examples:

* Kubernetes
* Ansible
* CloudFormation

---

## JSON

Disadvantages:

* No comments
* More brackets
* Harder to read

Still fully supported by CloudFormation.

---

# CloudFormation Features

## Infrastructure Creation

Creates AWS resources automatically.

---

## Infrastructure Updates

Modify templates and update existing resources.

---

## Drift Detection

Detects manual changes made outside CloudFormation.

Example:

CloudFormation creates:

```
S3 Bucket
Versioning = Enabled
```

Someone manually changes:

```
Versioning = Suspended
```

CloudFormation detects the difference.

Benefits:

* Security
* Compliance
* Infrastructure consistency

---

# CloudFormation Stack

A **Stack** is the deployment unit in CloudFormation.

Workflow:

```
Write Template
       │
Upload Template
       │
Create Stack
       │
CloudFormation Executes
       │
AWS Resources Created
```

---

# Ways to Deploy Templates

* AWS Console
* AWS CLI
* Jenkins
* CI/CD Pipelines

---

# CloudFormation Template Structure

Basic structure:

```yaml
AWSTemplateFormatVersion:

Description:

Metadata:

Parameters:

Rules:

Mappings:

Conditions:

Resources:

Outputs:
```

Only **Resources** is mandatory.

---

# Template Components

## 1. AWSTemplateFormatVersion

CloudFormation template version.

Example:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
```

Standard version.

---

## 2. Description

Explains the purpose of the template.

Example:

```yaml
Description: Creates an EC2 instance
```

---

## 3. Metadata

Stores information like:

* Author
* Team
* Project
* Owner

---

## 4. Parameters

Runtime inputs.

Example:

Instead of hardcoding:

```yaml
ImageId: ami-12345
```

Use:

```yaml
Parameters:
  ImageId:
```

Then users supply the AMI during deployment.

Useful for:

* AMI IDs
* Instance Types
* Key Pairs
* Bucket Names

---

## 5. Rules

Validate user inputs.

Example:

Only allow:

```
t2.micro
t2.medium
```

Reject:

```
t2.xlarge
```

Useful for:

* Cost control
* Naming standards
* Validation

---

## 6. Mappings

Maps values to variables.

Used to:

* Store regional AMIs
* Environment configurations

---

## 7. Conditions

Conditional resource creation.

Example:

Only create resources when:

```
Environment = Production
```

---

## 8. Resources (Mandatory)

This is where AWS resources are defined.

Example:

```yaml
Resources:

  MyEC2:

    Type: AWS::EC2::Instance

    Properties:

      ImageId:

      InstanceType:
```

CloudFormation identifies resources using:

```yaml
Type:
```

Example:

```yaml
AWS::EC2::Instance
```

---

## 9. Outputs

Displays useful information after deployment.

Examples:

* Instance ID
* Public IP
* Bucket Name
* Load Balancer DNS

---

# Resource Naming

Example:

```yaml
Resources:

  MyBucket:

    Type: AWS::S3::Bucket

  MyEC2:

    Type: AWS::EC2::Instance
```

Resource names can be anything.

Examples:

```
MyBucket

Bucket1

ProductionBucket

AbhishekBucket
```

CloudFormation identifies the actual resource using:

```yaml
Type:
```

---

# Creating an S3 Bucket

Example:

```yaml
Resources:

  S3Bucket:

    Type: AWS::S3::Bucket

    Properties:

      BucketName:

        demo.aws.example.com
```

---

# Enabling Versioning

```yaml
VersioningConfiguration:

  Status: Enabled
```

---

# Drift Detection Demo

Scenario:

CloudFormation:

```
Versioning = Enabled
```

User manually changes:

```
Versioning = Suspended
```

Run:

```
Stack
    ↓
Detect Drift
```

CloudFormation reports:

```
Expected:

Enabled

Actual:

Suspended
```

This helps identify unauthorized manual changes.

---

# AWS CloudFormation Designer

CloudFormation provides a drag-and-drop designer.

Features:

* Drag resources
* Automatically generates YAML
* Automatically generates JSON

Good for beginners.

---

# AWS Documentation

The official AWS CloudFormation documentation contains:

* Resource references
* Examples
* Properties
* Syntax
* Best practices

Useful whenever writing templates.

---

# VS Code Extensions

## 1. YAML (Red Hat)

Benefits:

* YAML validation
* Auto indentation
* Auto completion
* Error highlighting

---

## 2. AWS Toolkit

Benefits:

* CloudFormation support
* AWS integration
* Resource assistance
* Auto completion
* Faster development

---

# Tips for Writing CloudFormation

* Prefer YAML over JSON.
* Use AWS documentation frequently.
* Don't memorize resource properties.
* Use VS Code extensions.
* Add descriptions.
* Parameterize reusable values.
* Keep templates in Git.
* Use comments (YAML).
* Test with small templates first.
* Start with S3 and EC2 before larger stacks.

---

# CloudFormation vs Terraform

## CloudFormation

Best when:

* AWS only
* Organization is fully on AWS

Advantages:

* Native AWS service
* Deep AWS integration
* No additional installation

---

## Terraform

Best when:

* AWS + Azure
* AWS + GCP
* Hybrid Cloud
* Multi-cloud

Advantages:

* Supports multiple cloud providers
* Industry standard
* Large ecosystem
* Portable infrastructure

Recommendation:

If learning only one IaC tool today:

> **Learn Terraform first.**

Learn CloudFormation if your organization exclusively uses AWS.

---

# Practical Workflow

```
Write YAML Template
        │
Validate
        │
Upload Template
        │
Create Stack
        │
CloudFormation
        │
AWS APIs
        │
Resources Created
```

---

# Key Takeaways

* CloudFormation is AWS's Infrastructure as Code service.
* Templates are written in YAML or JSON.
* YAML is preferred.
* CloudFormation converts templates into AWS API calls.
* Only the `Resources` section is mandatory.
* Use Parameters for reusable templates.
* Drift Detection identifies manual infrastructure changes.
* Stacks are deployment units.
* Store templates in Git.
* Use AWS documentation instead of memorizing syntax.
* Use AWS Toolkit + YAML extensions in VS Code.
* Prefer Terraform for multi-cloud environments.
* Prefer CloudFormation for AWS-only environments.

---

# Assignment

Create an **EC2 instance** using an AWS CloudFormation template.

Hints:

* Use YAML.
* Refer to AWS CloudFormation documentation.
* Use the CloudFormation Designer if needed.
* Deploy it as a CloudFormation Stack.
