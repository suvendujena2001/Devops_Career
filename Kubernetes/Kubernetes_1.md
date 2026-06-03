# Day 31: Kubernetes Architecture (K8s Architecture) 🚀

> **Goal:** Understand Kubernetes Architecture by comparing it with Docker and learning the purpose of every Kubernetes component in a practical way.

---

# Table of Contents

1. Introduction
2. Why Kubernetes is called K8s?
3. Docker vs Kubernetes
4. Why Kubernetes Needs an Architecture?
5. Kubernetes High-Level Architecture
6. Worker Node (Data Plane) Components
7. Control Plane (Master Node) Components
8. Pod Creation Workflow
9. Kubernetes Architecture Diagram
10. Control Plane vs Data Plane
11. Cloud Controller Manager Explained
12. Interview Questions
13. Key Takeaways

---

# 1. Introduction

Kubernetes is a **Container Orchestration Platform** designed to manage containers at scale.

While Docker can run containers on a single machine, Kubernetes provides:

* Cluster Management
* Auto Healing
* Auto Scaling
* Enterprise-Level Features

---

# 2. Why Kubernetes is Called K8s?

```
Kubernetes
K -------- 8 Letters -------- s
```

There are **8 letters** between **K** and **S**.

Hence:

```text
Kubernetes = K8s
```

---

# 3. Docker vs Kubernetes

| Feature             | Docker  | Kubernetes |
| ------------------- | ------- | ---------- |
| Container Runtime   | ✅       | ✅          |
| Single Host         | ✅       | ❌          |
| Cluster Support     | Limited | ✅          |
| Auto Healing        | ❌       | ✅          |
| Auto Scaling        | ❌       | ✅          |
| Advanced Networking | Basic   | ✅          |
| Load Balancing      | Limited | ✅          |
| Enterprise Features | Limited | ✅          |

---

# 4. Why Kubernetes Needs an Architecture?

Docker can run containers using:

```bash
docker run nginx
```

However, at enterprise scale we need:

* Multiple Nodes
* Automatic Recovery
* Load Balancing
* Scheduling
* Scaling
* Networking
* Security

These capabilities require several specialized Kubernetes components.

---

# 5. Kubernetes High-Level Architecture

```text
                    USER
                      |
                      v
               +-------------+
               | API Server  |
               +-------------+
                      |
        --------------------------------
        |              |              |
        v              v              v
  Scheduler        etcd      Controller Manager
        |
        v
  -------------------------
  |                       |
  v                       v
Worker Node 1      Worker Node 2
(Data Plane)       (Data Plane)

```

---

# Kubernetes Architecture Overview

```text
+--------------------------------------------------+
|                CONTROL PLANE                     |
|--------------------------------------------------|
| API Server                                       |
| Scheduler                                        |
| etcd                                             |
| Controller Manager                               |
| Cloud Controller Manager                         |
+--------------------------------------------------+

                     |
                     |
                     v

+--------------------------------------------------+
|                 WORKER NODES                      |
|--------------------------------------------------|
| Kubelet                                          |
| Kube Proxy                                       |
| Container Runtime                                |
| Pods                                              |
+--------------------------------------------------+
```

---

# 6. Worker Node (Data Plane)

Worker nodes are responsible for:

✅ Running Applications

✅ Running Pods

✅ Networking

✅ Container Execution

---

## Worker Node Architecture

```text
+--------------------------------------+
|            Worker Node               |
|--------------------------------------|
| Kubelet                              |
| Kube Proxy                           |
| Container Runtime                    |
|--------------------------------------|
| Pod 1                                |
| Pod 2                                |
| Pod 3                                |
+--------------------------------------+
```

---

## 6.1 Kubelet

### Purpose

Kubelet is the **agent running on every worker node**.

It ensures:

* Pods are created
* Pods remain healthy
* Failed Pods are reported

### Responsibilities

```text
Monitor Pods
     |
     v
Check Health
     |
     v
Report Issues
     |
     v
Take Corrective Actions
```

### Example

If a Pod crashes:

```text
Pod Crashed
      |
      v
Kubelet Detects Failure
      |
      v
Control Plane Notified
      |
      v
Pod Restarted
```

This contributes to Kubernetes' **Auto-Healing** capability.

---

## 6.2 Kube Proxy

### Purpose

Handles networking and load balancing.

### Responsibilities

* Assign Networking Rules
* Service Communication
* Pod Connectivity
* Load Balancing

