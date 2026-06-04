# Day 34 – Kubernetes Deployments & ReplicaSets 🚀

## Complete DevOps Course Notes

> **Topic:** Kubernetes Deployment, ReplicaSet, Pod
> **Objective:** Understand why Deployments are preferred over Pods, how ReplicaSets provide self-healing, and how Kubernetes achieves high availability and zero-downtime operations.

---

# 📌 Learning Outcomes

By the end of this session, you should be able to:

* Understand the difference between **Container**, **Pod**, and **Deployment**
* Explain why Pods are rarely created directly in production
* Understand the purpose of **ReplicaSets**
* Learn Kubernetes **Auto-Healing**
* Learn Kubernetes **Scaling**
* Understand the relationship between:

  * Deployment
  * ReplicaSet
  * Pod
* Perform basic deployment operations using `kubectl`

---

# 1️⃣ Container vs Pod vs Deployment

This is one of the most commonly asked Kubernetes interview questions.

---

## A. Container

A container is the smallest executable unit of an application.

Example using Docker:

```bash
docker run -d nginx
```

While running a container, we typically provide:

| Option                | Purpose                |
| --------------------- | ---------------------- |
| `-p`                  | Port Mapping           |
| `-v`                  | Volume Mount           |
| `--network`           | Network Configuration  |
| Image Name            | Container Image        |
| Environment Variables | Runtime Configurations |

---

### Traditional Docker Workflow

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

## B. Pod

Kubernetes introduced Pods to make container management more declarative.

Instead of:

```bash
docker run ...
```

we define everything in a YAML file.

Example:

```yaml
apiVersion: v1
kind: Pod

spec:
  containers:
  - name: nginx
    image: nginx
```

---

### What is a Pod?

A Pod is:

> A running specification of one or more containers in Kubernetes.

---

### Pod Characteristics

✅ Can contain a single container

```text
Pod
 └── Container
```

✅ Can contain multiple containers

```text
Pod
 ├── App Container
 └── Sidecar Container
```

---

### Why Multiple Containers in a Pod?

Common use cases:

* Service Mesh
* Logging Agents
* Monitoring Agents
* API Gateway Sidecars

---

### Shared Resources Inside a Pod

Containers inside the same Pod share:

```text
┌──────────────────┐
│       Pod        │
│                  │
│ Container A      │
│ Container B      │
│                  │
│ Shared Network   │
│ Shared Storage   │
└──────────────────┘
```

Advantages:

* Communication via `localhost`
* Shared volume access
* Simplified networking

---

# C. Deployment

A Deployment is a higher-level Kubernetes resource used to manage Pods.

---

## Why Not Create Pods Directly?

Pods alone do NOT provide:

❌ Auto-Healing

❌ Auto-Scaling

❌ Rolling Updates

❌ High Availability

❌ Zero Downtime Deployments

---

### Kubernetes Solution

Use:

```text
Deployment
```

instead of:

```text
Pod
```

---

# 2️⃣ Kubernetes Deployment Architecture

When you create a Deployment:

```text
Deployment
     │
     ▼
 ReplicaSet
     │
     ▼
    Pod(s)
```

---

## Detailed Flow

```text
User
 │
 ▼
Deployment YAML
 │
 ▼
Deployment
 │
 ▼
ReplicaSet
 │
 ▼
Pod-1
Pod-2
Pod-3
```

---

# 3️⃣ Role of Deployment

A Deployment is responsible for:

### ✔ Managing Application Lifecycle

* Create Pods
* Update Pods
* Scale Pods
* Rollback Pods

---

### ✔ Zero Downtime Updates

When a new version is deployed:

```text
Old Pods Running
       │
       ▼
New Pods Created
       │
       ▼
Old Pods Removed
```

Users experience minimal or no downtime.

---

### ✔ Scaling

Example:

```yaml
replicas: 3
```

Deployment ensures:

```text
Pod-1
Pod-2
Pod-3
```

are running.

---

# 4️⃣ What is a ReplicaSet?

A ReplicaSet is a Kubernetes Controller.

Its job:

> Ensure the desired number of Pods are always running.

---

## Example

Deployment YAML:

```yaml
replicas: 3
```

Desired State:

```text
3 Pods
```

Current State:

```text
3 Pods
```

ReplicaSet continuously checks:

```text
Desired State == Actual State ?
```

---

# ReplicaSet Working Diagram

```text
Desired State
(replicas: 3)
        │
        ▼
   ReplicaSet
        │
        ▼
Actual State
Pod-1
Pod-2
Pod-3
```

---

# 5️⃣ Auto-Healing in Kubernetes

One of Kubernetes' most powerful features.

---

## Scenario

Deployment:

```yaml
replicas: 3
```

Current State:

```text
Pod-1
Pod-2
Pod-3
```

---

### Accidentally Delete a Pod

```bash
kubectl delete pod pod-2
```

---

### Immediately After Deletion

```text
Pod-1
Pod-3
```

ReplicaSet detects:

```text
Desired = 3
Actual = 2
```

Mismatch found!

---

### ReplicaSet Action

```text
Create New Pod
```

Result:

```text
Pod-1
Pod-3
Pod-4
```

Back to:

```text
3 Running Pods
```

---

## Auto-Healing Diagram

```text
           Desired Replicas = 3
                    │
                    ▼
              ReplicaSet
                    │
      ┌─────────────┴─────────────┐
      │                           │
      ▼                           ▼
  Pod Deleted              Detects Mismatch
      │                           │
      └─────────────┬─────────────┘
                    ▼
            Creates New Pod
                    ▼
           Desired State Restored
```

---

