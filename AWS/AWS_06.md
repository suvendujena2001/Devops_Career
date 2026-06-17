# Day 6 – AWS Route 53 Explained (AWS Zero to Hero Series)

## DNS as a Service on AWS

> **Key Takeaway:**
> Route 53 is AWS's managed **DNS (Domain Name System) service** that helps users access applications using easy-to-remember domain names instead of IP addresses.

---

# 📚 Learning Objectives

After completing this topic, you should be able to:

* Understand what DNS is.
* Understand why DNS is required.
* Explain what AWS Route 53 does.
* Understand Domain Registration.
* Understand Hosted Zones and DNS Records.
* Understand Route 53 Health Checks.
* Visualize how Route 53 fits into AWS architecture.

---

# 1️⃣ What is Route 53?

### Definition

**AWS Route 53** is a highly available and scalable **DNS (Domain Name System) service** provided by AWS.

Just like:

| AWS Service | Provides                |
| ----------- | ----------------------- |
| EC2         | Compute as a Service    |
| EKS         | Kubernetes as a Service |
| S3          | Storage as a Service    |
| Route 53    | DNS as a Service        |

---

# 2️⃣ What is DNS?

### DNS = Domain Name System

DNS is responsible for:

> Converting a human-readable domain name into a machine-readable IP address.

---

## Example

Instead of accessing:

```text
54.231.123.15
```

We access:

```text
amazon.com
```

DNS translates:

```text
amazon.com → 54.231.123.15
```

---

# DNS Resolution Flow

```text
User Types:
amazon.com

        │
        ▼

+----------------+
|      DNS       |
| (Route 53)     |
+----------------+
        │
        ▼

54.231.123.15

        │
        ▼

Application Server
```

---

# 3️⃣ Why Do We Need DNS?

Without DNS:

```text
Application A → 15.12.45.22
Application B → 35.74.100.8
Application C → 52.12.18.90
```

Users would have to remember IP addresses.

This is impractical.

---

## Problem 1: IP Addresses Are Hard to Remember

### Difficult

```text
3.6.10.171
```

### Easy

```text
amazon.com
```

Humans remember names much better than numbers.

---

## Problem 2: IP Addresses Can Change

Suppose:

```text
Today:
3.6.10.171
```

Tomorrow:

```text
3.6.15.210
```

If users rely on IP addresses, every change would break access.

With DNS:

```text
amazon.com
      │
      ▼
(new IP)
```

Users continue using the same domain name.

---

# 4️⃣ Real-World AWS Architecture

Before Route 53

```text
User
  │
  ▼

Load Balancer
  │
  ▼

Application
```

Users would need the Load Balancer IP address.

---

## With Route 53

```text
                 +------------+
                 | Route 53   |
                 | DNS Service|
                 +------------+
                        │
                        ▼

User ---> amazon.com ---> Load Balancer ---> Application
```

Route 53 resolves:

```text
amazon.com
      │
      ▼
Load Balancer IP
```

---

# 5️⃣ How Route 53 Works with VPC

A typical AWS deployment:

```text
+------------------------------------------------+
|                    VPC                         |
|                                                |
|  +-----------------------------+               |
|  | Public Subnet               |               |
|  |                             |               |
|  |   Load Balancer             |               |
|  |   NAT Gateway               |               |
|  +-----------------------------+               |
|                                                |
|  +-----------------------------+               |
|  | Private Subnet              |               |
|  |                             |               |
|  |   Application Servers       |               |
|  |   Databases                 |               |
|  +-----------------------------+               |
|                                                |
+------------------------------------------------+
```

---

## Traffic Flow

```text
User
 │
 ▼

Domain Name
(example.com)

 │
 ▼

Route 53
(DNS Resolution)

 │
 ▼

Load Balancer

 │
 ▼

Application Servers
```

---

# 6️⃣ Components of Route 53

AWS Route 53 mainly provides:

```text
Route 53
│
├── Domain Registration
├── Hosted Zones
└── Health Checks
```

---

# 7️⃣ Domain Registration

## What is Domain Registration?

Before using DNS, you need a domain.

Examples:

```text
amazon.com
flipkart.com
google.com
```

---

## Traditional Method

You buy domains from:

```text
GoDaddy
Namecheap
Hostinger
etc.
```

---

## AWS Method

AWS Route 53 also allows you to:

```text
Purchase Domains Directly
```

Example:

```text
mycompany.com
mycompany.in
mycompany.xyz
```

---

## Domain Registration Flow

