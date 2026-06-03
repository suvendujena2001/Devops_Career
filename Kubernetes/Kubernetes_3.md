# Day 33 – Kubernetes Pods: Deploy Your First Application 🚀

> **Goal:** Understand the fundamental Kubernetes object called a **Pod**, learn why Pods exist, install Kubernetes locally, deploy your first application, and explore essential debugging commands.

---

# 📖 Learning Objectives

By the end of this session, you should be able to:

✅ Understand what a Pod is

✅ Differentiate between Docker Containers and Kubernetes Pods

✅ Understand why Kubernetes uses YAML manifests

✅ Install and configure `kubectl`

✅ Create a local Kubernetes cluster using Minikube

✅ Deploy your first application as a Pod

✅ Debug Pods using kubectl commands

✅ Understand the relationship between Pods and Deployments

---

# 🏗️ Prerequisites

Before proceeding, make sure you understand:

| Day    | Topic                   |
| ------ | ----------------------- |
| Day 30 | Docker vs Kubernetes    |
| Day 31 | Kubernetes Architecture |
| Day 32 | Kubernetes Installation |

These concepts form the foundation required to understand Pods effectively.

---

# Why Kubernetes?

Kubernetes became popular because it solves several limitations of standalone container platforms.

## Major Advantages

```text
+--------------------------------+
|         Kubernetes             |
+--------------------------------+
           |
           |
  +--------+--------+
  |        |        |
Scaling  Healing  Enterprise
(Auto)   (Auto)   Features
```

### Key Benefits

| Feature             | Description                                 |
| ------------------- | ------------------------------------------- |
| Auto Scaling        | Automatically increases/decreases resources |
| Auto Healing        | Restarts failed containers                  |
| Clustering          | Multiple nodes work together                |
| Enterprise Features | High availability, security, monitoring     |

---

# Understanding Kubernetes Pods

## The Most Important Concept

### In Docker

You deploy:

```text
Application
     ↓
Container
     ↓
Docker Engine
```

### In Kubernetes

You deploy:

```text
Application
     ↓
Container
     ↓
Pod
     ↓
Kubernetes Cluster
```

> **A Pod is the smallest deployable unit in Kubernetes.**

---

# What is a Pod?

## Definition

A **Pod** is a Kubernetes object that defines **how one or more containers should run**.

### Simple Explanation

A Pod acts as a wrapper around containers.

```text
+--------------------------------+
|             POD                |
|                                |
|  +--------------------------+  |
|  |      Container           |  |
|  |      (Application)       |  |
|  +--------------------------+  |
|                                |
+--------------------------------+
```

---

# Why Not Deploy Containers Directly?

A common question:

> Docker runs containers directly. Why does Kubernetes introduce Pods?

### Answer

Kubernetes focuses on:

* Standardization
* Declarative configuration
* Automation
* Enterprise-scale management

Instead of using long command-line arguments, Kubernetes stores everything in YAML files.

---

# Docker vs Kubernetes Approach

## Docker

Running a container:

```bash
docker run -d \
--name nginx \
-p 80:80 \
nginx
```

Everything is provided as command-line arguments.

---

## Kubernetes

Everything is defined declaratively:

```yaml
apiVersion: v1
kind: Pod

metadata:
  name: nginx

spec:
  containers:
  - name: nginx
    image: nginx
```

### Visualization

```text
Docker
------
Command Line
      ↓
Container

Kubernetes
-----------
YAML File
      ↓
Pod
      ↓
Container
```

---

# Why YAML Files?

Kubernetes uses YAML because:

### Benefits

✅ Version controlled in Git

✅ Easily shareable

✅ Human-readable

✅ Declarative

✅ Reproducible

### Real-world Scenario

A DevOps engineer can simply open:

```text
pod.yaml
```

and immediately understand:

* Which image is used
* Which ports are exposed
* What volumes are mounted
* Networking configuration
* Resource requirements

---

# Pod Architecture

## Single Container Pod (Most Common)

```text
+----------------------+
|        POD           |
|                      |
|   +--------------+   |
|   | Container    |   |
|   +--------------+   |
|                      |
+----------------------+
```

---

## Multi-Container Pod

```text
+--------------------------------+
|             POD                |
|                                |
| +------------+ +------------+  |
| | ContainerA | | ContainerB |  |
| +------------+ +------------+  |
|                                |
+--------------------------------+
```