### Example

Suppose:

```text
Pod A
Pod B
```

Both serve the same application.

Incoming requests:

```text
Request 1 --> Pod A
Request 2 --> Pod B
Request 3 --> Pod A
Request 4 --> Pod B
```

Kube Proxy distributes traffic.

### Networking Flow

```text
Client
   |
   v
Kube Proxy
  / \
 /   \
v     v
PodA PodB
```

---

## 6.3 Container Runtime

### Purpose

Actually runs containers.

Without a runtime, containers cannot execute.

### Supported Container Runtimes

```text
Kubernetes
     |
     +---- containerd
     |
     +---- CRI-O
     |
     +---- Docker Shim (older)
```

### Important Concept

Kubernetes follows:

```text
CRI
(Container Runtime Interface)
```

Any runtime implementing CRI can work with Kubernetes.

---

# 7. Control Plane (Master Node)

Control Plane is the brain of Kubernetes.

It decides:

* What to run
* Where to run
* When to scale
* When to recover

---

## Control Plane Components

```text
+--------------------------------+
|        Control Plane           |
|--------------------------------|
| API Server                     |
| Scheduler                      |
| etcd                           |
| Controller Manager             |
| Cloud Controller Manager       |
+--------------------------------+
```

---

# 7.1 API Server

## The Heart of Kubernetes

Every request enters Kubernetes through the API Server.

### Responsibilities

* Accept Requests
* Authenticate Users
* Validate Requests
* Communicate with Cluster Components

### Flow

```text
User
  |
  v
API Server
  |
  v
Kubernetes Cluster
```

### Example

```bash
kubectl apply -f pod.yaml
```

Request Flow:

```text
kubectl
   |
   v
API Server
   |
   v
Scheduler
```

---

# 7.2 Scheduler

## Purpose

Determines where Pods should run.

### Questions Scheduler Answers

```text
Which node has resources?

Node 1?
Node 2?
Node 3?
```

### Scheduling Flow

```text
New Pod Request
        |
        v
    Scheduler
        |
        v
Select Best Node
        |
        v
Deploy Pod
```

### Example

```text
Node1 --> 90% Busy
Node2 --> 20% Busy
Node3 --> 50% Busy
```

Scheduler chooses:

```text
Node2
```

---

# 7.3 etcd

## Purpose

Cluster Database of Kubernetes.

Stores:

* Pods
* Services
* Secrets
* ConfigMaps
* Nodes
* Cluster State

### Type

```text
Distributed Key-Value Store
```

### Example

```text
Key              Value
--------------------------------
pod1             Running
node1            Ready
service1         Active
```

### Why etcd is Important?

Without etcd:

❌ No cluster state

❌ No backup

❌ No recovery

Think of etcd as:

```text
Brain Memory of Kubernetes
```

---

# 7.4 Controller Manager

## Purpose

Runs Kubernetes Controllers.

Controllers continuously compare:

```text
Desired State
vs
Actual State
```

### Example

User wants:

```yaml
replicas: 2
```

Actual:

```text
Only 1 Pod Running
```

Controller detects mismatch:

```text
Desired = 2
Actual = 1
```

Creates:

```text
1 Additional Pod
```

### Controller Loop

```text
Desired State
       |
       v
Compare
       |
       v
Actual State
       |
Mismatch?
       |
      Yes
       |
       v
Fix State
```

---

# ReplicaSet Example

Desired:

```yaml
replicas: 2
```

Current:

```text
Pod1 Running
Pod2 Crashed
```

Controller Manager ensures:

```text
New Pod Created
```

Result:

```text
Pod1 Running
Pod2 Running
```

---

# 7.5 Cloud Controller Manager (CCM)

## Purpose

Integrates Kubernetes with Cloud Providers.

### Supported Clouds

* AWS EKS
* Azure AKS
* Google GKE

---

## Why CCM Exists?

Kubernetes doesn't know cloud-specific APIs.

Example:

User requests:

```yaml
type: LoadBalancer
```

AWS Load Balancer creation is different from Azure.

Cloud Controller Manager translates requests.

---

## Flow

```text
User Request
      |
      v
Kubernetes
      |
      v
Cloud Controller Manager
      |
      v
AWS / Azure / GCP API
```

---

## Important Note

If Kubernetes runs on:

```text
On-Premises Environment
```

Cloud Controller Manager may not be required.

