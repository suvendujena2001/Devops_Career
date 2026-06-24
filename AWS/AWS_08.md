# 📘 Day 9 – AWS S3 Deep Dive

## AWS Zero to Hero Notes 

---

# 🌟 What is AWS S3?

**Amazon S3 (Simple Storage Service)** is AWS's highly scalable, durable, secure, and cost-effective object storage service.

It is designed to store and retrieve any amount of data from anywhere in the world.

> Think of S3 as an infinitely scalable cloud hard drive where organizations store logs, backups, images, videos, databases, reports, and application data.

---

# 🎯 Why Was S3 Created?

Organizations generate enormous amounts of data:

* Database backups
* Application logs
* Images & Videos
* Reports & Dashboards
* CSV / Excel files
* Configuration files

Traditional storage solutions become difficult to manage at scale.

AWS introduced **S3** to solve this problem.

---

# 🏗️ High-Level Architecture

```text
                 User
                   |
                   |
              Internet
                   |
                   ▼
         +------------------+
         |   AWS S3 Bucket  |
         +------------------+
                   |
         ---------------------
         |         |         |
         ▼         ▼         ▼
      Object1   Object2   Object3
      (Logs)   (Backups) (Images)
```

---

# 🔑 What Does S3 Stand For?

```text
S3 = Simple Storage Service
```

---

# ⭐ Why is S3 So Popular?

AWS S3 became one of AWS's most successful services because of:

| Feature         | Description                  |
| --------------- | ---------------------------- |
| Scalability     | Virtually unlimited storage  |
| Durability      | Extremely reliable           |
| Availability    | Highly available             |
| Security        | Encryption & Access Controls |
| Cost Efficiency | Multiple storage classes     |
| Performance     | Fast uploads/downloads       |

---

# 🎯 The Famous "11 Nines"

One of the biggest reasons behind S3's success:

```text
99.999999999%
```

### Also Called

```text
11 Nines Durability
```

---

## What Does It Mean?

If you store:

```text
1 Billion Objects
```

over

```text
100 Years
```

AWS predicts that only:

```text
1 Object
```

may be lost.

---

### Durability Visualization

```text
Stored Object
      |
      ▼

+-------------+
| Data Center |
+-------------+
      |
      ▼

AWS automatically replicates
your object across multiple
Availability Zones.

      ▼
+-------------+
| AZ-1 Copy   |
+-------------+

+-------------+
| AZ-2 Copy   |
+-------------+

+-------------+
| AZ-3 Copy   |
+-------------+
```

Even if an Availability Zone fails, data remains safe.

---

# 🌎 Is S3 Global?

## Important Interview Question

### Answer

**S3 Buckets are Region-specific**
but

**Objects are Globally Accessible**

---

## Example

Bucket:

```text
app1-payments-prod-example-com
```

Created in:

```text
US-East-1
```

Still accessible worldwide via HTTP/HTTPS.

---

### Architecture

```text
Bucket Location:
US-East-1

       +----------------+
       |   S3 Bucket    |
       +----------------+
                |
      -------------------
      |        |        |
      ▼        ▼        ▼

 India     USA     Australia

(All can access via HTTPS)
```

---

# 📦 Core Terminologies

## Bucket

Container that stores data.

```text
S3 Service
    |
    ▼
 Bucket
    |
    ▼
 Objects
```

---

## Object

Anything stored inside a bucket.

Examples:

* index.html
* image.jpg
* database.sql
* backup.zip
* log.txt

Everything is treated as an **Object**.

---

# 📋 S3 Bucket Naming Rules

Bucket names must be:

✅ Globally Unique

❌ Not unique only within your account

---

### Good Naming Convention

```text
app1-payments-prod-example-com
```

Structure:

```text
Application
     |
Feature
     |
Environment
     |
Organization
```

---

# 🚀 Creating an S3 Bucket

### Step 1

Search:

```text
S3
```

---

### Step 2

Click:

```text
Create Bucket
```

---

### Step 3

Provide:

```text
Bucket Name
AWS Region
```

---

### Step 4

Keep:

```text
Block Public Access = Enabled
```

(Recommended)

---

### Step 5

Click:

```text
Create Bucket
```

---

