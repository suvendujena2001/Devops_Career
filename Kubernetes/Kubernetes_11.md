# 📘 Day 41 - Kubernetes Live Project

# ConfigMaps & Secrets in Kubernetes

### Complete Study Notes with Diagrams, Interview Points & Practical Demonstration.

---

# 📚 Table of Contents

1. Introduction
2. Why ConfigMaps Exist?
3. Why Secrets Exist?
4. ConfigMap vs Secret
5. Internal Working Architecture
6. Creating ConfigMaps
7. Using ConfigMaps Inside Pods
8. ConfigMap as Environment Variables
9. Problem with Environment Variables
10. ConfigMap as Volumes (Recommended)
11. Live Demo Workflow
12. Kubernetes Secrets
13. Types of Secrets
14. Secret Creation
15. Security Best Practices
16. Interview Questions
17. Key Takeaways

---

# 1. Introduction

In every application, there are configuration values that the application requires before it can function properly.

Examples include:

* Database Host
* Database Port
* API Endpoint
* Cache URL
* Logging Level
* SMTP Server
* Feature Flags

Some of these values are **public configuration**, while others are **sensitive credentials**.

Kubernetes provides two separate resources for managing them:

| Resource  | Purpose                           |
| --------- | --------------------------------- |
| ConfigMap | Store non-sensitive configuration |
| Secret    | Store sensitive configuration     |

---

# 2. Why ConfigMaps Exist?

Imagine a backend application.

```
                User
                  │
                  ▼
        ┌─────────────────┐
        │ Backend Service │
        └─────────────────┘
                  │
                  ▼
           MySQL Database
```

The backend needs information such as:

```
DB_HOST
DB_PORT
DB_NAME
DB_CONNECTION_TYPE
MAX_CONNECTIONS
```

Without these values, the application cannot connect to the database.

---

## ❌ Bad Practice

Hardcoding configuration inside the application.

```python
DB_PORT = 3306
```

Problems:

* Need code changes for configuration updates
* Rebuild Docker image
* Redeploy application
* Difficult across environments

---

## ✅ Best Practice

Keep configuration outside the application.

```
Application
      │
      ▼
Environment Variables
OR
Configuration Files
```

This keeps applications portable and environment-independent.

---

# 3. How Kubernetes Solves This

Kubernetes introduces **ConfigMaps**.

```
Developer
     │
     ▼
Create ConfigMap
     │
     ▼
Kubernetes Cluster
     │
     ▼
Pod reads ConfigMap
```

ConfigMap stores configuration separately from application code.

---

# Example ConfigMap Data

```text
DB_PORT = 3306
DB_HOST = mysql-service
DB_TYPE = mysql
MAX_CONNECTIONS = 20
```

---

# 4. Why Secrets Exist?

Consider these values:

```
DB_USERNAME
DB_PASSWORD
API_KEYS
JWT_SECRET
TLS_CERTIFICATES
```

These are confidential.

Storing them in ConfigMaps is dangerous.

---

## Problem

Everything stored in ConfigMaps is stored inside **etcd**.

```
ConfigMap
      │
      ▼
API Server
      │
      ▼
etcd
```

If an attacker gains access to etcd,

they can directly read:

```
DB_PASSWORD
API Keys
Database Credentials
```

---

## Kubernetes Solution

Store confidential information inside **Secrets**.

```
Secret
     │
     ▼
API Server
     │
Encrypt Data
     │
     ▼
etcd
```

Secrets are encrypted before being stored.

---

# 5. ConfigMap vs Secret

| Feature        | ConfigMap          | Secret            |
| -------------- | ------------------ | ----------------- |
| Stores         | Non-sensitive data | Sensitive data    |
| Encryption     | No                 | Yes               |
| Stored in etcd | Plain text         | Encrypted         |
| Examples       | Port, URLs         | Passwords, Tokens |
| Security       | Low                | High              |

---

# Interview Definition

> ConfigMaps store application configuration, whereas Secrets store confidential information. Secrets are encrypted before being stored in etcd and should be protected further using RBAC.

---

# 6. Kubernetes Architecture

```
          User

            │

 kubectl apply

            │

            ▼

      API Server

            │

            ▼

         etcd Storage

            ▲

            │

         Kubernetes

            │

            ▼

           Pod
```