```text
Search Domain
      │
      ▼

Available?
      │
      ▼

Purchase Domain
      │
      ▼

Configure DNS
```

---

# 8️⃣ Hosted Zones

Hosted Zones are one of the most important concepts in Route 53.

---

## What is a Hosted Zone?

A Hosted Zone is a container for DNS records.

Think of it as:

```text
A Database of DNS Entries
```

---

## Example Hosted Zone

```text
example.com
│
├── www.example.com
├── api.example.com
├── dev.example.com
└── mail.example.com
```

Each record points to an IP address or AWS resource.

---

# Hosted Zone Diagram

```text
+----------------------------------+
|      Hosted Zone                 |
|                                  |
|  example.com                     |
|                                  |
|  www  → 54.12.10.1              |
|  api  → 54.12.10.2              |
|  dev  → 54.12.10.3              |
|                                  |
+----------------------------------+
```

---

# DNS Resolution Through Hosted Zone

```text
User Request

www.example.com
        │
        ▼

Route 53
        │
        ▼

Hosted Zone Lookup
        │
        ▼

DNS Record Found
        │
        ▼

IP Address Returned
        │
        ▼

Application Accessed
```

---

# 9️⃣ DNS Records

Hosted Zones contain DNS Records.

A DNS Record maps:

```text
Domain Name
        │
        ▼
IP Address
```

Example:

| Domain          | IP Address |
| --------------- | ---------- |
| app.example.com | 3.6.10.171 |
| api.example.com | 3.6.10.172 |

---

## Simplified Representation

```text
app.example.com
          │
          ▼
      3.6.10.171

api.example.com
          │
          ▼
      3.6.10.172
```

---

# 🔟 Route 53 Health Checks

Another important feature.

---

## What Are Health Checks?

Route 53 can periodically verify whether:

* Web servers are running.
* Applications are responding.
* Endpoints are healthy.

---

## Example

Two application servers:

```text
Server A
Server B
```

---

### Health Check Flow

```text
                 Route 53
                     │
        ┌────────────┴────────────┐
        ▼                         ▼

   Server A                 Server B
   Healthy                  Unhealthy
```

Route 53 detects:

```text
Server B = Down
```

and can avoid routing traffic to it.

---

## Benefits

✅ Improved Availability

✅ Better Reliability

✅ Reduced Downtime

✅ Automatic Traffic Routing Decisions

---

# Complete Route 53 Architecture

```text
                    User
                      │
                      ▼

             example.com
                      │
                      ▼

              +---------------+
              |   Route 53    |
              +---------------+
                      │
       ┌──────────────┼──────────────┐
       │                              │

       ▼                              ▼

 DNS Records                  Health Checks

       │                              │
       ▼                              ▼

 Load Balancer  <---------------- Health Status

       │
       ▼

 Application Servers

       │
       ▼

 Database
```

---

# Interview Perspective

A common DevOps interview question:

> **"Have you implemented a Public-Private Subnet architecture in AWS, and how does traffic reach the application?"**

Expected explanation:

```text
User
 │
 ▼

Route 53
 │
 ▼

Load Balancer (Public Subnet)
 │
 ▼

Application (Private Subnet)
 │
 ▼

Database (Private Subnet)
```

---

# Quick Revision Sheet

## Route 53

```text
AWS Managed DNS Service
```

---

## DNS

```text
Domain Name → IP Address
```

---

## Why DNS?

```text
✓ Easy to remember
✓ IPs can change
✓ User-friendly
```

---

## Route 53 Features

```text
1. Domain Registration
2. Hosted Zones
3. DNS Records
4. Health Checks
```

---

## Hosted Zone

```text
Container for DNS Records
```

---

## DNS Record

```text
example.com → IP Address
```

---

## Health Check

```text
Monitor application/server health
```

---

# Final Summary

AWS Route 53 is a **managed DNS service** that enables users to access applications through memorable domain names instead of IP addresses. It acts as the bridge between **domain names and application endpoints**, typically Load Balancers. Route 53 provides three core capabilities:

1. **Domain Registration** – Purchase and manage domains.
2. **Hosted Zones & DNS Records** – Map domains to AWS resources.
3. **Health Checks** – Monitor endpoint health and improve availability.

In a production AWS architecture, the typical traffic flow is:

```text
User
  │
  ▼

Route 53
  │
  ▼

Load Balancer
  │
  ▼

Application Servers
  │
  ▼

Database
```

This makes Route 53 one of the foundational services for building highly available, scalable, and production-ready applications on AWS. 🚀
