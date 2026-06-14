# AWS EC2 Deep Dive & Jenkins Deployment on AWS

### Day 3 Notes – AWS Zero to Hero Series

> **Topic:** AWS EC2 Fundamentals + Deploying Jenkins on EC2
> **Objective:** Understand EC2 concepts, regions, availability zones, instance types, and deploy Jenkins on an AWS EC2 instance.

---

# 📌 Table of Contents

1. Introduction to EC2
2. Why EC2?
3. Understanding Elastic Cloud Compute
4. Virtualization & Hypervisors
5. Types of EC2 Instances
6. AWS Regions & Availability Zones
7. Creating an EC2 Instance
8. Connecting to EC2 via SSH
9. Deploying Jenkins on EC2
10. Accessing Jenkins from the Internet
11. Key Interview Questions
12. Architecture Diagrams
13. Summary

---

# 1️⃣ What is EC2?

**EC2 = Elastic Cloud Compute**

EC2 is one of AWS's most popular services that allows users to create and manage **virtual servers** in the cloud.

### Definition

> EC2 is a service that provides scalable virtual machines (VMs) on AWS infrastructure.

---

## Breaking Down the Term EC2

| Term    | Meaning                                 |
| ------- | --------------------------------------- |
| Elastic | Resources can scale up/down dynamically |
| Cloud   | Hosted on AWS cloud infrastructure      |
| Compute | CPU, RAM, Storage resources             |

---

### EC2 in One Line

> EC2 is an elastic virtual machine provided by AWS that can be scaled according to business requirements.

---

# 2️⃣ Understanding Compute

When you request an EC2 instance, AWS provides:

```text
CPU
+
RAM
+
Disk Storage
+
Network Interface
```

Which together form a:

```text
Virtual Machine (VM)
```

---

# 3️⃣ Virtualization Explained

Before cloud computing:

```text
One Physical Server
        │
        ▼
One User/Application
```

This led to resource wastage.

---

## With Virtualization

A Hypervisor divides a physical server into multiple VMs.

```text
┌──────────────────────────┐
│     Physical Server      │
└──────────┬───────────────┘
           │
     Hypervisor
           │
 ┌─────────┼─────────┐
 ▼         ▼         ▼
VM-1     VM-2      VM-3
```

Each VM behaves like an independent server.

---

## How AWS Uses Virtualization

```text
AWS Data Center
       │
Physical Servers
       │
 Hypervisor
       │
 ┌─────┼─────┐
 ▼     ▼     ▼
EC2   EC2   EC2
```

Whenever users request an EC2 instance, AWS allocates a VM from its massive pool of physical servers.

---

# 4️⃣ Why Use EC2?

Instead of managing physical servers yourself:

```text
Buy Servers
Install Hypervisor
Create VMs
Monitor Hardware
Apply Security Patches
Handle Failures
Upgrade Components
```

AWS does all of this for you.

---

## Benefits of EC2

### 1. Reduced Maintenance

AWS handles:

* Hardware failures
* Physical security
* Infrastructure upgrades
* Hypervisor management

---

### 2. Cost Savings

Traditional Infrastructure:

```text
Purchase Server
      +
Power Cost
      +
Cooling Cost
      +
Maintenance Cost
```

AWS Model:

```text
Pay As You Go
```

Pay only for resources you use.

---

### 3. Elasticity

Scale resources whenever required.

```text
Small Traffic
     ↓
Small Instance

Large Traffic
     ↓
Large Instance
```

---

# 5️⃣ EC2 Instance Types

AWS provides different instance families for different workloads.

---

## EC2 Instance Classification

```text
                 EC2
                  │
    ┌─────────────┼─────────────┐
    ▼             ▼             ▼
 General      Compute       Memory
 Purpose      Optimized     Optimized
                  │
                  ▼
           Storage Optimized
                  │
                  ▼
          Accelerated Compute
```

---

## 1. General Purpose

Balanced CPU, RAM, and networking.

### Use Cases

* Web applications
* Small databases
* Learning AWS
* Jenkins Servers

Examples:

```text
T2
T3
T4g
```

---

