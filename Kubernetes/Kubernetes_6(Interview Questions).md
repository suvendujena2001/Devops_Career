# 🚀 Kubernetes Interview Questions (Part 1) - Complete Notes

> **Goal:** Strengthen Kubernetes fundamentals and prepare for common DevOps/Kubernetes interview questions.

---

# 📚 Table of Contents

1. Docker vs Kubernetes
2. Kubernetes Architecture
3. Docker Swarm vs Kubernetes
4. Container vs Pod
5. Namespace in Kubernetes
6. Role of kube-proxy
7. Kubernetes Services
8. NodePort vs LoadBalancer
9. Role of kubelet
10. Day-to-Day Activities of a Kubernetes Engineer

---

# 1️⃣ Docker vs Kubernetes

## Interview Question

**What is the difference between Docker and Kubernetes?**

## Answer

| Docker                | Kubernetes                       |
| --------------------- | -------------------------------- |
| Container Platform    | Container Orchestration Platform |
| Runs containers       | Manages containers at scale      |
| Single-node focused   | Multi-node cluster focused       |
| No auto-healing       | Auto-healing available           |
| No auto-scaling       | Auto-scaling available           |
| Limited orchestration | Advanced orchestration           |
| Basic deployment      | Enterprise-grade deployment      |

---

## Visual Representation

```text
Docker

+----------------+
| Host Machine   |
|                |
|  Container 1   |
|  Container 2   |
|  Container 3   |
+----------------+

If Host Fails
      ↓
Application Down
```

```text
Kubernetes

        +------------------+
        | Control Plane    |
        +------------------+
                 |
   --------------------------------
   |              |              |
+------+      +------+      +------+
|Node-1|      |Node-2|      |Node-3|
+------+      +------+      +------+
   |              |              |
 Pod A         Pod B         Pod C

If Node-1 Fails
      ↓
Pod A moved automatically
to Node-2 or Node-3
```

---

## Key Features Added by Kubernetes

✅ Auto-Healing

✅ Auto-Scaling

✅ Load Balancing

✅ Rolling Updates

✅ High Availability

✅ Service Discovery

✅ Custom Resource Definitions (CRDs)

---

# 2️⃣ Kubernetes Architecture

## Interview Question

**What are the main components of Kubernetes Architecture?**

---

# High-Level Architecture

```text
                 Kubernetes Cluster

        +---------------------------+
        |       Control Plane       |
        +---------------------------+

                   |
                   |
        +---------------------------+
        |        Worker Nodes       |
        +---------------------------+
```

---

# Control Plane Components

```text
+------------------------------------------------+
|                 Control Plane                  |
+------------------------------------------------+
| API Server                                     |
| Scheduler                                      |
| ETCD                                           |
| Controller Manager                             |
| Cloud Controller Manager                       |
+------------------------------------------------+
```

---

## API Server

* Entry point of Kubernetes
* Handles all API requests
* Communicates with users and components

```text
User
  |
kubectl
  |
API Server
```

---

## Scheduler

Responsible for:

* Selecting the best node
* Scheduling Pods

```text
Pod Created
     ↓
Scheduler
     ↓
Selects Best Node
```

---

## ETCD

### Kubernetes Database

Stores:

* Pods
* Services
* Deployments
* ConfigMaps
* Secrets

```text
ETCD
 ├── Pod Objects
 ├── Service Objects
 ├── Deployments
 └── Cluster State
```

---

## Controller Manager

Maintains desired state.

Examples:

* ReplicaSet Controller
* Deployment Controller
* Node Controller

```text
Desired Pods = 3

Current Pods = 2

Controller Manager
        ↓
Creates 1 New Pod
```

---

## Cloud Controller Manager

Provides cloud integration.

Examples:

* AWS
* Azure
* GCP

### Example

```text
Service Type = LoadBalancer

        ↓

Cloud Controller Manager

        ↓

Creates AWS ELB
```

---

# Worker Node Components

```text
+------------------------------------+
|          Worker Node               |
+------------------------------------+
| kubelet                            |
| kube-proxy                         |
| Container Runtime                  |
+------------------------------------+
```

---

## kubelet

* Pod lifecycle management
* Reports node status
* Ensures containers run correctly

---

## kube-proxy

* Handles networking
* Updates iptables/IPVS rules
* Enables service routing

