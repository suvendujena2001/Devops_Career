Below are polished, structured, and interview-friendly notes from **Day 23 – Introduction to Containers**. The content has been reorganized for learning, revision, and documentation purposes, with diagrams and comparison charts for clarity.

# 🚀 Day 23: Introduction to Containers

## 📖 Overview

Containers are one of the most important technologies in the modern DevOps ecosystem. Before learning tools like Docker, Kubernetes, Podman, or Buildah, it is crucial to understand:

* Why containers were created
* Problems with traditional infrastructure
* Difference between Virtual Machines and Containers
* How Docker works
* Introduction to Buildah

---

# 1️⃣ Evolution of Infrastructure

Infrastructure has evolved through multiple stages:

```text
Physical Servers
       │
       ▼
Virtual Machines (VMs)
       │
       ▼
Containers
```

### Key Idea

* **Virtual Machines** solved many problems of Physical Servers.
* **Containers** solved many problems of Virtual Machines.

---

# 2️⃣ Physical Servers

A physical server is actual hardware.

Examples:

* IBM Servers
* HP Servers
* Dell Servers
* Your Laptop/Desktop

## Problem with Physical Servers

Suppose you have:

```text
Physical Server
─────────────────
CPU: 100
RAM: 100 GB
```

But your application uses only:

```text
CPU: 20
RAM: 15 GB
```

### Result

❌ Huge resource wastage

```text
Used Resources
██████░░░░░░░░░░░░░░

Unused Resources
████████████████████
```

Organizations owning thousands of servers were paying for resources they never utilized.

---

# 3️⃣ Virtualization and Hypervisors

To solve resource wastage, virtualization was introduced.

## What is Virtualization?

Virtualization allows multiple virtual machines to run on a single physical server.

## What is a Hypervisor?

A Hypervisor is software that creates and manages Virtual Machines.

Examples:

* VMware ESXi
* Xen Hypervisor
* Hyper-V
* KVM

---

## Virtual Machine Architecture

```text
+--------------------------------+
|       Physical Server          |
+--------------------------------+
|          Hypervisor            |
+--------------------------------+
| VM1 | VM2 | VM3 | VM4          |
+--------------------------------+
```

Each VM contains:

```text
VM
│
├── Operating System
├── Libraries
├── Dependencies
└── Application
```

---

## Benefits of Virtual Machines

### Better Resource Utilization

Instead of:

```text
1 Server → 1 Application
```

We can do:

```text
1 Server → Multiple Applications
```

### Strong Isolation

Each VM has:

* Separate OS
* Separate Memory
* Separate Filesystem

Thus, applications remain isolated and secure.

---

# 4️⃣ Problem with Virtual Machines

Although VMs improved utilization, another issue remained.

## Example

Suppose:

```text
Physical Server
RAM = 100 GB
CPU = 100
```

You create:

```text
VM1 = 25 GB
VM2 = 25 GB
VM3 = 25 GB
VM4 = 25 GB
```

But actual usage is:

```text
VM1
Allocated RAM = 25 GB
Used RAM      = 10 GB

Unused RAM    = 15 GB
```

### Observation

Even at peak load:

```text
Allocated: 25 GB
Used:      10 GB
Waste:     15 GB
```

This happens across thousands or millions of servers.

---

## Real Cloud Example

Think about AWS EC2 instances.

Question:

> How many times have your EC2 instances actually exhausted all CPU and RAM?

Usually:

```text
Very Rarely
```

Most instances remain underutilized.

This causes:

💰 Increased infrastructure costs

---

# 5️⃣ Birth of Containers

Containers were introduced to solve VM inefficiencies.

## Goal

Increase resource utilization further.

```text
Physical Server
      ↓
Virtual Machines
      ↓
Containers
```

Each stage improves efficiency.

---

# 6️⃣ Container Architecture

Containers can run in two ways.

---

## Model 1: Directly on Physical Server

```text
+---------------------+
| Physical Server     |
+---------------------+
| Operating System    |
+---------------------+
| Docker              |
+---------------------+
| C1 C2 C3 C4 C5      |
+---------------------+
```

---

## Model 2: On Top of Virtual Machine

```text
+---------------------+
| Physical Server     |
+---------------------+
| Hypervisor          |
+---------------------+
| Virtual Machine     |
+---------------------+
| Docker              |
+---------------------+
| C1 C2 C3 C4 C5      |
+---------------------+
```

### Industry Trend

Today, most organizations prefer:

✅ Model 2

Because:

* Cloud providers manage hardware
* Reduced maintenance
* Reduced operational overhead
* Easier scalability

---

# 7️⃣ Why Containers are Lightweight

The biggest difference:

## Virtual Machine

```text
VM
│
├── Full Operating System
├── Libraries
├── Dependencies
└── Application
```

---

## Container

```text
Container
│
├── Application
├── Application Libraries
├── Required Dependencies
└── Minimal Base OS
```

Containers share resources from the host operating system.

---

## Visual Representation

### Virtual Machines

```text
Physical Server
│
├── VM1 (Full OS)
├── VM2 (Full OS)
├── VM3 (Full OS)
└── VM4 (Full OS)
```

---

### Containers

```text
Host OS
│
├── Container 1
├── Container 2
├── Container 3
└── Container 4
```

All containers share the host OS kernel.

---

# 8️⃣ What Exactly is a Container?

A Container is a package containing:

```text
Container
│
├── Application
├── Application Libraries
├── System Dependencies
└── Runtime Requirements
```

Example:

```text
Python App Container
│
├── Python Runtime
├── Required Libraries
├── Dependencies
└── Application Code
```

