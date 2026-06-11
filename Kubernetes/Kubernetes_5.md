# 🚀 Day 35: Kubernetes Services — Service Discovery, Load Balancing & Networking

> **"Pods are ephemeral, Services provide stability."**
>
> In Kubernetes, Pods can be created, destroyed, and recreated at any time. Their IP addresses change frequently. To ensure reliable communication, traffic distribution, and external accessibility, Kubernetes introduces a powerful abstraction called **Service**.

---

# 📖 Table of Contents

1. What is a Kubernetes Service?
2. Why Do We Need Services?
3. Problems Without Services
4. Service Benefits

   * Load Balancing
   * Service Discovery
   * External Exposure
5. Labels & Selectors
6. Types of Kubernetes Services

   * ClusterIP
   * NodePort
   * LoadBalancer
7. Complete Traffic Flow
8. Real-World Examples
9. Interview Questions & Answers
10. Key Takeaways

---

# 1️⃣ What is a Kubernetes Service?

A **Service** is a Kubernetes resource that provides a **stable network endpoint** for a group of Pods.

It acts as:

* A **Load Balancer**
* A **Service Discovery Mechanism**
* A **Network Gateway**
* A **Stable Entry Point** for applications

---

# 2️⃣ Why Do We Need Services?

Before understanding Services, let's understand the problem.

---

## Deployment Architecture

```text
Deployment
    │
    ▼
ReplicaSet
    │
 ┌──┴──┐
 ▼  ▼  ▼
Pod Pod Pod
```

A Deployment creates:

* ReplicaSet
* ReplicaSet creates Pods
* Pods run the application

---

## Example

Suppose we deploy:

```yaml
replicas: 3
```

Result:

```text
Deployment
    │
ReplicaSet
    │
 ┌──┴───────────┐
 ▼      ▼      ▼

Pod1   Pod2   Pod3

IP:    IP:    IP:
3.4    3.5    3.6
```

---

# 3️⃣ Problem Without Services

---

## Problem 1: Pod IPs Change

Pods are **ephemeral**.

When a Pod crashes:

```text
Pod1 (172.16.3.4)
        ❌
      Crashed
```

Kubernetes Auto-Healing:

```text
ReplicaSet
     │
     ▼
Creates New Pod
```

New Pod:

```text
Pod1 New

IP = 172.16.3.8
```

Notice:

```text
OLD IP = 172.16.3.4

NEW IP = 172.16.3.8
```

The application is alive, but the IP changed.

---

## User Access Problem

Previously users were accessing:

```text
User A → 172.16.3.4
User B → 172.16.3.5
User C → 172.16.3.6
```

After Pod restart:

```text
172.16.3.4 ❌

New IP:
172.16.3.8
```

Now User A gets:

```text
Application Unreachable
```

Even though the application is healthy.

---

## Real-World Question

Imagine Google doing this:

```text
User1 → 100.64.1.2
User2 → 100.64.1.3
User3 → 100.64.1.4
```

Impossible.

Users should access:

```text
google.com
```

not Pod IPs.

---

# Kubernetes Solution: Service

```text
Users
  │
  ▼
Service
  │
 ┌┴────────────┐
 ▼      ▼      ▼

Pod1   Pod2   Pod3
```

Users access:

```text
payment.default.svc
```

instead of Pod IPs.

---

# 4️⃣ Service Benefits

---

# Benefit 1: Load Balancing

Services automatically distribute traffic among Pods.

---

## Without Load Balancing

```text
100 Requests
     │
     ▼
   Pod1
```

Result:

```text
Pod1 overloaded
```

---

## With Load Balancing

```text
100 Requests
      │
      ▼
   Service
      │
 ┌────┼────┐
 ▼    ▼    ▼

P1   P2   P3
```

Distribution:

```text
33 → Pod1
33 → Pod2
34 → Pod3
```

Result:

✅ Better Performance

✅ Better Availability

✅ Better Scalability

---

## Service + kube-proxy

Internally Kubernetes uses:

```text
Service
   │
   ▼
kube-proxy
   │
   ▼
Pods
```

kube-proxy manages traffic routing.

---

# Benefit 2: Service Discovery

This is one of the most important interview topics.

---

## Problem

Service cannot keep tracking Pod IPs manually.

Imagine:

```text
1000 Pods
```

Tracking all IPs becomes impossible.

---

## Kubernetes Solution

### Labels & Selectors

Instead of tracking IPs:

```text
Track Labels
```

---

## Label Example

Every Pod gets a label.

```yaml
labels:
  app: payment
```

---

### Pod 1

```yaml
labels:
  app: payment
```

### Pod 2

```yaml
labels:
  app: payment
```

### Pod 3

```yaml
labels:
  app: payment
```

---

## Service Selector

Service watches Pods using labels.

```yaml
selector:
  app: payment
```

---

## Visual Representation

```text
                Service
        selector=app:payment
                     │
 ┌───────────────────┼───────────────────┐
 ▼                   ▼                   ▼

Pod1              Pod2               Pod3

label             label              label
app=payment       app=payment        app=payment
```

---

## Pod Restart Scenario

Before:

```text
Pod1

IP = 172.16.3.4
Label = payment
```

After restart:

```text
Pod1

IP = 172.16.3.8
Label = payment
```

Service doesn't care about:

```text
IP Changes ❌
```

Service only cares about:

```text
Label = payment ✅
```

This mechanism is called:

# 🔍 Service Discovery

---

# Benefit 3: Exposing Applications

By default, Pods are accessible only inside the cluster.

---

## Without Service

```text
Internet User
      │
      ❌
      │
 Kubernetes Cluster
      │
      ▼
     Pod
```