---

# ConfigMap Workflow

```
Developer

    │

Writes YAML

    │

kubectl apply

    │

API Server

    │

Stores ConfigMap

    │

Pod Reads ConfigMap
```

---

# Secret Workflow

```
Developer

     │

Creates Secret

     │

API Server

     │

Encrypt Data

     │

Store inside etcd

     │

Pod Uses Secret
```

---

# 7. Creating a ConfigMap

Example YAML

```yaml
apiVersion: v1
kind: ConfigMap

metadata:
  name: test-cm

data:
  DB-Port: "3306"
```

Apply:

```bash
kubectl apply -f cm.yaml
```

Verify:

```bash
kubectl get configmap
```

Describe:

```bash
kubectl describe configmap test-cm
```

---

# 8. Using ConfigMap Inside a Pod

Two approaches:

```
             ConfigMap

          /             \

Environment Variables   Volume Mount

```

---

# Method 1

## Environment Variables

Deployment YAML

```yaml
env:
  - name: DB_PORT
    valueFrom:
      configMapKeyRef:
        name: test-cm
        key: DB-Port
```

---

Inside container

```bash
env | grep DB
```

Output

```
DB_PORT=3306
```

Application can read:

Python

```python
os.environ["DB_PORT"]
```

Java

```java
System.getenv("DB_PORT")
```

NodeJS

```javascript
process.env.DB_PORT
```

---

# 9. Limitation of Environment Variables

Suppose ConfigMap changes.

```
3306

↓

3307
```

The Pod still has

```
3306
```

because environment variables are loaded only when the container starts.

Updating ConfigMap **does not update environment variables**.

---

## Result

```
ConfigMap Updated

       │

Environment Variables

       │

NO UPDATE
```

Pods must be restarted.

---

# 10. Better Approach

## Mount ConfigMap as Volume

```
ConfigMap

      │

Mounted as File

      │

Pod File System

      │

Application Reads File
```

---

Deployment

```yaml
volumes:
  - name: db-connection
    configMap:
      name: test-cm

volumeMounts:
  - name: db-connection
    mountPath: /opt
```

---

Inside Pod

```
/opt

   │

   └── DB-Port
```

Read

```bash
cat /opt/DB-Port
```

Output

```
3306
```

---

# Amazing Feature

If ConfigMap changes

```
3306

↓

3309
```

The mounted file updates automatically.

No Pod restart required.

---

## Kubernetes Synchronization

```
ConfigMap Updated

        │

Kubelet Detects Change

        │

Mounted File Updated

        │

Application Reads Latest Value
```

---

This makes Volume Mounts the preferred approach when configuration changes frequently.

---

# 11. Live Demonstration Summary

### Step 1

Create ConfigMap

```bash
kubectl apply -f cm.yaml
```

---

### Step 2

Deploy Application

```bash
kubectl apply -f deployment.yaml
```

---

### Step 3

Login into Pod

```bash
kubectl exec -it POD_NAME -- /bin/bash
```

---

### Step 4

Read Environment Variable

```bash
env | grep DB
```

---

### Step 5

Mount ConfigMap

```yaml
volumeMounts:
```

---

### Step 6

Read Mounted File

```bash
cat /opt/DB-Port
```

---

### Step 7

Update ConfigMap

```yaml
3306

↓

3309
```

Apply

```bash
kubectl apply -f cm.yaml
```

---

### Step 8

Check Mounted File Again

```bash
cat /opt/DB-Port
```

Output

```
3309
```

No Pod restart.

---

# 12. Kubernetes Secrets

Secrets work exactly like ConfigMaps.

Difference:

Instead of plain configuration,

they store confidential data.

Examples

```
Passwords

API Tokens

JWT Keys

SSH Keys

TLS Certificates

OAuth Credentials
```

---

# Secret Creation

Command

```bash
kubectl create secret generic test-secret \
--from-literal=DB_PASSWORD=MyPassword123
```

---

View Secret

```bash
kubectl describe secret test-secret
```

---

Edit Secret

```bash
kubectl edit secret test-secret
```

Output

```
REJfUEFTU1dPUkQ=
```