---

## Container Runtime

Runs containers.

Examples:

* containerd
* CRI-O
* Docker Shim (legacy)

---

# 3️⃣ Docker Swarm vs Kubernetes

## Interview Question

**What are the differences between Docker Swarm and Kubernetes?**

---

| Feature             | Docker Swarm | Kubernetes |
| ------------------- | ------------ | ---------- |
| Installation        | Easy         | Complex    |
| Learning Curve      | Simple       | Moderate   |
| Scaling             | Limited      | Excellent  |
| Networking          | Basic        | Advanced   |
| Ecosystem           | Small        | Huge       |
| Community Support   | Low          | Very High  |
| Enterprise Adoption | Limited      | Massive    |

---

## Recommendation

### Use Docker Swarm

* Small projects
* Learning purposes
* Simple deployments

### Use Kubernetes

* Enterprise applications
* Production workloads
* Large-scale deployments

---

# 4️⃣ Docker Container vs Kubernetes Pod

## Interview Question

**What is the difference between a Container and a Pod?**

---

## Container

Smallest executable unit.

```text
Container
 ├── Application
 ├── Libraries
 └── Dependencies
```

---

## Pod

Smallest deployable unit in Kubernetes.

A Pod can contain:

* One container
* Multiple containers

```text
+----------------------+
|        Pod           |
|                      |
| Container A          |
| Container B          |
|                      |
+----------------------+
```

---

## Key Characteristics

### Shared Network

```text
Pod
├── Container A
├── Container B

Both share same IP
```

### Shared Storage

```text
Volume
   |
Pod
├── Container A
└── Container B
```

---

## Interview Answer

> A Pod is a Kubernetes abstraction that acts as a runtime specification for one or more containers that share the same network and storage resources.

---

# 5️⃣ Namespace in Kubernetes

## Interview Question

**What is a Namespace?**

---

## Definition

A Namespace provides logical isolation within a Kubernetes cluster.

---

## Why Namespaces?

Large organizations have multiple teams.

```text
Kubernetes Cluster

├── Namespace-A
│     ├── App-A
│     └── Team-A
│
├── Namespace-B
│     ├── App-B
│     └── Team-B
```

---

## Benefits

### Resource Isolation

```text
Namespace A
    |
 Deployment A

Namespace B
    |
 Deployment B
```

---

### Access Control

Using RBAC:

```text
Developer A
      ↓
Namespace A

Cannot Access

Namespace B
```

---

### Network Isolation

Different teams can have separate network policies.

---

## Interview Definition

> A Namespace is a logical partition within a Kubernetes cluster that enables multiple teams and applications to share the same cluster while maintaining isolation.

---

# 6️⃣ Role of kube-proxy

## Interview Question

**What is the role of kube-proxy?**

---

## Purpose

kube-proxy handles Kubernetes networking.

---

## Responsibilities

* Maintains network rules
* Updates iptables/IPVS
* Routes traffic to Pods

---

## Traffic Flow

```text
User
  |
NodeIP:NodePort
  |
kube-proxy
  |
iptables Rules
  |
Pod
```

---

## Example

When a NodePort Service is created:

```text
NodePort Service
      ↓
kube-proxy
      ↓
Updates iptables
      ↓
Traffic reaches Pod
```

---

# 7️⃣ Kubernetes Services

## Interview Question

**What are the different types of Services?**

---

## Main Service Types

```text
Services

├── ClusterIP
├── NodePort
└── LoadBalancer
```

---

# ClusterIP

Default service type.

Accessible only inside cluster.

```text
Pod A
   |
ClusterIP
   |
Pod B
```

---

# NodePort

Accessible via node IP.

```text
NodeIP:30080
      |
      ↓
     Pod
```

---

# LoadBalancer

Accessible from outside the cluster.

```text
Internet
    |
Load Balancer
    |
Kubernetes Service
    |
Pods
```

---

# Service Comparison Chart

| Service Type | Internal Access | External Access |
| ------------ | --------------- | --------------- |
| ClusterIP    | ✅               | ❌               |
| NodePort     | ✅               | ✅               |
| LoadBalancer | ✅               | ✅ (Public)      |

---

# Responsibilities of Services

```text
Service

├── Service Discovery
├── Load Balancing
└── Application Exposure
```

---

# 8️⃣ NodePort vs LoadBalancer