External users cannot access the application.

---

## With Service

```text
Internet User
      │
      ▼
   Service
      │
      ▼
     Pod
```

Now application can be exposed depending on Service type.

---

# 5️⃣ Types of Kubernetes Services

There are three commonly used service types:

```text
1. ClusterIP
2. NodePort
3. LoadBalancer
```

---

# Type 1: ClusterIP

Default service type.

```yaml
type: ClusterIP
```

---

## Access Scope

```text
Inside Kubernetes Cluster Only
```

---

### Architecture

```text
Pod
 ▲
 │
Service (ClusterIP)
 ▲
 │
Internal Kubernetes Users
```

External users:

```text
❌ Cannot Access
```

---

## Benefits

✅ Load Balancing

✅ Service Discovery

❌ No External Access

---

## Use Cases

* Internal APIs
* Database Services
* Backend Services
* Microservice Communication

---

# Type 2: NodePort

```yaml
type: NodePort
```

---

## Access Scope

Accessible through Node IP + Port.

---

### Architecture

```text
Internet/User
      │
      ▼
NodeIP:30000
      │
      ▼
Service
      │
      ▼
Pods
```

---

## Example

```text
Node IP:
192.168.1.10

Port:
30080
```

Access:

```text
192.168.1.10:30080
```

---

## Benefits

✅ Load Balancing

✅ Service Discovery

✅ Accessible inside organization/VPC

❌ Not ideal for public internet

---

## Use Cases

* Internal Testing
* Development Environments
* Internal Applications

---

# Type 3: LoadBalancer

```yaml
type: LoadBalancer
```

---

## Access Scope

Accessible from anywhere on the internet.

---

### Architecture

```text
Internet
    │
    ▼
Cloud Load Balancer
    │
    ▼
Kubernetes Service
    │
    ▼
Pods
```

---

## AWS Example

```text
Service
   │
   ▼
AWS ELB
(Elastic Load Balancer)
   │
   ▼
Public IP
```

Users access:

```text
http://Public-IP
```

or

```text
https://your-domain.com
```

---

## Benefits

✅ Load Balancing

✅ Service Discovery

✅ Public Access

---

## Use Cases

* Amazon.com
* Netflix
* Banking Portals
* Public APIs

---

# Comparison Table

| Feature           | ClusterIP | NodePort | LoadBalancer |
| ----------------- | --------- | -------- | ------------ |
| Default Type      | ✅         | ❌        | ❌            |
| Internal Access   | ✅         | ✅        | ✅            |
| External Access   | ❌         | Limited  | ✅            |
| Load Balancing    | ✅         | ✅        | ✅            |
| Service Discovery | ✅         | ✅        | ✅            |
| Public IP         | ❌         | ❌        | ✅            |
| Cloud Required    | ❌         | ❌        | Usually Yes  |

---

# 6️⃣ Complete Service Flow

```text
                     Users
                       │
                       ▼
                 Kubernetes Service
                       │
                (Load Balancing)
                       │
      ┌────────────────┼────────────────┐
      ▼                ▼                ▼

   Pod-1            Pod-2            Pod-3

Label             Label             Label
app=payment       app=payment       app=payment
```

---

# 7️⃣ How Kubernetes Service Works Internally

```text
Client
   │
   ▼
Service
   │
   ▼
kube-proxy
   │
   ▼
Matching Pods
```

Selection happens using:

```text
Labels + Selectors
```

---

# 8️⃣ Real-World Examples

---

## Example 1: Amazon Website

```text
amazon.com
```

Requirement:

```text
Accessible Worldwide
```

Service Type:

```text
LoadBalancer
```

---

## Example 2: Internal HR Portal

Requirement:

```text
Only Company Employees
```

Service Type:

```text
NodePort
```

or

```text
Internal Load Balancer
```

---

## Example 3: Backend Database

Requirement:

```text
Only Kubernetes Applications
```

Service Type:

```text
ClusterIP
```

---

# 9️⃣ Interview Questions

---

## Q1: Why do we need Kubernetes Services?

**Answer:**

Services provide:

1. Load Balancing
2. Service Discovery
3. Stable Network Endpoint
4. External Exposure

---

## Q2: What problem does Service Discovery solve?

**Answer:**

Pods are ephemeral and their IP addresses change after restart.

Services use:

```text
Labels + Selectors
```

instead of Pod IPs, ensuring uninterrupted communication.

---

## Q3: How does a Service identify Pods?

**Answer:**

Using:

```yaml
selector:
  app: payment
```

which matches Pod labels.

---

## Q4: What is the default Service type?

**Answer:**

```text
ClusterIP
```

---

## Q5: Difference Between NodePort and LoadBalancer?

| NodePort            | LoadBalancer                    |
| ------------------- | ------------------------------- |
| Exposes via Node IP | Exposes via Cloud Load Balancer |
| Limited Access      | Public Access                   |
| No Cloud Dependency | Cloud Integration Required      |

---

# 🔥 Key Takeaways

### Kubernetes Services provide:

✅ Stable Access Point

✅ Load Balancing

✅ Service Discovery

✅ Network Abstraction

✅ Application Exposure

---

### Service Discovery uses:

```text
Labels + Selectors
```

instead of

```text
Pod IP Addresses
```

---

### Service Types

```text
ClusterIP
    │
    ├─ Internal Only

NodePort
    │
    ├─ Accessible via Node IP

LoadBalancer
    │
    └─ Accessible from Internet
```

---

# 🏆 One-Line Summary

> **A Kubernetes Service is a stable networking layer that enables load balancing, service discovery, and controlled exposure of Pods, ensuring applications remain reachable even when Pods are recreated and their IP addresses change.**