# 6️⃣ Kubernetes Controllers

A Controller is a process that continuously watches cluster resources.

Its purpose:

> Ensure actual state matches desired state.

---

## Desired State vs Actual State

### Desired State

Defined in YAML

```yaml
replicas: 3
```

---

### Actual State

Current cluster condition

```text
2 Pods Running
```

---

### Controller Action

```text
Desired State = 3
Actual State = 2

Mismatch Detected

Create 1 More Pod
```

---

# Controller Reconciliation Loop

```text
         Desired State
               │
               ▼
        Kubernetes API
               │
               ▼
          Controller
               │
               ▼
         Actual State
               │
      Mismatch?
         / \
       Yes  No
        │
        ▼
 Correct State
```

---

# 7️⃣ Deployment YAML Structure

Basic Deployment Example:

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
      - name: nginx
        image: nginx
```

---

## Important Fields

| Field        | Purpose                 |
| ------------ | ----------------------- |
| `kind`       | Resource Type           |
| `replicas`   | Number of Pod Copies    |
| `selector`   | Finds Matching Pods     |
| `template`   | Pod Blueprint           |
| `containers` | Container Configuration |
| `image`      | Docker Image            |

---

# 8️⃣ Practical Commands

---

## Create Pod

```bash
kubectl apply -f pod.yaml
```

---

## List Pods

```bash
kubectl get pods
```

---

## Detailed Pod Information

```bash
kubectl get pods -o wide
```

---

## Delete Pod

```bash
kubectl delete pod <pod-name>
```

---

## Create Deployment

```bash
kubectl apply -f deployment.yaml
```

---

## View Deployments

```bash
kubectl get deploy
```

---

## View ReplicaSets

```bash
kubectl get rs
```

---

## Watch Pods in Real-Time

```bash
kubectl get pods -w
```

---

## View All Resources

```bash
kubectl get all
```

---

## View Resources Across All Namespaces

```bash
kubectl get all -A
```

---

# 9️⃣ Live Deployment Lifecycle

When deployment is created:

```text
kubectl apply -f deployment.yaml
```

---

### Kubernetes Automatically Creates

```text
Deployment
    │
    ▼
ReplicaSet
    │
    ▼
Pod
```

---

### Verify

```bash
kubectl get deploy
kubectl get rs
kubectl get pods
```

Expected Output:

```text
Deployment Created
ReplicaSet Created
Pod Created
```

---

# 🔟 Scaling Example

Current:

```yaml
replicas: 1
```

Pods:

```text
Pod-1
```

---

Modify:

```yaml
replicas: 3
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

---

Result:

```text
Pod-1
Pod-2
Pod-3
```

Created automatically by ReplicaSet.

---

## Scaling Visualization

```text
replicas: 1
      │
      ▼
     Pod-1

Change to

replicas: 3
      │
      ▼

ReplicaSet Creates

Pod-1
Pod-2
Pod-3
```

---

# 🎯 Important Interview Questions

---

## Q1. Difference Between Container, Pod, and Deployment?

| Feature                | Container | Pod | Deployment |
| ---------------------- | --------- | --- | ---------- |
| Runs Application       | ✅         | ✅   | ✅          |
| YAML Based             | ❌         | ✅   | ✅          |
| Multiple Containers    | ❌         | ✅   | ✅          |
| Auto-Healing           | ❌         | ❌   | ✅          |
| Auto-Scaling           | ❌         | ❌   | ✅          |
| Rolling Updates        | ❌         | ❌   | ✅          |
| Production Recommended | ❌         | ❌   | ✅          |

---

## Q2. Difference Between Deployment and ReplicaSet?

| Deployment                     | ReplicaSet              |
| ------------------------------ | ----------------------- |
| High-level abstraction         | Controller              |
| Manages ReplicaSets            | Manages Pods            |
| Supports updates and rollbacks | Maintains replica count |
| User-created                   | Usually auto-created    |

---

## Q3. What is a Kubernetes Controller?

**Answer:**

A Kubernetes Controller continuously monitors cluster resources and ensures that the actual state matches the desired state defined in Kubernetes manifests.

---

## Q4. What is Auto-Healing?

**Answer:**

Auto-Healing is Kubernetes' ability to automatically recreate failed or deleted Pods to maintain the desired replica count.

---

## Q5. Why Deployments Are Preferred Over Pods?

**Answer:**

Deployments provide:

* Auto-Healing
* Auto-Scaling
* Rolling Updates
* Rollbacks
* High Availability
* Zero Downtime Deployments

which Pods alone cannot provide.

---

# 📝 Key Takeaways

✅ Never deploy applications directly as Pods in production.

✅ Deployments are the standard way to run applications in Kubernetes.

✅ Deployments create ReplicaSets.

✅ ReplicaSets create and maintain Pods.

✅ ReplicaSets implement Kubernetes Auto-Healing.

✅ Desired State should always match Actual State.

✅ Controllers are responsible for maintaining cluster state.

---

# Final Architecture Summary

```text
                    USER
                      │
                      ▼
              Deployment YAML
                      │
                      ▼
                Deployment
                      │
                      ▼
                 ReplicaSet
                      │
      ┌───────────────┼───────────────┐
      ▼               ▼               ▼
    Pod-1           Pod-2           Pod-3
      │               │               │
      └────── Auto-Healing ───────────┘

ReplicaSet ensures:
Desired Pods = Running Pods
```

> **Golden Rule:** In production Kubernetes environments, create **Deployments**, not standalone **Pods**. Deployments leverage ReplicaSets to provide **self-healing, scalability, high availability, and zero-downtime application management**. 🚀