---

# 8. Pod Creation Workflow

This is one of the most important interview topics.

---

## Step-by-Step Flow

```text
User
 |
 | kubectl apply
 |
 v
API Server
 |
 v
Scheduler
 |
 v
Select Worker Node
 |
 v
Kubelet
 |
 v
Container Runtime
 |
 v
Pod Created
 |
 v
Kube Proxy
 |
 v
Networking Configured
```

---

# Complete Pod Deployment Flow

```text
+---------+
|  User   |
+---------+
     |
     v
+--------------+
| API Server   |
+--------------+
     |
     v
+--------------+
| Scheduler    |
+--------------+
     |
     v
+------------------------+
| Worker Node            |
|------------------------|
| Kubelet                |
| Container Runtime      |
| Kube Proxy             |
+------------------------+
     |
     v
+-----------+
|   Pod     |
+-----------+
```

---

# 9. Complete Kubernetes Architecture Diagram

```text
                          USER
                            |
                            v
                     +-------------+
                     | API Server  |
                     +-------------+
                            |
        ------------------------------------------------
        |                    |                        |
        v                    v                        v
 +------------+      +--------------+      +------------------+
 | Scheduler  |      |    etcd      |      | Controller Mgr   |
 +------------+      +--------------+      +------------------+
                                                  |
                                                  v
                                        +------------------+
                                        | Cloud Controller |
                                        |     Manager      |
                                        +------------------+

                            |
                            v

 =========================================================
                    WORKER NODES
 =========================================================

 +-------------------+      +-------------------+
 | Worker Node 1     |      | Worker Node 2     |
 |-------------------|      |-------------------|
 | Kubelet           |      | Kubelet           |
 | Kube Proxy        |      | Kube Proxy        |
 | Container Runtime |      | Container Runtime |
 | Pods              |      | Pods              |
 +-------------------+      +-------------------+
```

---

# 10. Control Plane vs Data Plane

| Control Plane         | Data Plane         |
| --------------------- | ------------------ |
| Makes Decisions       | Executes Decisions |
| Manages Cluster       | Runs Applications  |
| Schedules Pods        | Hosts Pods         |
| Stores State          | Runs Workloads     |
| Handles Scaling Logic | Runs Scaled Pods   |

---

# 11. Interview Summary

## Worker Node Components

```text
1. Kubelet
2. Kube Proxy
3. Container Runtime
```

---

## Control Plane Components

```text
1. API Server
2. Scheduler
3. etcd
4. Controller Manager
5. Cloud Controller Manager
```

---

# Frequently Asked Interview Questions

### Q1. What are the components of a Worker Node?

Answer:

```text
Kubelet
Kube Proxy
Container Runtime
```

---

### Q2. What are the components of Control Plane?

Answer:

```text
API Server
Scheduler
etcd
Controller Manager
Cloud Controller Manager
```

---

### Q3. Which component acts as the heart of Kubernetes?

Answer:

```text
API Server
```

---

### Q4. Which component stores cluster information?

Answer:

```text
etcd
```

---

### Q5. Which component schedules Pods?

Answer:

```text
Scheduler
```

---

### Q6. Which component ensures Pods remain running?

Answer:

```text
Kubelet
```

---

### Q7. Which component provides networking?

Answer:

```text
Kube Proxy
```

---

### Q8. Which component performs cloud integration?

Answer:

```text
Cloud Controller Manager
```

---

# 12. Key Takeaways

✅ Kubernetes is divided into:

```text
Control Plane
+
Data Plane
```

✅ Worker Nodes contain:

```text
Kubelet
Kube Proxy
Container Runtime
```

✅ Control Plane contains:

```text
API Server
Scheduler
etcd
Controller Manager
Cloud Controller Manager
```

✅ API Server is the heart of Kubernetes.

✅ Scheduler decides where Pods run.

✅ etcd stores cluster state.

✅ Controller Manager maintains desired state.

✅ Kubelet manages Pod lifecycle.

✅ Kube Proxy provides networking and load balancing.

✅ Container Runtime executes containers.

✅ Cloud Controller Manager integrates Kubernetes with cloud providers.

---

# One-Line Revision

> **Control Plane makes decisions, Data Plane executes them. API Server receives requests, Scheduler places Pods, etcd stores state, Controller Manager maintains desired state, while Kubelet, Kube Proxy, and Container Runtime run applications on Worker Nodes.**
