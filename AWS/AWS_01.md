# ☁️ Day 1: Introduction to AWS & Cloud Computing

## 🎯 Learning Objectives

By the end of this lesson, you should understand:

* What is Cloud Computing?
* Why Cloud Computing was introduced?
* Difference between Public Cloud and Private Cloud
* Why Public Cloud became popular
* Why AWS dominates the Cloud market
* What is Cloud Repatriation?
* How to create an AWS Account

---

# 1️⃣ Understanding Cloud Computing

## Traditional Infrastructure Model

Before cloud computing became popular, organizations used to:

1. Purchase physical servers from vendors like IBM, HP, Dell, etc.
2. Build their own Data Centers.
3. Deploy applications directly on these servers.
4. Hire teams to manage and maintain the infrastructure.

### Traditional Setup

```text
+----------------------+
|     Organization     |
+----------------------+
           |
           v
+----------------------+
|   Physical Servers   |
| (IBM / HP / Dell)    |
+----------------------+
           |
           v
+----------------------+
|     Data Center      |
+----------------------+
           |
           v
+----------------------+
|    Applications      |
+----------------------+
```

---

## What is a Data Center?

A **Data Center** is a facility that houses:

* Physical servers
* Networking equipment
* Storage systems
* Power backup systems
* Cooling systems
* Security infrastructure

### Responsibilities

✅ Server Maintenance

✅ Networking

✅ Power Management

✅ Temperature Control

✅ Hardware Upgrades

✅ Security

---

# 2️⃣ The Problem with Traditional Infrastructure

Consider a server with:

| Resource | Capacity |
| -------- | -------- |
| CPU      | 100 CPUs |
| RAM      | 100 GB   |

If one application uses:

| Resource | Used  |
| -------- | ----- |
| CPU      | 1 CPU |
| RAM      | 1 GB  |

### Resource Utilization

```text
Server Capacity

CPU : [■□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□] 1%
RAM : [■□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□□] 1%

Unused Resources = 99%
```

### Problems

❌ Expensive hardware

❌ Poor utilization

❌ Infrastructure wastage

❌ High operational costs

---

# 3️⃣ Virtualization – The Foundation of Cloud

To solve resource wastage, **Virtualization** was introduced.

---

## What is Virtualization?

Virtualization allows multiple virtual servers (VMs) to run on a single physical server.

### Without Virtualization

```text
Physical Server
      |
      v
  Application
```

Huge resource wastage.

---

### With Virtualization

```text
+--------------------------------+
|      Physical Server           |
| 100 CPU | 100 GB RAM           |
+--------------------------------+
            |
            v
+--------------------------------+
|      Hypervisor Layer          |
+--------------------------------+
      |      |      |      |
      v      v      v      v
    VM1    VM2    VM3    VM4
     |      |      |      |
   App1   App2   App3   App4
```

### Benefits

✅ Better utilization

✅ Cost reduction

✅ Multiple applications per server

✅ Easy scalability

---

# 4️⃣ What is Cloud?

Once virtualization became common:

* Organizations could create virtual machines.
* These VMs could be accessed remotely.
* Users no longer needed to know where the physical server existed.

This gave rise to the concept of **Cloud Computing**.

---

## Simple Definition

> Cloud Computing is the delivery of computing resources such as servers, storage, databases, networking, and software over the internet on-demand.

---

### Cloud Concept

```text
        Developers
             |
             v
      Request Server
             |
             v
      +--------------+
      |    Cloud     |
      +--------------+
             |
             v
      Virtual Machine
```

The user consumes resources without worrying about:

* Hardware
* Networking
* Maintenance
* Physical location

---

# 5️⃣ Private Cloud

## Definition

A cloud infrastructure built, owned, and managed by an organization for its internal use.

---

### Architecture

```text
Organization
      |
      v
+---------------------+
|   Own Data Center   |
+---------------------+
      |
      v
+---------------------+
| Virtualization      |
| VMware/OpenStack    |
+---------------------+
      |
      v
 Virtual Machines
```

