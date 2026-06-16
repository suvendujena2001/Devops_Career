# Day 5 – AWS Security Groups & NACL (Network ACL)

## AWS Zero to Hero – Detailed Notes

> **Topic Focus:** Understanding AWS VPC Security using **Security Groups** and **Network ACLs (NACLs)** with both theory and practical implementation.

---

# 📚 Recap of Day 4 – AWS VPC Fundamentals

Before learning Security Groups and NACLs, it's important to understand the AWS networking foundation.

## What is a VPC?

**VPC (Virtual Private Cloud)** is a logically isolated network inside AWS where you deploy your applications and infrastructure.

### Why VPC is Important?

AWS is a **public cloud**, but organizations need **private and secure environments**.

VPC introduces:

* Network isolation
* Private networking
* Controlled access
* Enhanced security
* Traffic management

---

# 🏘️ Real-World Analogy: Secure Gated Community

```text
+------------------------------------------------+
|                GATED COMMUNITY                 |
|                                                |
|  Security Gate --> Visitors --> Buildings      |
|                                                |
+------------------------------------------------+
```

### Mapping to AWS

| Real World      | AWS Component    |
| --------------- | ---------------- |
| Gated Community | VPC              |
| Security Gate   | Internet Gateway |
| Roads           | Route Tables     |
| Building        | EC2 Instance     |
| Security Guard  | Security Group   |
| Area Security   | NACL             |
| Reception Desk  | Load Balancer    |

---

# 🌐 Traffic Flow Inside a VPC

A request reaching an application typically follows:

```text
User
 |
 v
Internet Gateway
 |
 v
Public Subnet
 |
 v
Load Balancer
 |
 v
Private Subnet
 |
 v
EC2 Instance
 |
 v
Application
```

---

# 🔐 AWS Security Model

AWS follows a:

## Shared Responsibility Model

```text
AWS Responsibility
-------------------
✔ Physical Security
✔ Data Centers
✔ Network Infrastructure
✔ Hardware

Customer Responsibility
------------------------
✔ Security Groups
✔ NACLs
✔ IAM Permissions
✔ Application Security
✔ Data Security
✔ Network Configuration
```

AWS provides security tools.

**You must configure them correctly.**

---

# Understanding Subnets

When creating a VPC, you define an IP range.

Example:

```text
CIDR Block:
10.1.0.0/16
```

This provides:

```text
65,536 IP Addresses
```

---

## Subnet Division

```text
VPC (10.1.0.0/16)
|
+---- Public Subnet
|
+---- Private Subnet
|
+---- Private Subnet
```

### Public Subnet

* Accessible from Internet
* Usually hosts:

  * Load Balancers
  * Bastion Hosts

### Private Subnet

* Not directly accessible from Internet
* Hosts:

  * Applications
  * Databases
  * Internal Services

---

# Security Layers Inside VPC

AWS allows multiple security checkpoints.

```text
Internet
   |
   v
Internet Gateway
   |
   v
NACL (Subnet Security)
   |
   v
Security Group (Instance Security)
   |
   v
Application
```

---

# Security Group

## Definition

A Security Group acts as a **virtual firewall** attached to an EC2 instance.

### Scope

```text
Applied At:
EC2 Instance Level
```

```text
EC2 Instance
    |
    +--> Security Group
```

---

# Why Security Groups?

Suppose you deploy:

```text
Jenkins
```

on:

```text
Port 8080
```

By default AWS blocks incoming traffic.

To access Jenkins:

```text
Security Group
   |
   +--> Allow Port 8080
```

---

# Security Group Traffic Types

## 1. Inbound Traffic

Traffic entering your instance.

```text
User
 |
 v
Application
```

Examples:

* HTTP (80)
* HTTPS (443)
* SSH (22)
* Jenkins (8080)

---

## 2. Outbound Traffic

Traffic leaving your instance.

```text
Application
 |
 v
Internet
```

Examples:

* Calling APIs
* Downloading Packages
* Accessing External Services

---

# Security Group Default Behavior

## Inbound Rules

```text
Default:
DENY ALL
```

No incoming traffic is allowed unless explicitly configured.