# 📂 Uploading Objects

```text
Bucket
  |
Upload
  |
Choose File
  |
Upload
```

Example:

```text
index.html
```

becomes

```text
Object
```

inside the bucket.

---

# 📏 S3 Scalability

AWS S3 is virtually unlimited.

### Object Size Limit

```text
5 TB per object
```

---

### Bucket Size Limit

```text
No practical limit
```

You can store:

```text
100 TB
500 TB
Petabytes
```

and beyond.

---

# 🔒 S3 Security

Security is one of the strongest features of S3.

---

## Security Components

```text
S3 Security
     |
     ├── Encryption
     ├── Bucket Policies
     ├── IAM Policies
     ├── Access Logging
     ├── Object Locking
     └── Event Monitoring
```

---

# 🔐 Encryption

## At Rest

Data stored on disk is encrypted.

```text
Stored Data
      |
Encryption
      |
Encrypted Data
```

---

## In Transit

Data transferred over:

```text
HTTPS
```

is encrypted.

---

# 👥 Access Control Methods

AWS provides:

### IAM Policies

```text
User Permissions
```

### Bucket Policies

```text
Bucket-Level Permissions
```

### ACLs

```text
Legacy Access Control
```

---

# 🛡️ Bucket Policy Demo Concept

## Scenario

User has:

```text
AmazonS3FullAccess
```

but

Bucket Owner wants to block access.

---

### Solution

Apply Bucket Policy:

```json
{
  "Effect": "Deny",
  "Principal": "*",
  "Action": "s3:*",
  "Resource": "bucket-resource"
}
```

---

### Permission Flow

```text
IAM User
     |
     ▼
Has Full S3 Access
     |
     ▼
Bucket Policy = DENY
     |
     ▼
Access Blocked
```

> Explicit DENY always wins over ALLOW.

---

# 🔄 S3 Versioning

Versioning allows keeping multiple copies of the same object.

---

## Without Versioning

```text
Upload V1
      |
Upload V2
      |
V1 Replaced
```

---

## With Versioning

```text
Upload V1
      |
Upload V2
      |
Upload V3

Stored As:

Version 1
Version 2
Version 3
```

All versions remain available.

---

## Benefits

✅ Rollback capability

✅ Recovery from accidental deletion

✅ Historical tracking

---

# 🏷️ Tags

Tags help organize AWS resources.

Example:

```text
Project = App1
Environment = Prod
Owner = DevOps
```

---

# 📜 Access Logging

Tracks:

* Who accessed bucket
* When accessed
* What action performed

---

### Architecture

```text
User
 |
 ▼
S3 Bucket
 |
 ▼
Access Logs
 |
 ▼
CloudWatch / Monitoring
```

---

# 🔔 Event Notifications

Can trigger actions when:

* Object uploaded
* Object deleted
* Object modified

---

### Example

```text
Upload File
      |
      ▼
Event Trigger
      |
      ▼
AWS Lambda
      |
      ▼
Notification
```

---

# 🔒 Object Lock

Prevents modification/deletion.

Useful for:

* Compliance
* Auditing
* Legal retention

---

# 💰 S3 Storage Classes

Different storage classes provide different:

* Cost
* Performance
* Retrieval Speed

---

## Storage Class Overview

```text
S3 Storage
     |
     ├── Standard
     ├── Standard-IA
     ├── One Zone-IA
     ├── Intelligent Tiering
     ├── Glacier Instant Retrieval
     ├── Glacier Flexible Retrieval
     └── Glacier Deep Archive
```

---

# 📊 Storage Class Comparison

| Storage Class              | Cost      | Access Speed  | Use Case                |
| -------------------------- | --------- | ------------- | ----------------------- |
| Standard                   | High      | Milliseconds  | Frequently accessed     |
| Standard-IA                | Medium    | Milliseconds  | Infrequent access       |
| One Zone-IA                | Lower     | Milliseconds  | Non-critical backups    |
| Intelligent Tiering        | Optimized | Automatic     | Unknown access patterns |
| Glacier Instant Retrieval  | Low       | Seconds       | Archives                |
| Glacier Flexible Retrieval | Very Low  | Minutes/Hours | Backup archives         |
| Glacier Deep Archive       | Cheapest  | 12–48 Hours   | Long-term archival      |