---

# 9️⃣ Why Containers are Faster

Since containers do not carry a full operating system:

### VM Snapshot Size

```text
1 GB
2 GB
3 GB
```

or more.

---

### Container Image Size

```text
100 MB
200 MB
500 MB
```

(Depending on application)

---

## Benefits

### Smaller Size

✅ Less storage

### Faster Transfer

✅ Easy shipping

### Faster Deployment

✅ Quick startup

### Better Scalability

✅ More containers per host

---

# 🔟 Virtual Machines vs Containers

| Feature        | Virtual Machine | Container  |
| -------------- | --------------- | ---------- |
| OS             | Full OS         | Minimal OS |
| Size           | Large           | Small      |
| Startup Time   | Slow            | Fast       |
| Resource Usage | High            | Low        |
| Isolation      | Strong          | Moderate   |
| Security       | Higher          | Lower      |
| Portability    | Moderate        | Excellent  |
| Scalability    | Moderate        | High       |

---

# 1️⃣1️⃣ Security Perspective

### Virtual Machines

```text
VM1
│
└── Full OS

VM2
│
└── Full OS
```

Strong isolation because every VM has its own operating system.

---

### Containers

```text
Container 1
Container 2
Container 3
      │
      ▼
Shared Host OS
```

Since resources are shared:

* Isolation exists
* But not as strong as VMs

Therefore:

```text
VM Security > Container Security
```

---

# 1️⃣2️⃣ Introduction to Docker

Docker is a containerization platform.

It simplified:

* Container creation
* Container management
* Container deployment

Docker made containerization easy through:

* Dockerfile
* Docker Engine
* Docker Commands

---

# Docker Workflow

```text
Dockerfile
     │
     ▼
Docker Image
     │
     ▼
Docker Container
```

---

## Docker Components

### Dockerfile

A text file containing instructions.

Example:

```text
Install Python
Copy Code
Run Application
```

---

### Docker Image

A packaged blueprint.

Contains:

```text
Application
Dependencies
Libraries
Runtime
```

---

### Docker Container

Running instance of an image.

```text
Image + Execution = Container
```

---

# Docker Lifecycle

```text
Step 1
Write Dockerfile
        │
        ▼

Step 2
Build Docker Image
        │
        ▼

Step 3
Run Docker Container
```

---

## Common Commands

### Build Image

```bash
docker build
```

Converts:

```text
Dockerfile → Docker Image
```

---

### Run Container

```bash
docker run
```

Converts:

```text
Docker Image → Running Container
```

---

# Docker Engine

Docker relies heavily on:

```text
Docker Engine
```

Architecture:

```text
Docker Commands
        │
        ▼
Docker Engine
        │
        ▼
Images & Containers
```

---

# 1️⃣3️⃣ Drawbacks of Docker

One major concern:

## Single Point of Failure (SPOF)

```text
Docker Engine
      │
      ▼
All Containers
```

If Docker Engine fails:

```text
Docker Engine Down
        │
        ▼
Containers Impacted
```

---

### Additional Challenges

* Layer management
* Storage overhead
* Dependency on Docker Engine

---

# 1️⃣4️⃣ Introduction to Buildah

Buildah is a modern container image-building tool.

It addresses several Docker limitations.

---

## Why Buildah?

### Solves

✅ Layer management challenges

✅ Reduced dependency on Docker Engine

✅ Better flexibility

✅ OCI-compliant image generation

---

## OCI

OCI stands for:

```text
Open Container Initiative
```

A standard for container images and runtimes.

---

## Buildah Advantages

```text
Buildah
│
├── Lightweight
├── OCI Compliant
├── Flexible
├── Script Friendly
├── Podman Integration
└── Better Modern Workflow
```

---

## Buildah Workflow

Instead of Dockerfile-heavy workflows:

```text
Shell Script
      │
      ▼
Buildah Commands
      │
      ▼
Container Image
```

---

## Works Well With

* Podman
* Skopeo
* OCI Ecosystem

---

# 🎯 Key Interview Questions

### Q1. Why were containers introduced?

**Answer:**
Containers were introduced to solve resource inefficiencies of virtual machines and improve application portability, scalability, and deployment speed.

---

### Q2. Why are containers lightweight?

**Answer:**
Containers do not include a complete operating system. They share the host OS kernel and only package the application, dependencies, and required libraries.

---

### Q3. Difference between VM and Container?

**Answer:**
VMs include a full operating system and provide stronger isolation, whereas containers share the host OS kernel, making them lightweight and faster.

---

### Q4. What is Docker?

**Answer:**
Docker is a containerization platform used to create, manage, package, and deploy containers.

---

### Q5. What is the Docker lifecycle?

```text
Dockerfile
    ↓
Docker Image
    ↓
Docker Container
```

---

### Q6. What is Buildah?

**Answer:**
Buildah is an OCI-compliant image-building tool that allows building container images without depending heavily on Docker Engine and integrates well with Podman and Skopeo.

---

# 📝 Summary

```text
Physical Servers
       ↓
Virtual Machines
       ↓
Containers
```

### Core Takeaways

✅ Containers are an evolution of Virtual Machines

✅ Containers are lightweight because they do not contain a full OS

✅ Containers package:

* Application
* Libraries
* Dependencies

✅ Docker popularized containerization

✅ Docker Workflow:

Dockerfile → Image → Container

✅ Buildah is a modern image-building alternative

✅ Containers improve:

* Resource utilization
* Scalability
* Deployment speed
* Portability

✅ Virtual Machines still provide stronger isolation and security than containers.