---

## Outbound Rules

```text
Default:
ALLOW ALL
```

All outbound traffic is permitted.

---

# Default Security Group Behavior Chart

```text
+----------------+------------+
| Traffic Type   | Default    |
+----------------+------------+
| Inbound        | DENY ALL   |
| Outbound       | ALLOW ALL  |
+----------------+------------+
```

---

# Port 25 Restriction

AWS blocks outbound traffic on:

```text
Port 25 (SMTP)
```

### Why?

To prevent:

* Spam Emails
* Abuse
* Reputation Damage

This helps AWS maintain trusted IP ranges.

---

# Network ACL (NACL)

## Full Form

```text
Network Access Control List
```

---

# What is NACL?

A NACL is a firewall operating at the **Subnet Level**.

### Scope

```text
Applied At:
Subnet Level
```

```text
Subnet
 |
 +--> NACL
      |
      +--> EC2-1
      +--> EC2-2
      +--> EC2-3
```

---

# Why NACL is Needed?

Imagine:

Developer creates:

```text
Security Group:
Allow ALL Traffic
```

This is risky.

As a DevOps Engineer, you need an additional control layer.

NACL provides:

```text
Subnet-wide Security
```

---

# Security Group vs NACL

## Quick Comparison

| Feature        | Security Group   | NACL             |
| -------------- | ---------------- | ---------------- |
| Level          | Instance         | Subnet           |
| Firewall Type  | Virtual Firewall | Network Firewall |
| Allows Rules   | Yes              | Yes              |
| Deny Rules     | No               | Yes              |
| Scope          | Single EC2       | Entire Subnet    |
| Stateful       | Yes              | No               |
| Security Layer | Last Layer       | First Layer      |

---

# Security Architecture

```text
Internet
   |
   v
+-------------------+
|      NACL         |
| Subnet Firewall   |
+-------------------+
   |
   v
+-------------------+
| Security Group    |
| Instance Firewall |
+-------------------+
   |
   v
Application
```

---

# Key Difference

## Security Group

Can only:

```text
ALLOW
```

Cannot:

```text
DENY
```

---

## NACL

Can:

```text
ALLOW
DENY
```

Both are supported.

---

# Practical Demonstration Overview

The practical setup created:

```text
Custom VPC
    |
    +--> Public Subnet
    |
    +--> EC2 Instance
    |
    +--> Security Group
    |
    +--> NACL
```

---

# Step 1 – Create VPC

AWS Console

```text
VPC
  |
  +--> Create VPC
```

Select:

```text
VPC and More
```

AWS automatically creates:

* Public Subnets
* Private Subnets
* Route Tables
* Internet Gateway
* NACL

---

# CIDR Block Example

```text
10.0.0.0/16
```

Provides:

```text
65,536 IP Addresses
```

---

# VPC Components Created Automatically

```text
Demo VPC
 |
 +--> Public Subnet
 |
 +--> Private Subnet
 |
 +--> Route Table
 |
 +--> Internet Gateway
 |
 +--> NACL
```

---

# Step 2 – Launch EC2 Instance

Configuration:

```text
Name:
Demo Instance

AMI:
Ubuntu

Type:
t2.micro
```

---

# Important Network Selection

Choose:

```text
Custom VPC
```

instead of:

```text
Default VPC
```

---

# Place EC2 in Public Subnet

```text
Demo VPC
   |
   +--> Public Subnet
          |
          +--> EC2 Instance
```

---

# Enable Public IP

```text
Assign Public IP = YES
```

This allows internet access.

---

# Step 3 – Run Python Application

Connect to EC2:

```bash
ssh -i key.pem ubuntu@<public-ip>
```

Update packages:

```bash
sudo apt update
```

Verify Python:

```bash
python3
```

---

# Run Simple HTTP Server

```bash
python3 -m http.server 8000
```

Application starts on:

```text
Port 8000
```

---

# First Access Attempt

Open browser:

```text
http://<public-ip>:8000
```

Result:

```text
FAILED
```

---

# Why Did It Fail?

Traffic Flow:

```text
Internet
   |
   v
NACL
   |
   v
Security Group
   |
   X BLOCKED
```

Security Group only allowed:

```text
Port 22 (SSH)
```

---

# Security Group Configuration

Default:

```text
Inbound:
22 (SSH)

Outbound:
All Traffic
```

---

# Step 4 – Allow Port 8000

Edit Security Group:

```text
Inbound Rules
    |
    +--> Custom TCP
    +--> Port 8000
    +--> Source: Anywhere
```

---

# Result

```text
Browser
    |
    +--> http://IP:8000
```

Output:

```text
Directory Listing
```

Success!

---

# Traffic Path After Allowing Security Group

```text
Internet
   |
   v
NACL (Allow)
   |
   v
Security Group (Allow 8000)
   |
   v
Application
```

---

# Step 5 – Block Port 8000 in NACL

Edit NACL:

```text
Inbound Rule
```

Create:

```text
Rule Number: 100
Port: 8000
Action: DENY
```

---

# Result

Browser Access:

```text
FAILED
```

---

# Why?

Traffic Flow:

```text
Internet
   |
   v
NACL
   |
   X DENIED
```

Traffic never reaches Security Group.

---

# Critical Learning

Even if Security Group allows:

```text
Port 8000
```

NACL can still block it.

---

# NACL Rule Evaluation Order

NACL processes rules from:

```text
Lowest Number
      to
Highest Number
```

---

## Example 1

```text
100 -> Allow All
200 -> Deny 8000
```

Result:

```text
ALLOWED
```

Because Rule 100 matched first.

---

## Example 2

```text
100 -> Deny 8000
110 -> Allow All
```

Result:

```text
DENIED
```

Because Rule 100 matched first.

---

# NACL Processing Diagram

```text
Incoming Request
        |
        v
 Rule 100 ?
        |
        +--> Match --> Apply Action
        |
        +--> No Match
                |
                v
 Rule 110 ?
                |
                +--> Match --> Apply Action
                |
                +--> No Match
                        |
                        v
                      *
```

---

# Real Organizational Use Cases

NACLs are often used for:

### Blocking Specific Ports

```text
8000
8080
9000
```

---

### Blocking Specific IPs

```text
3.4.5.6
```

---

### Blocking Entire CIDR Ranges

```text
3.4.0.0/16
```

---

### Organization-Wide Security Policies

Applied once at subnet level.

Automatically affects:

```text
1 Instance
10 Instances
100 Instances
10,000 Instances
```

---

# Security Flow Summary

```text
User
 |
 v
Internet Gateway
 |
 v
NACL
 |
 v
Route Table
 |
 v
Security Group
 |
 v
Application
```

---

# Final Key Takeaways

## Security Group

✅ Applied at EC2 level

✅ Stateful firewall

✅ Allows traffic

✅ Instance-specific

❌ Cannot deny traffic

---

## NACL

✅ Applied at subnet level

✅ Can allow traffic

✅ Can deny traffic

✅ Suitable for organization-wide controls

✅ First layer of defense

---

# Interview Questions

### Q1. Difference between Security Group and NACL?

| Security Group | NACL          |
| -------------- | ------------- |
| Instance Level | Subnet Level  |
| Stateful       | Stateless     |
| Allow Only     | Allow + Deny  |
| Last Defense   | First Defense |

---

### Q2. Can NACL block traffic allowed by Security Group?

**Yes.**

NACL is evaluated first.

---

### Q3. Can Security Group block traffic allowed by NACL?

**Yes.**

Traffic must pass both layers.

---

### Q4. Which is evaluated first?

```text
NACL
   ↓
Security Group
   ↓
Application
```

---

# Day 5 Conclusion

AWS provides multiple layers of security inside a VPC:

```text
Internet Gateway
        ↓
NACL
        ↓
Route Table
        ↓
Security Group
        ↓
Application
```

A request must successfully pass **every security layer** before reaching the application.

**Security Groups** protect individual instances, while **NACLs** enforce security policies across entire subnets. Together they form a powerful defense mechanism and are foundational concepts for every AWS, DevOps, Cloud, and Platform Engineer.