---

# 🎯 Choosing Storage Classes

```text
Need Fast Access?
        |
      YES
        |
   S3 Standard

        |
       NO
        |
 Need Cheap Storage?
        |
      YES
        |
 Glacier / Deep Archive
```

---

# ⚡ Performance Features

S3 provides:

### Multipart Uploads

Large files are divided into chunks.

---

### Traditional Upload

```text
4 TB File
    |
Single Upload
    |
Failure = Restart
```

---

### Multipart Upload

```text
4 TB File
    |
--------------------
|    |    |    |
100MB Chunks
|    |    |    |
--------------------
    |
Upload Parallel
    |
Retry Only Failed Part
```

Benefits:

* Faster uploads
* Better reliability
* Parallel processing

---

# 🌐 Demo Project 1

# Restrict Access Using Bucket Policies

---

## Objective

Even if a user has:

```text
AmazonS3FullAccess
```

Block them from accessing a specific bucket.

---

### Workflow

```text
IAM User
    |
Has S3 Full Access
    |
    ▼
Bucket Policy
(DENY)
    |
    ▼
Access Denied
```

---

## Learning Outcome

Bucket policies can override IAM permissions.

---

# 🌐 Demo Project 2

# Host a Static Website Using S3

One of the most popular S3 use cases.

---

## Architecture

```text
index.html
     |
     ▼
S3 Bucket
     |
Static Website Hosting
     |
Public Access
     |
     ▼
Website URL
```

---

# Steps

### 1. Create Bucket

```text
my-static-site
```

---

### 2. Upload

```text
index.html
```

---

### 3. Enable

```text
Static Website Hosting
```

---

### 4. Disable

```text
Block Public Access
```

---

### 5. Add Bucket Policy

```json
{
  "Effect":"Allow",
  "Principal":"*",
  "Action":"s3:GetObject"
}
```

---

### 6. Access Website

```text
http://bucket-endpoint-url
```

---

# ⚠️ Common Static Website Issue

## Access Denied

Reasons:

```text
Public Access Blocked
OR
Bucket Policy Missing
```

---

# 🎓 Important Interview Questions

## Q1. What is S3?

Object storage service provided by AWS.

---

## Q2. Why is S3 highly durable?

Because AWS replicates data across multiple Availability Zones.

---

## Q3. What is the durability of S3?

```text
99.999999999%
```

(11 Nines)

---

## Q4. Difference Between IAM Policy and Bucket Policy?

| IAM Policy              | Bucket Policy          |
| ----------------------- | ---------------------- |
| Attached to users/roles | Attached to bucket     |
| Identity-based          | Resource-based         |
| Controls user access    | Controls bucket access |

---

## Q5. What is Versioning?

Maintains multiple versions of an object.

---

## Q6. Maximum Size of an Object?

```text
5 TB
```

---

## Q7. What is Multipart Upload?

Uploading large files in smaller chunks for reliability and speed.

---

## Q8. Why Use Glacier?

For long-term archival at extremely low cost.

---

# 📝 Key Takeaways

✅ S3 = Simple Storage Service

✅ Object-based storage

✅ Region-specific buckets, globally accessible objects

✅ 11 Nines durability

✅ Virtually unlimited scalability

✅ Strong security through IAM, Bucket Policies, Encryption

✅ Supports Versioning

✅ Multiple Storage Classes for cost optimization

✅ Can host Static Websites

✅ Multipart Upload improves performance

✅ One of the most important and widely used AWS services

---

# 🚀 AWS S3 Mental Model

```text
                    AWS S3
                       |
      ------------------------------------------------
      |             |            |         |         |
      ▼             ▼            ▼         ▼         ▼

  Storage      Security    Scalability  Cost     Hosting
                                   |
                                   ▼
                          Static Websites

                       +
                 Versioning
                       +
                  Durability
               (11 Nines)
```

> **Interview Summary:** AWS S3 is a highly durable, scalable, secure, and cost-effective object storage service that stores data as objects inside buckets and supports advanced capabilities such as versioning, lifecycle management, encryption, static website hosting, and multi-tier storage classes.
