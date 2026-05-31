# 🚀 Day 24 – Docker Zero to Hero (Part 1)

## From Docker Basics to Best Practices

> **Goal:** Understand Docker fundamentals, container architecture, Docker lifecycle, Docker terminologies, installation, first Dockerfile, image creation, container execution, and image publishing.

---

# 📖 Table of Contents

1. Why Containers are Lightweight
2. Virtual Machines vs Containers
3. Docker Introduction
4. Docker Architecture
5. Docker Lifecycle
6. Docker Terminologies
7. Docker Registry & Docker Hub
8. Docker Installation
9. Common Docker Permission Issues
10. Writing Your First Dockerfile
11. Building Docker Images
12. Running Docker Containers
13. Pushing Images to Docker Hub
14. Interview Notes & Key Takeaways

---

# 1️⃣ Why Are Containers Lightweight?

One of the most common interview questions:

> **Why are containers lightweight compared to Virtual Machines?**

## Virtual Machine Architecture

```text
+----------------------------------+
| Application                      |
+----------------------------------+
| Application Dependencies         |
+----------------------------------+
| Guest Operating System           |
+----------------------------------+
| Hypervisor                       |
+----------------------------------+
| Host Operating System            |
+----------------------------------+
| Physical Infrastructure          |
+----------------------------------+
```

Each VM contains:

* Full Operating System
* Kernel
* Libraries
* Binaries
* Dependencies

Therefore:

❌ More Storage

❌ More Memory

❌ More CPU Consumption

---

## Container Architecture

```text
+----------------------------------+
| Application                      |
+----------------------------------+
| Application Dependencies         |
+----------------------------------+
| Minimal System Dependencies      |
+----------------------------------+
| Docker Engine                    |
+----------------------------------+
| Host Operating System Kernel     |
+----------------------------------+
| Physical Infrastructure          |
+----------------------------------+
```

Containers:

✅ Share Host OS Kernel

✅ Contain only required dependencies

✅ Start quickly

✅ Consume fewer resources

---

# 2️⃣ Logical Isolation in Containers

Many people say:

> "Containers don't have an operating system."

This statement is partially correct.

Containers contain a **minimal userspace environment** to provide isolation.

---

## Files Present Inside Containers

```text
/
├── bin
├── sbin
├── etc
├── lib
├── usr
├── var
└── root
```

### Purpose

| Directory | Purpose                |
| --------- | ---------------------- |
| `/bin`    | Binary executables     |
| `/sbin`   | System binaries        |
| `/etc`    | Configuration files    |
| `/lib`    | Libraries              |
| `/usr`    | User-related binaries  |
| `/var`    | Logs and variable data |
| `/root`   | Root user's home       |

These files help create:

✅ Process Isolation

✅ Security Boundaries

✅ Independent Runtime Environment

---

## What Containers Use From Host OS?

Containers share:

```text
Host OS Kernel
│
├── File System
├── Networking Stack
├── System Calls
├── Namespaces
└── Control Groups (cgroups)
```

### Shared Components

| Component   | Purpose               |
| ----------- | --------------------- |
| Kernel      | Core OS functionality |
| Namespaces  | Isolation             |
| cgroups     | Resource control      |
| Networking  | Network communication |
| File System | Storage management    |

---

# 3️⃣ Container Size Comparison

## Ubuntu VM Image

```text
Ubuntu VM ≈ 2.3 GB
```

## Ubuntu Docker Image

```text
Ubuntu Docker Image ≈ 28.16 MB
```

---

### Visual Comparison

```text
VM Image

████████████████████████████████████
2.3 GB


Docker Image

█
28 MB
```

Almost:

> 🚀 100x Smaller

---

# 4️⃣ What is Docker?

Docker is a platform that implements containerization.

### Simple Definition

```text
Containerization = Concept

Docker = Platform implementing that concept
```

---

## Alternatives to Docker

| Tool    | Description          |
| ------- | -------------------- |
| Docker  | Most popular         |
| Podman  | Docker alternative   |
| Buildah | Image building       |
| Skopeo  | Registry interaction |

---

# 5️⃣ Docker Architecture

```text
                User

                 │
                 ▼

         Docker CLI Client

                 │
                 ▼

         Docker Daemon (dockerd)

        ┌────────┼────────┐
        │        │        │
        ▼        ▼        ▼

     Images   Containers Registry
```

---

## Components

### Docker Client

Commands executed by users:

```bash
docker build
docker run
docker pull
docker push
```

---

### Docker Daemon (dockerd)

The heart of Docker.

Responsible for:

* Building images
* Running containers
* Managing networks
* Managing storage
* Interacting with registries

---

### Docker Registry

Stores Docker Images.

Examples:

* Docker Hub
* Quay.io
* Private Registry

---

# 6️⃣ Docker Lifecycle

A very important interview topic.

```text
          Write Dockerfile
                   │
                   ▼
          docker build
                   │
                   ▼
            Docker Image
                   │
                   ▼
            docker run
                   │
                   ▼
         Docker Container
                   │
                   ▼
            docker push
                   │
                   ▼
           Docker Registry
```

---

## Lifecycle Flow

### Step 1

Create Dockerfile

### Step 2

Build Image

```bash
docker build
```

### Step 3

Create Container

```bash
docker run
```

### Step 4

Share Image

```bash
docker push
```

---

# 7️⃣ Important Docker Terminologies

---

## Docker Daemon

```text
dockerd
```

Background service responsible for:

* Building images
* Running containers
* Pulling images
* Pushing images

Think of it as:

> ❤️ Heart of Docker

---

## Docker Client