---

## Characteristics

✅ Full control

✅ High customization

✅ Better compliance

✅ Internal security control

---

## Examples

* VMware
* OpenStack
* Xen

---

# 6️⃣ Public Cloud

## Definition

A cloud platform owned and managed by cloud providers where anyone can consume resources on demand.

---

### Public Cloud Providers

| Provider     | Platform           |
| ------------ | ------------------ |
| Amazon       | AWS                |
| Microsoft    | Azure              |
| Google       | GCP                |
| Oracle       | OCI                |
| DigitalOcean | DigitalOcean Cloud |

---

### Public Cloud Architecture

```text
               AWS
                |
    +-----------+-----------+
    |           |           |
    v           v           v

Company A   Company B   Company C
```

All companies share the provider's infrastructure while remaining logically isolated.

---

## Characteristics

✅ Pay as you go

✅ No infrastructure management

✅ Highly scalable

✅ Global availability

✅ Quick provisioning

---

# 7️⃣ Public Cloud vs Private Cloud

| Feature               | Private Cloud | Public Cloud   |
| --------------------- | ------------- | -------------- |
| Ownership             | Organization  | Cloud Provider |
| Maintenance           | Organization  | Provider       |
| Initial Cost          | Very High     | Low            |
| Scalability           | Limited       | Very High      |
| Setup Time            | Weeks/Months  | Minutes        |
| Accessibility         | Internal      | Global         |
| Resource Provisioning | Complex       | Easy           |

---

### Visual Comparison

```text
PRIVATE CLOUD

Organization
      |
      v
Own Data Center
      |
      v
Servers + Network + Storage


PUBLIC CLOUD

Organization
      |
      v
AWS / Azure / GCP
      |
      v
Ready-to-use Resources
```

---

# 8️⃣ Why Public Cloud Became So Popular

Many people think the answer is cost.

The biggest reason is actually:

## Reduced Operational Overhead

---

### Traditional Approach

```text
Need Servers
      +
Need Networking
      +
Need Cooling
      +
Need Security
      +
Need Maintenance Team
      +
Need Upgrades
```

---

### Public Cloud Approach

```text
Create Account
      |
      v
Launch Resource
      |
      v
Start Using
```

---

## Benefits

### 1. No Data Center Management

Cloud providers handle:

* Power
* Cooling
* Hardware
* Security
* Upgrades

---

### 2. Fast Provisioning

```text
Traditional Setup
     Days / Weeks

Cloud Setup
     Minutes
```

---

### 3. Elastic Scaling

```text
Traffic Increase
       |
       v
Add More Servers

Traffic Decrease
       |
       v
Remove Servers
```

---

### 4. Pay-As-You-Go

```text
Use Resource
      |
      v
Pay Only For Usage
```

---

# 9️⃣ AWS Services Growth

Initially AWS started with a few services:

* Virtual Machines
* Storage
* Networking

Today AWS offers **200+ services**.

---

## Examples of AWS Services

| Category   | Service |
| ---------- | ------- |
| Compute    | EC2     |
| Storage    | S3      |
| Database   | RDS     |
| Containers | EKS     |
| Serverless | Lambda  |
| Networking | VPC     |

---

# 🔟 Why AWS is the Most Popular Cloud Platform

## 1. First-Mover Advantage

AWS pioneered cloud computing.

```text
AWS Started First
        |
        v
More Customers
        |
        v
More Adoption
        |
        v
More Jobs
```

---

## 2. Largest Market Share

```text
Cloud Market Share (Approximate Ranking)

1. AWS
2. Azure
3. GCP
```

---

## 3. Largest Ecosystem

AWS provides:

* Extensive documentation
* Large community support
* Massive service portfolio

---

## 4. Better Career Opportunities

Because many companies use AWS:

```text
More AWS Adoption
       |
       v
More AWS Jobs
       |
       v
More Demand for AWS Skills
```

---

# 1️⃣1️⃣ Can Learning AWS Help in Learning Other Clouds?