---

# Why Multiple Containers in a Pod?

Used in advanced scenarios such as:

* Sidecar Containers
* Init Containers
* Logging Containers
* Service Mesh Components

---

# Advantages of Containers Inside Same Pod

## Shared Networking

Containers can communicate using localhost.

```text
Container A
     |
 localhost
     |
Container B
```

Example:

```bash
localhost:3000
```

No external networking required.

---

## Shared Storage

```text
+--------------------+
|      POD           |
|                    |
| Container A        |
|      ↕             |
| Shared Volume      |
|      ↕             |
| Container B        |
+--------------------+
```

Both containers can access the same files.

---

# Pod Networking

Every Pod receives its own IP Address.

```text
Pod
 |
 +--> IP Address Assigned
```

### Important

❌ Containers do not receive individual Kubernetes IPs

✅ Pods receive IP addresses

---

### Example

```text
Pod
 |
 +-- Container
```

Access Application:

```text
Pod IP → Application
```

---

# Kubernetes Pod Manifest

Example:

```yaml
apiVersion: v1

kind: Pod

metadata:
  name: nginx

spec:
  containers:
  - name: nginx
    image: nginx:1.14.2

    ports:
    - containerPort: 80
```

---

# Understanding the Manifest

| Section       | Purpose                |
| ------------- | ---------------------- |
| apiVersion    | Kubernetes API version |
| kind          | Object type            |
| metadata      | Name and labels        |
| spec          | Desired state          |
| containers    | Container definitions  |
| image         | Docker image           |
| containerPort | Application port       |

---

# kubectl – Kubernetes CLI

Just like Docker has:

```bash
docker
```

Kubernetes has:

```bash
kubectl
```

---

## Purpose

Used to interact with Kubernetes clusters.

```text
Developer
    |
 kubectl
    |
Kubernetes Cluster
```

---

# Common kubectl Commands

## View Nodes

```bash
kubectl get nodes
```

---

## View Pods

```bash
kubectl get pods
```

---

## View Deployments

```bash
kubectl get deployments
```

---

## Delete Pod

```bash
kubectl delete pod <pod-name>
```

---

# Installing kubectl

Visit Kubernetes documentation:

```text
Install Tools → kubectl
```

Choose your platform:

* Linux
* macOS
* Windows

Install using the provided command.

Verify:

```bash
kubectl version
```

---

# Creating a Local Kubernetes Cluster

Several options exist:

| Tool     | Description            |
| -------- | ---------------------- |
| Minikube | Beginner-friendly      |
| Kind     | Kubernetes in Docker   |
| K3s      | Lightweight Kubernetes |
| MicroK8s | Canonical Kubernetes   |

---

# Why Minikube?

### Pros

✅ Easy setup

✅ Lightweight

✅ Perfect for learning

✅ No cloud cost

---

# Minikube Architecture

```text
Laptop
   |
   v
Virtual Machine
   |
   v
Single Node Kubernetes Cluster
```

---

# Production vs Minikube

## Production

```text
+------------+
| Master 1   |
+------------+

+------------+
| Master 2   |
+------------+

+------------+
| Master 3   |
+------------+

     ↓

+------------+
| Worker 1   |
+------------+

+------------+
| Worker 2   |
+------------+

+------------+
| Worker N   |
+------------+
```

---

## Minikube

```text
+------------------+
| Single Node      |
| Control Plane    |
| Worker Node      |
+------------------+
```

---

# Starting Minikube

Basic:

```bash
minikube start
```

---

Advanced:

```bash
minikube start \
--memory=4096 \
--driver=hyperkit
```

---

# Verify Cluster

```bash
kubectl get nodes
```

Example Output:

```text
NAME       STATUS
minikube   Ready
```

---

# Deploying Your First Application

Create:

```yaml
apiVersion: v1

kind: Pod

metadata:
  name: nginx

spec:
  containers:
  - name: nginx
    image: nginx:1.14.2

    ports:
    - containerPort: 80
```

Save as:

```text
pod.yaml
```

---

# Create the Pod

```bash
kubectl create -f pod.yaml
```

Output:

```text
pod/nginx created
```

---

# Verify Pod

```bash
kubectl get pods
```

Example:

```text
NAME    READY   STATUS
nginx   1/1     Running
```

---

# Detailed Pod Information

```bash
kubectl get pods -o wide
```

Example:

```text
NAME    IP
nginx   10.x.x.x
```

---

# Accessing the Pod

SSH into Minikube:

```bash
minikube ssh
```

Then:

```bash
curl <pod-ip>
```

Expected Output:

```text
Welcome to nginx!
```

---

# Lifecycle of a Pod

```text
pod.yaml
     |
kubectl create
     |
Pod Created
     |
Container Starts
     |
Running State
```

---

# Pod Debugging Commands

One of the most important DevOps skills is troubleshooting.

---

## Describe Pod

```bash
kubectl describe pod nginx
```

Provides:

* Events
* Current status
* Scheduling information
* Errors
* Networking details

---

### Visual Representation

```text
kubectl describe pod
        |
        +--> Events
        +--> Conditions
        +--> Errors
        +--> IP Address
        +--> Container Details
```

---

## View Logs

```bash
kubectl logs nginx
```

Useful for:

* Application failures
* Runtime issues
* Startup errors

---

### Debugging Flow

```text
Application Issue
        |
        |
   kubectl describe
        |
   Infrastructure?
        |
       Yes
        |
      Fix

        OR

   kubectl logs
        |
   Application?
        |
       Yes
        |
      Fix
```

---

# Equivalent Docker Command

The following Pod:

```yaml
image: nginx
containerPort: 80
```

is approximately equivalent to:

```bash
docker run -d \
--name nginx \
-p 80:80 \
nginx
```

---

# Important Interview Questions

## Q1. What is a Pod?

**Answer:**

A Pod is the smallest deployable unit in Kubernetes that contains one or more containers and defines how those containers should run.

---

## Q2. Can a Pod contain multiple containers?

**Answer:**

Yes.

A Pod can contain one or more containers that share:

* Network namespace
* Storage volumes
* Lifecycle

---

## Q3. How do containers communicate inside a Pod?

**Answer:**

Using localhost because they share the same network namespace.

---

## Q4. How do you check Pod logs?

```bash
kubectl logs <pod-name>
```

---

## Q5. How do you get detailed Pod information?

```bash
kubectl describe pod <pod-name>
```

---

## Q6. How do you list all Pods?

```bash
kubectl get pods
```

---

# Pods vs Deployments

Pods are generally not used directly in production.

```text
Deployment
      |
      v
    Pod
      |
      v
 Container
```

---

# Why Use Deployments?

Deployments provide:

✅ Auto Healing

✅ Auto Scaling

✅ Rolling Updates

✅ Rollbacks

✅ Replica Management

---

### Comparison

| Feature          | Pod | Deployment |
| ---------------- | --- | ---------- |
| Run Container    | ✅   | ✅          |
| Auto Healing     | ❌   | ✅          |
| Auto Scaling     | ❌   | ✅          |
| Rollback         | ❌   | ✅          |
| Rolling Updates  | ❌   | ✅          |
| Production Ready | ❌   | ✅          |

---

# Key Takeaways

### Kubernetes Hierarchy

```text
Deployment
     ↓
Pod
     ↓
Container
     ↓
Application
```

---

### Core Concepts Learned

✅ Pod is the smallest deployable unit

✅ Pod is a wrapper around containers

✅ Kubernetes uses YAML manifests

✅ Pods can contain one or more containers

✅ Containers inside a Pod share networking and storage

✅ kubectl is the Kubernetes CLI

✅ Minikube provides a local Kubernetes cluster

✅ Pods can be debugged using:

```bash
kubectl describe pod <name>
kubectl logs <name>
```

✅ Deployments are built on top of Pods and provide enterprise-grade features

---

# Final Summary

Pods are the **foundation of Kubernetes**. While Docker focuses on running containers directly, Kubernetes introduces Pods as an abstraction layer that standardizes deployment, networking, storage, and lifecycle management. Understanding Pods thoroughly is essential because every higher-level Kubernetes object—such as Deployments, StatefulSets, DaemonSets, and Jobs—ultimately manages Pods. Mastering Pods today creates the foundation for learning **Deployments, Services, Auto-Healing, Auto-Scaling, and Production Kubernetes Architecture** in the upcoming topics. 🚀