CLI used by users.

Example:

```bash
docker run nginx
```

---

## Dockerfile

A file containing instructions to build an image.

Example:

```dockerfile
FROM ubuntu
RUN apt update
```

---

## Docker Image

Immutable blueprint.

Think:

```text
Image = Class
Container = Object
```

---

## Docker Container

Running instance of an image.

---

## Docker Registry

Storage location for images.

Examples:

* Docker Hub
* Quay.io
* AWS ECR
* Azure ACR

---

# 8️⃣ Docker Hub vs GitHub

Very common interview question.

| GitHub             | Docker Hub           |
| ------------------ | -------------------- |
| Stores Source Code | Stores Docker Images |
| Version Control    | Image Versioning     |
| Git Repositories   | Image Repositories   |
| Developers         | DevOps/Containers    |

---

# 9️⃣ Installing Docker on Ubuntu

---

## Update Package Repository

```bash
sudo apt update
```

---

## Install Docker

```bash
sudo apt install docker.io -y
```

---

## Verify Installation

```bash
sudo systemctl status docker
```

Expected:

```text
Active: running
```

---

# 🔟 Common Docker Permission Issue

After installation:

```bash
docker run hello-world
```

You may get:

```text
permission denied
```

---

## Why?

Docker runs as root.

Current user doesn't belong to Docker group.

---

## Fix

```bash
sudo usermod -aG docker ubuntu
```

Where:

```text
ubuntu = username
```

---

## Then Logout/Login

OR

```bash
source ~/.profile
```

Now test again:

```bash
docker run hello-world
```

Success!

---

# 1️⃣1️⃣ First Docker Application

## Python Application

```python
print("Hello World")
```

File:

```text
app.py
```

---

# 1️⃣2️⃣ First Dockerfile

```dockerfile
FROM ubuntu:latest

WORKDIR /app

COPY . .

RUN apt-get update && \
    apt-get install python3 -y

CMD ["python3", "app.py"]
```

---

## Dockerfile Breakdown

### FROM

```dockerfile
FROM ubuntu:latest
```

Uses Ubuntu base image.

---

### WORKDIR

```dockerfile
WORKDIR /app
```

Equivalent to:

```bash
cd /app
```

---

### COPY

```dockerfile
COPY . .
```

Copies current folder contents.

---

### RUN

```dockerfile
RUN apt-get install python3 -y
```

Installs Python.

---

### CMD

```dockerfile
CMD ["python3", "app.py"]
```

Runs application when container starts.

---

# 1️⃣3️⃣ Building Docker Image

## Command

```bash
docker build -t username/my-first-docker-image:latest .
```

---

## Breakdown

```text
docker build
       │
       ├── -t
       │
       ├── image name
       │
       ├── tag
       │
       └── .
           Current Directory
```

---

Example:

```bash
docker build \
-t abhishekf5/my-first-docker-image:latest .
```

---

# 1️⃣4️⃣ Running Docker Container

## Run Container

```bash
docker run -it image-id
```

or

```bash
docker run -it my-first-docker-image
```

---

Output:

```text
Hello World
```

---

## What Happened Internally?

```text
Docker Image
      │
docker run
      │
      ▼
Docker Container
      │
      ▼
Application Started
```

---

# 1️⃣5️⃣ Docker Login

Before pushing images:

```bash
docker login
```

Provide:

```text
Username
Password
```

---

# 1️⃣6️⃣ Push Image to Docker Hub

```bash
docker push username/my-first-docker-image:latest
```

---

## Flow

```text
Local Image
     │
     ▼
docker push
     │
     ▼
Docker Hub Repository
     │
     ▼
Available Worldwide
```

---

# Pulling Image Anywhere

Anyone can download:

```bash
docker pull username/my-first-docker-image:latest
```

---

# Complete Docker Workflow

```text
Write Application
        │
        ▼
Write Dockerfile
        │
        ▼
docker build
        │
        ▼
Docker Image
        │
        ▼
docker run
        │
        ▼
Docker Container
        │
        ▼
docker login
        │
        ▼
docker push
        │
        ▼
Docker Hub
        │
        ▼
docker pull
        │
        ▼
Run Anywhere
```

---

# 🎯 Important Interview Questions

### Q1. Why are containers lightweight?

Because they share the host OS kernel and only package application-specific dependencies.

---

### Q2. What is Docker?

A containerization platform used to build, run, and manage containers.

---

### Q3. What is Docker Daemon?

Background service (`dockerd`) that executes Docker commands and manages containers/images.

---

### Q4. Difference Between Image and Container?

| Image     | Container        |
| --------- | ---------------- |
| Blueprint | Running Instance |
| Static    | Dynamic          |
| Immutable | Mutable Runtime  |

---

### Q5. Difference Between Docker Hub and GitHub?

| GitHub           | Docker Hub         |
| ---------------- | ------------------ |
| Source Code      | Docker Images      |
| Git Repositories | Image Repositories |

---

### Q6. What is Dockerfile?

A file containing instructions used to build Docker images.

---

# 🏆 Key Takeaways

✅ Containers share Host OS Kernel

✅ Containers are significantly smaller than VMs

✅ Docker implements containerization

✅ Docker Daemon is the heart of Docker

✅ Dockerfile → Image → Container

✅ Docker Hub stores Docker images

✅ Docker images can be shared and run anywhere

✅ A Docker image contains application + dependencies

✅ Docker drastically reduces deployment complexity

---

# 💡 One-Line Summary

> **Docker enables developers to package applications along with their dependencies into lightweight, portable containers that can run consistently across any environment.** 🚀