## 2. Compute Optimized

More CPU power.

### Use Cases

* Gaming servers
* Machine learning
* Scientific calculations

Examples:

```text
C5
C6
```

---

## 3. Memory Optimized

More RAM.

### Use Cases

* Big Data Analytics
* SAP HANA
* In-memory databases

Examples:

```text
R5
R6
X1
```

---

## 4. Storage Optimized

High-speed storage access.

### Use Cases

* Data warehousing
* NoSQL databases
* Large file systems

Examples:

```text
I3
D2
```

---

## 5. Accelerated Computing

GPU/FPGA-based processing.

### Use Cases

* AI/ML
* Deep Learning
* Video Rendering

Examples:

```text
P4
G5
F1
```

---

# 6️⃣ AWS Regions and Availability Zones

---

## What is a Region?

A Region is a geographical location containing AWS data centers.

Examples:

| Region      |
| ----------- |
| Mumbai      |
| Singapore   |
| Frankfurt   |
| N. Virginia |
| Ohio        |
| Sydney      |

---

### Region Diagram

```text
AWS Global Infrastructure

     ┌─────────────┐
     │   Mumbai    │
     └─────────────┘

     ┌─────────────┐
     │ Singapore   │
     └─────────────┘

     ┌─────────────┐
     │ Frankfurt   │
     └─────────────┘

     ┌─────────────┐
     │ N. Virginia │
     └─────────────┘
```

---

## Why Regions Matter

### 1. Compliance

European banks may require data to stay within Europe.

---

### 2. Latency

Users experience faster responses when servers are geographically closer.

```text
User → Nearby Server
       ↓
 Low Latency
```

---

# Availability Zones (AZs)

Each AWS Region contains multiple isolated data centers.

Example:

```text
Mumbai Region

 ┌────────────┐
 │ Mumbai-1A  │
 └────────────┘

 ┌────────────┐
 │ Mumbai-1B  │
 └────────────┘

 ┌────────────┐
 │ Mumbai-1C  │
 └────────────┘
```

---

## Why Availability Zones?

Suppose:

```text
AZ-A crashes
```

Then:

```text
AZ-B continues running
```

Result:

```text
High Availability
```

---

## High Availability Architecture

```text
                Users
                  │
                  ▼
           Load Balancer
                  │
      ┌───────────┴───────────┐
      ▼                       ▼

   EC2 (AZ-A)           EC2 (AZ-B)

      │                       │
      └───────────┬───────────┘
                  ▼
              Database
```

---

# 7️⃣ Creating an EC2 Instance

---

## Step 1

Navigate to:

```text
AWS Console
    ↓
EC2
    ↓
Instances
    ↓
Launch Instance
```

---

## Step 2: Name the Instance

Example:

```text
my-first-instance
```

---

## Step 3: Choose OS

Popular choices:

* Ubuntu
* Amazon Linux
* RedHat
* Debian

Recommended:

```text
Ubuntu LTS
```

---

## Step 4: Select Instance Type

For Free Tier:

```text
t2.micro
```

Specifications:

| Resource | Value  |
| -------- | ------ |
| CPU      | 1 vCPU |
| RAM      | 1 GB   |

---

## Step 5: Create Key Pair

Key Pair = Authentication mechanism.

```text
Public Key
      +
Private Key (.pem)
```

---

### Authentication Flow

```text
Your Laptop
     │
Private Key (.pem)
     │
     ▼
EC2 Instance
     │
Public Key
```

---

### Important

⚠️ Never share your private key.

---

## Step 6: Storage

Default:

```text
8 GB
```

Usually enough for learning and Jenkins installation.

---

## Step 7: Launch Instance

```text
Launch Instance
      ↓
Instance Running
```

---

# 8️⃣ Connecting to EC2

---

## Obtain Public IP

Example:

```text
3.110.xxx.xxx
```

---

## SSH Command

```bash
ssh -i AWS-login.pem ubuntu@<PUBLIC-IP>
```

Example:

```bash
ssh -i AWS-login.pem ubuntu@3.110.xxx.xxx
```

---

## Permission Error Fix

If SSH fails:

```bash
chmod 600 AWS-login.pem
```

Then reconnect.

---

## Verify Login

```bash
whoami
```

Output:

```text
ubuntu
```

---

# 9️⃣ Updating Packages

Always update packages first.

```bash
sudo apt update
```

---

# 🔟 Installing Jenkins

---

## Step 1: Install Java

```bash
sudo apt install openjdk-11-jdk
```

Verify:

```bash
java --version
```

---

## Step 2: Install Jenkins

Follow official Jenkins repository installation steps.

---

## Step 3: Verify Jenkins

```bash
systemctl status jenkins
```

Expected:

```text
active (running)
```

---

# 1️⃣1️⃣ Jenkins Architecture on EC2

```text
             Internet
                  │
                  ▼

        Public IP of EC2
                  │
                  ▼

       Ubuntu EC2 Instance
                  │
                  ▼

             Jenkins
             Port 8080
```

---

# 1️⃣2️⃣ Why Jenkins is Not Accessible Initially

Even though Jenkins is running:

```text
http://PUBLIC-IP:8080
```

may fail.

Reason:

```text
AWS Security Group blocks traffic
```

---

# Security Groups

Security Group acts as a virtual firewall.

```text
Internet
    │
    ▼

Security Group
    │
    ▼

EC2 Instance
```

---

## Inbound Rule Required

Add:

| Type       | Port | Source               |
| ---------- | ---- | -------------------- |
| Custom TCP | 8080 | Anywhere (0.0.0.0/0) |

---

### Security Group Flow

```text
Browser
   │
   ▼

Port 8080 Request
   │
   ▼

Security Group
   │
Allowed?
   │
   ▼
 Jenkins
```

---

# 1️⃣3️⃣ Access Jenkins

Open:

```text
http://<PUBLIC-IP>:8080
```

Example:

```text
http://3.110.xxx.xxx:8080
```

---

## Jenkins Unlock Screen

Retrieve initial password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Paste it into Jenkins UI.

---

## Success 🎉

You have:

✅ Created an EC2 Instance

✅ Connected using SSH

✅ Installed Java

✅ Installed Jenkins

✅ Opened Security Group Rules

✅ Accessed Jenkins from the Internet

---

# Important Interview Questions

### Q1. What is EC2?

EC2 is AWS's virtual machine service that provides scalable compute resources in the cloud.

---

### Q2. Why is it called Elastic Cloud Compute?

* Elastic → Scalable
* Cloud → AWS Infrastructure
* Compute → CPU/RAM/Storage

---

### Q3. What are Availability Zones?

Physically separate data centers inside a region designed for high availability.

---

### Q4. Difference Between Public IP and Private IP?

| Public IP                | Private IP                         |
| ------------------------ | ---------------------------------- |
| Accessible from internet | Accessible only inside AWS network |
| Used for SSH             | Internal communication             |

---

### Q5. What is a Security Group?

A virtual firewall controlling inbound and outbound traffic to AWS resources.

---

# Final Architecture Diagram

```text
                        Internet
                            │
                            ▼
                    Public IP Address
                            │
                            ▼
                  ┌─────────────────┐
                  │ Security Group  │
                  │ Port 22 (SSH)   │
                  │ Port 8080       │
                  └────────┬────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ Ubuntu EC2 VM   │
                  │                 │
                  │  Jenkins        │
                  │  Java           │
                  └─────────────────┘
                           │
                           ▼
                     AWS Region
                           │
                           ▼
                  Availability Zone
```

---

# 🎯 Key Takeaways

* EC2 provides virtual machines in AWS.
* Elastic means scalable resources.
* AWS manages infrastructure, reducing maintenance overhead.
* Different EC2 types serve different workloads.
* Regions help reduce latency and satisfy compliance requirements.
* Availability Zones provide fault tolerance and high availability.
* Security Groups control network access.
* Jenkins can be deployed on EC2 and exposed to the internet via port **8080**.
* SSH access requires a **private key (.pem)** and proper permissions.
* EC2 + Jenkins is one of the most common DevOps learning projects and serves as a foundation for CI/CD pipelines.