## Interview Question

**What is the difference between NodePort and LoadBalancer Service?**

---

| Feature             | NodePort    | LoadBalancer |
| ------------------- | ----------- | ------------ |
| Access Method       | NodeIP:Port | Public LB IP |
| Internet Accessible | Limited     | Yes          |
| Cloud Dependency    | No          | Yes          |
| Production Ready    | Rarely      | Yes          |

---

## NodePort

```text
User
  |
NodeIP:30080
  |
Pod
```

---

## LoadBalancer

```text
User
  |
Public IP
  |
Load Balancer
  |
Pods
```

---

## Practical Usage

### NodePort

* Testing
* Internal access

### LoadBalancer

* Production workloads
* Public applications

---

# 9️⃣ Role of kubelet

## Interview Question

**What is the role of kubelet?**

---

## Definition

kubelet is the primary agent running on every worker node.

---

## Responsibilities

### Pod Monitoring

```text
Pod Running?
     |
     Yes
```

---

### Pod Failure Detection

```text
Pod Crashes
      |
kubelet Detects
      |
API Server
      |
ReplicaSet
      |
New Pod Created
```

---

### Container Lifecycle Management

* Start Containers
* Stop Containers
* Restart Containers

---

## Interview Definition

> kubelet is responsible for maintaining and monitoring Pods on worker nodes and ensuring that containers are running according to the desired state defined in Kubernetes.

---

# 🔟 Day-to-Day Activities of a Kubernetes Engineer

## Interview Question

**What are your day-to-day activities in Kubernetes?**

---

# Typical Responsibilities

## Cluster Management

```text
Manage

├── Control Plane
├── Worker Nodes
└── Cluster Health
```

---

## Application Deployment

```text
CI/CD Pipeline
      |
Deployments
      |
Kubernetes Cluster
```

---

## Monitoring

Tools:

* Prometheus
* Grafana
* ELK Stack

Monitor:

* CPU
* Memory
* Network
* Pod Health

---

## Troubleshooting

Common Issues:

```text
Pods Not Running
Services Not Accessible
CrashLoopBackOff
Image Pull Errors
Networking Issues
```

---

## Cluster Maintenance

* Kubernetes upgrades
* Security patching
* Worker node maintenance
* Certificate renewal

---

## Security

* RBAC management
* Network Policies
* Secrets management
* Vulnerability remediation

---

## Support Activities

```text
Developers
     |
Raise Tickets
     |
DevOps Team
     |
Investigate
     |
Resolve
```

---

# 🎯 Quick Interview Revision Sheet

| Question                | One-Line Answer                                       |
| ----------------------- | ----------------------------------------------------- |
| Docker vs Kubernetes    | Docker runs containers; Kubernetes orchestrates them. |
| Kubernetes Architecture | Control Plane + Worker Nodes.                         |
| ETCD                    | Kubernetes object store/database.                     |
| Scheduler               | Assigns Pods to Nodes.                                |
| kubelet                 | Manages Pod lifecycle.                                |
| kube-proxy              | Handles networking and routing.                       |
| Namespace               | Logical isolation in a cluster.                       |
| Pod                     | Smallest deployable unit in Kubernetes.               |
| ClusterIP               | Internal service access.                              |
| NodePort                | Access via Node IP and Port.                          |
| LoadBalancer            | Publicly accessible service.                          |
| Docker Swarm            | Simple orchestration for small workloads.             |
| Kubernetes              | Enterprise-grade orchestration platform.              |

---

# 🏆 Interview Success Tips

### Always Structure Answers

```text
Definition
     ↓
Purpose
     ↓
How It Works
     ↓
Real-World Example
```

### Example

Instead of saying:

❌ "kubelet manages Pods."

Say:

✅ "kubelet is an agent running on every worker node that continuously monitors and manages Pods, ensuring they remain in the desired state and reporting status back to the API server."

---

# Final Takeaway

The most frequently asked Kubernetes interview topics from this session are:

1. Kubernetes Architecture
2. Docker vs Kubernetes
3. Pod vs Container
4. Namespace
5. kubelet
6. kube-proxy
7. Services (ClusterIP, NodePort, LoadBalancer)
8. NodePort vs LoadBalancer
9. Docker Swarm vs Kubernetes
10. Day-to-Day Kubernetes Responsibilities