### Yes.

Cloud concepts remain similar:

| Concept    | AWS | Azure           | GCP            |
| ---------- | --- | --------------- | -------------- |
| VM         | EC2 | Virtual Machine | Compute Engine |
| Storage    | S3  | Blob Storage    | Cloud Storage  |
| Kubernetes | EKS | AKS             | GKE            |

---

## Core Concepts Stay Same

✅ Networking

✅ Compute

✅ Storage

✅ Security

✅ Containers

Only names and implementation details differ.

---

# 1️⃣2️⃣ Cloud Repatriation

## What is Cloud Repatriation?

Moving workloads from Public Cloud back to Private Cloud or On-Premises infrastructure.

---

### Journey

```text
Private Cloud
      |
      v
Public Cloud
      |
      v
Private Cloud
```

---

## Why Some Companies Move Back?

### 1. Security Requirements

Sensitive industries:

* Banking
* Government
* Defense

---

### 2. Cost Optimization

For certain workloads:

```text
Long-Term Stable Workloads
        |
        v
Private Infrastructure
May Become Cheaper
```

---

### 3. Compliance Requirements

Some regulations require data to remain under organizational control.

---

## Important Reality

```text
Cloud Repatriation
≈ 1-2%

Public Cloud Adoption
≈ 98-99%
```

Cloud adoption continues to grow worldwide.

---

# 1️⃣3️⃣ Creating an AWS Account

## Step-by-Step Process

---

### Step 1

Visit:

```text
https://aws.amazon.com
```

Select:

```text
Create AWS Account
```

---

### Step 2

Provide:

* Email Address
* AWS Account Name

```text
Email Verification
       |
       v
Verification Code
       |
       v
Verify
```

---

### Step 3

Create Strong Password

Requirements:

✅ Uppercase

✅ Lowercase

✅ Numbers

✅ Special Characters

---

### Step 4

Choose Account Type

```text
Personal
or
Business
```

For learning purposes:

👉 Select **Personal**

---

### Step 5

Provide:

* Full Name
* Phone Number
* Country
* Address

---

### Step 6

Add Payment Method

AWS requires:

* Debit Card OR
* Credit Card

---

### Why?

To verify that:

```text
You Are A Real User
```

Not to charge immediately.

---

### Temporary Verification Charge

AWS may deduct a small amount temporarily:

```text
₹2 (India)
or
$1 (USA)
```

This is usually refunded.

---

### Important Note

AWS does **NOT automatically deduct charges** simply because a card is added.

If you exceed free-tier limits:

```text
AWS Generates Bill
       |
       v
You Must Pay
```

Otherwise:

```text
Account May Be Suspended
```

---

# 🏆 Key Takeaways

```text
Cloud Computing
    |
    +--> Virtualization
    |
    +--> Private Cloud
    |
    +--> Public Cloud
            |
            +--> AWS
            +--> Azure
            +--> GCP
```

### Remember

✅ Cloud emerged from virtualization.

✅ Public cloud removes infrastructure management overhead.

✅ AWS is the market leader because of first-mover advantage and extensive adoption.

✅ Cloud adoption continues to grow despite limited cloud repatriation.

✅ Learning AWS provides a strong foundation for all cloud platforms.

---

# 📌 Day 1 Summary

| Topic                   | Status |
| ----------------------- | ------ |
| What is Cloud?          | ✅      |
| Virtualization          | ✅      |
| Private Cloud           | ✅      |
| Public Cloud            | ✅      |
| Public vs Private Cloud | ✅      |
| Why Public Cloud?       | ✅      |
| Why AWS?                | ✅      |
| Cloud Repatriation      | ✅      |
| AWS Account Creation    | ✅      |

---

## 🚀 What's Next?

**Day 2:** AWS Global Infrastructure

Topics typically include:

* Regions
* Availability Zones (AZs)
* Edge Locations
* Global AWS Architecture

These concepts form the backbone of all AWS services and deployments.