Notice:

The value appears encoded.

---

# Base64 Decode

```bash
echo REJfUEFTU1dPUkQ= | base64 --decode
```

Output

```
DB_PASSWORD
```

---

# Important Note

Base64 **is not encryption**.

It is only encoding.

Production clusters should use:

* Encryption at Rest
* External Secret Management

---

# Popular Secret Managers

```
HashiCorp Vault

Bitnami Sealed Secrets

AWS Secrets Manager

Azure Key Vault

Google Secret Manager
```

---

# 13. Types of Kubernetes Secrets

| Type                  | Purpose                   |
| --------------------- | ------------------------- |
| Generic               | Username, Password        |
| TLS                   | SSL Certificates          |
| Docker Registry       | Docker Login Credentials  |
| Service Account Token | Kubernetes Authentication |

---

# 14. Security Best Practices

## Never

❌ Store passwords inside ConfigMaps.

---

## Always

✅ Store passwords inside Secrets.

---

## Enable

* Encryption at Rest
* Strong RBAC
* Least Privilege Principle

---

## RBAC Example

```
Developer

✔ Pods

✔ Deployments

✔ ConfigMaps

❌ Secrets
```

Only DevOps or administrators should access Secrets.

---

# 15. ConfigMap vs Secret Flowchart

```
Need to Store Configuration?

            │

            ▼

Is it Sensitive?

      ┌───────────────┐

      │               │

     NO              YES

      │               │

      ▼               ▼

 ConfigMap         Secret

      │               │

 Plain Storage   Encrypted Storage
```

---

# 16. Interview Questions

### Q1. What is ConfigMap?

Stores application configuration separately from application code.

---

### Q2. What is Secret?

Stores sensitive information securely in Kubernetes.

---

### Q3. Difference between ConfigMap and Secret?

| ConfigMap     | Secret            |
| ------------- | ----------------- |
| Non-sensitive | Sensitive         |
| Plain Storage | Encrypted Storage |

---

### Q4. Two ways to consume ConfigMaps?

* Environment Variables
* Volume Mounts

---

### Q5. Which approach is preferred?

Volume Mounts.

Reason:

Updates automatically without restarting Pods.

---

### Q6. Why not store passwords inside ConfigMaps?

Because ConfigMaps are stored in plain form and expose sensitive information.

---

### Q7. What is RBAC?

Role-Based Access Control used to restrict access to Kubernetes resources like Secrets.

---

### Q8. Is Base64 Encryption?

No.

It is only encoding.

---

### Q9. Why use HashiCorp Vault?

To securely manage and rotate secrets with strong encryption and centralized access control.

---

# 17. Key Takeaways

✅ ConfigMaps store non-sensitive configuration.

✅ Secrets store confidential information.

✅ Secrets are encrypted before storage in etcd (with encryption-at-rest support).

✅ ConfigMaps can be consumed using:

* Environment Variables
* Volume Mounts

✅ Environment Variables do **not** update dynamically.

✅ Volume-mounted ConfigMaps update automatically after a short synchronization delay.

✅ Use Secrets for passwords, API keys, certificates, and tokens.

✅ Protect Secrets using RBAC and the Principle of Least Privilege.

✅ For production environments, use dedicated secret management solutions such as HashiCorp Vault or cloud-native secret managers.

---

# 🎯 Memory Trick

```
               Kubernetes Configuration

                     │

          ┌──────────┴──────────┐

          │                     │

     ConfigMap              Secret

          │                     │

 Non-sensitive Data     Sensitive Data

          │                     │

   Plain Configuration    Encrypted Storage

          │                     │

 Environment / Files     Environment / Files

          │                     │

 Frequently Updated      Passwords, Tokens,
 Configurations          Certificates, Keys
```

---

# ⭐ Final Summary

* **ConfigMap** separates application configuration from code, making deployments flexible and environment-independent.
* **Secrets** provide a secure mechanism to manage confidential information, with encryption support and RBAC protection.
* **Volume-mounted ConfigMaps** are generally preferred over environment variables when configurations change frequently because updates propagate automatically without restarting Pods.
* Following these practices leads to **secure, maintainable, and production-ready Kubernetes deployments**.
