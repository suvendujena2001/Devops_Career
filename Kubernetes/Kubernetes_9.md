# Kubernetes RBAC (Role-Based Access Control) — Comprehensive Notes
> **Topic:** Kubernetes RBAC + Free 30-Day OpenShift Sandbox Cluster

---

# Table of Contents

1. Introduction to Kubernetes RBAC
2. Why RBAC is Important
3. Two Major Areas of RBAC
4. Kubernetes User Management
5. Service Accounts in Kubernetes
6. RBAC Architecture
7. Roles and Role Bindings
8. Cluster Roles and Cluster Role Bindings
9. Complete RBAC Workflow
10. Real-World Organizational Example
11. OpenShift Sandbox (30-Day Free Cluster)
12. Hands-on OpenShift Setup
13. Key Takeaways
14. Interview Questions

---

# 1. Introduction to Kubernetes RBAC

## What is RBAC?

**RBAC (Role-Based Access Control)** is a Kubernetes security mechanism used to control:

* Who can access the cluster
* What actions they can perform
* Which resources they can access

### Definition

> RBAC allows permissions to be assigned based on the role of a user or application.

---

## Why is RBAC Important?

RBAC is one of the most critical security components in Kubernetes.

Although the concept is simple, improper implementation can result in:

* Security vulnerabilities
* Unauthorized access
* Resource deletion
* Cluster outages
* Difficult troubleshooting

---

### Example

Imagine a Kubernetes cluster used by multiple teams:

| Team             | Required Access       |
| ---------------- | --------------------- |
| Developers       | Deploy applications   |
| QA Engineers     | Read logs and monitor |
| DevOps Engineers | Full administration   |
| Security Team    | Audit resources       |

Without RBAC:

❌ Anyone could delete production resources.

❌ Anyone could modify critical cluster configurations.

❌ Anyone could accidentally destroy cluster components.

---

# 2. High-Level View of RBAC

```text
                 Kubernetes RBAC

          ┌─────────────────────────┐
          │    Access Management    │
          └────────────┬────────────┘
                       │
        ┌──────────────┴──────────────┐
        │                             │
        ▼                             ▼

 User Access                  Service Access
 Management                  (Applications)
```

RBAC handles:

1. **Human Users**
2. **Applications/Services**

---

# 3. Two Major Areas of RBAC

---

## A. User Management

Controls:

* Developers
* QA Engineers
* DevOps Engineers
* Administrators

### Questions RBAC Answers

Can a developer:

* Create pods? ✅
* Delete namespaces? ❌

Can a QA engineer:

* Read logs? ✅
* Modify deployments? ❌

---

## B. Service Management

Controls what applications can do inside Kubernetes.

### Example

Suppose a pod wants to:

* Read Secrets
* Access ConfigMaps
* Query Kubernetes API
* Create resources

RBAC determines whether these actions are allowed.

---

### Without RBAC

```text
Malicious Pod
      │
      ▼
Delete Resources
Access Secrets
Modify Cluster
```

---

### With RBAC

```text
Malicious Pod
      │
      ▼
Permission Denied ❌
```

---

# 4. Core RBAC Components

Kubernetes RBAC revolves around **three major components**.

```text
+----------------+
| User / Service |
+--------+-------+
         |
         ▼
+----------------+
|      Role      |
+--------+-------+
         |
         ▼
+----------------+
| Role Binding   |
+----------------+
```

---

## Component 1: Users / Service Accounts

Identity of the entity.

Examples:

* Developer
* QA Engineer
* Application Pod
* Automation Tool

---

## Component 2: Roles

Defines permissions.

Examples:

* Read Pods
* Create Deployments
* Access Secrets
* Read ConfigMaps

---

## Component 3: Role Bindings

Connects identities to permissions.

Role Binding = Glue between User and Role.

---

# 5. Kubernetes User Management

A very important interview topic.

---

## Can Kubernetes Create Users?

### Answer: NO

Kubernetes does **not** manage users directly.

Unlike Linux:

```bash
useradd developer
```

Kubernetes has no native command to create users.

---

## Kubernetes Philosophy

```text
Kubernetes
     │
     ▼
Delegates User Management
to External Identity Providers
```

---

# 6. Identity Providers (IdP)

Kubernetes relies on external authentication systems.

```text
             Kubernetes
                   │
                   ▼
       External Identity Provider
```

Examples:

| Identity Provider | Usage                     |
| ----------------- | ------------------------- |
| LDAP              | Enterprise authentication |
| Okta              | SSO                       |
| AWS IAM           | EKS clusters              |
| Keycloak          | Identity broker           |
| Active Directory  | Corporate login           |
| GitHub            | Developer authentication  |
| Google OAuth      | OAuth login               |

---

## Real-Life Example

Many applications allow:

```text
Login with Google
Login with Facebook
Login with Instagram
```

Instead of creating users internally, they trust another provider.

Kubernetes follows the same model.

---

# 7. Kubernetes API Server and Authentication

The API Server acts as the gateway.

```text
User
 │
 ▼
Identity Provider
 │
 ▼
API Server
 │
 ▼
Kubernetes Cluster
```

---

### Authentication Flow

```text
Developer
    │
    ▼
Login using IAM / LDAP
    │
    ▼
Identity Verified
    │
    ▼
API Server Receives User Info
    │
    ▼
RBAC Checks Permissions
```

---

# 8. Example: EKS + IAM

For Amazon EKS:

```text
AWS IAM User
       │
       ▼
 AWS IAM Group
       │
       ▼
 EKS Cluster
       │
       ▼
 Kubernetes RBAC
```

Benefits:

* Reuse existing AWS users
* Centralized management
* No duplicate accounts

---

# 9. Service Accounts

Unlike users, Service Accounts are created directly in Kubernetes.

---

## What is a Service Account?

A Service Account represents an application running inside Kubernetes.

```text
Application
      │
      ▼
Service Account
      │
      ▼
Kubernetes API
```

---

### Examples

* Jenkins
* ArgoCD
* Monitoring Agents
* Custom Applications

---

## Creating a Service Account

Service Accounts are created using YAML.

Example:

```yaml
apiVersion: v1
kind: ServiceAccount

metadata:
  name: my-service-account
```

---

# 10. Default Service Account

Important Interview Question.

Every namespace contains a default Service Account.

When you create a pod:

```text
Pod Created
     │
     ▼
No Service Account?
     │
     ▼
Attach Default Service Account
```

---

### Automatic Flow

```text
Pod
 │
 ▼
Default Service Account
 │
 ▼
API Communication
```

---

# 11. Roles

Roles define permissions.

Think of a Role as a permission policy.

---

## Example Role

```yaml
Can:
  - Read Pods
  - Read ConfigMaps
  - Read Secrets
```

---

### Visual Representation

```text
Role
│
├── Pods
├── ConfigMaps
├── Secrets
└── Services
```

---

## Role Purpose

A Role answers:

> What actions can be performed?

Examples:

* get
* list
* watch
* create
* update
* delete

---

# 12. Role Binding

Roles alone do nothing.

They must be assigned.

---

### Role Binding Purpose

```text
Role Binding
      │
      ▼
Attach Role to User
```

---

## Example

```text
Role:
Read Pods
Read ConfigMaps

        │
        ▼

Developer User
```

Now the developer gets those permissions.

---

# 13. RBAC Relationship Diagram

```text
             User
               │
               │
               ▼
       +---------------+
       | Role Binding  |
       +---------------+
               │
               │
               ▼
       +---------------+
       |     Role      |
       +---------------+
               │
               ▼
          Permissions
```

---

# 14. Complete RBAC Workflow

```text
STEP 1
Create User
(or Service Account)

        │
        ▼

STEP 2
Create Role

        │
        ▼

STEP 3
Create Role Binding

        │
        ▼

STEP 4
User Gets Permissions
```

---

# 15. Role vs ClusterRole

## Role

Namespace-scoped permissions.

Example:

```text
Namespace: development

Can:
- Read Pods
- Read ConfigMaps
```

Only works within that namespace.

---

## ClusterRole

Cluster-wide permissions.

Example:

```text
All Namespaces

Can:
- Read Nodes
- Read Namespaces
- Manage Cluster Resources
```

---

## Comparison

| Feature         | Role                | ClusterRole        |
| --------------- | ------------------- | ------------------ |
| Scope           | Namespace           | Entire Cluster     |
| Resource Access | Namespace resources | Cluster resources  |
| Used For        | Team-level access   | Admin-level access |

---

# 16. RoleBinding vs ClusterRoleBinding

| Feature      | RoleBinding | ClusterRoleBinding |
| ------------ | ----------- | ------------------ |
| Scope        | Namespace   | Entire Cluster     |
| Binds        | Role        | ClusterRole        |
| Access Level | Limited     | Global             |

---

# 17. Real-World RBAC Example

## Scenario

An organization has:

```text
Company Kubernetes Cluster

├── Developers
├── QA Team
├── DevOps Team
└── Security Team
```

---

### Developers

Permissions:

```text
✔ Create Deployments
✔ View Pods

✘ Delete Namespaces
✘ Manage Nodes
```

---

### QA Team

Permissions:

```text
✔ View Logs
✔ Read Pods

✘ Modify Deployments
✘ Delete Resources
```

---

### DevOps Team

Permissions:

```text
✔ Full Cluster Access
✔ Manage Nodes
✔ Manage Namespaces
✔ Cluster Administration
```

---

# 18. OpenShift Sandbox

The instructor introduced a free OpenShift learning environment.

---

## What is OpenShift Sandbox?

A free OpenShift cluster provided for learning.

Features:

* Free for 30 days
* Shared Kubernetes cluster
* Real production-like environment
* Web UI + CLI access

---

## Benefits

You can practice:

* RBAC
* Pods
* Deployments
* Services
* Ingress
* Routes
* Storage
* Volumes
* Events
* API Explorer

---

# 19. OpenShift Sandbox Architecture

```text
                    OpenShift Shared Cluster

 ┌────────────────────────────────────────────┐
 │                                            │
 │ Namespace - User A                         │
 │ Namespace - User B                         │
 │ Namespace - User C                         │
 │ Namespace - User D                         │
 │                                            │
 └────────────────────────────────────────────┘
```

Each user gets their own namespace.

---

# 20. OpenShift Login Flow

```text
Create Red Hat Account
          │
          ▼
OpenShift Sandbox
          │
          ▼
Authentication
          │
          ▼
Dedicated Namespace
          │
          ▼
Cluster Access
```

---

# 21. Using the CLI

After logging in:

```bash
oc login <token>
```

or

```bash
kubectl get pods
```

---

## Create Deployment

```bash
kubectl create deployment nginx \
--image=nginx
```

---

## Scale Deployment

```bash
kubectl scale deployment nginx \
--replicas=2
```

---

# 22. Learning Opportunities in OpenShift

You can practice:

### Workloads

* Pods
* Deployments
* ReplicaSets

### Networking

* Services
* Routes
* Ingress

### Storage

* Persistent Volumes
* PVCs

### Security

* Service Accounts
* Roles
* Role Bindings

### Monitoring

* Events
* Logs
* Metrics

---

# 23. Master Summary Diagram

```text
                    KUBERNETES RBAC

┌───────────────────────────────────────────────┐
│                                               │
│           Authentication Layer                │
│                                               │
│ LDAP / IAM / Okta / Keycloak / GitHub         │
└─────────────────┬─────────────────────────────┘
                  │
                  ▼

          User / Service Account
                  │
                  ▼

              Role
      (Defines Permissions)

                  │
                  ▼

           Role Binding
      (Attaches Permissions)

                  │
                  ▼

         Kubernetes Resources

     Pods
     ConfigMaps
     Secrets
     Services
     Deployments
     Namespaces
```

---

# Key Takeaways

✅ RBAC stands for **Role-Based Access Control**

✅ RBAC is a core Kubernetes security mechanism

✅ Kubernetes manages authorization, not user creation

✅ User management is delegated to external Identity Providers

✅ Service Accounts represent applications

✅ Roles define permissions

✅ Role Bindings assign permissions

✅ ClusterRoles provide cluster-wide access

✅ ClusterRoleBindings grant cluster-wide permissions

✅ Every pod gets a default Service Account if none is specified

✅ OpenShift Sandbox provides a free 30-day Kubernetes learning environment

---

# Frequently Asked Interview Questions

### 1. What is Kubernetes RBAC?

Role-Based Access Control used to manage permissions in Kubernetes.

---

### 2. What are the main RBAC components?

* User / Service Account
* Role / ClusterRole
* RoleBinding / ClusterRoleBinding

---

### 3. Does Kubernetes create users?

No. Kubernetes relies on external Identity Providers.

---

### 4. What is a Service Account?

An identity used by applications running inside Kubernetes.

---

### 5. Difference between Role and ClusterRole?

Role is namespace-scoped; ClusterRole is cluster-scoped.

---

### 6. Difference between RoleBinding and ClusterRoleBinding?

RoleBinding grants namespace-level permissions, while ClusterRoleBinding grants cluster-wide permissions.

---

### 7. What happens if a pod does not specify a Service Account?

Kubernetes automatically attaches the default Service Account.

---

### 8. Why is RBAC important?

It prevents unauthorized access and protects critical cluster resources.
