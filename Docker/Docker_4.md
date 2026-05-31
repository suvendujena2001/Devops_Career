# Day 26: Multi-Stage Docker Builds & Distroless Container Images

> **Goal:** Reduce Docker image size dramatically, improve security, optimize deployments, and follow production-grade containerization practices.

---

# 📖 Table of Contents

1. Introduction
2. The Problem with Traditional Docker Builds
3. What are Multi-Stage Docker Builds?
4. How Multi-Stage Builds Work
5. Real-World Example
6. Distroless Images Explained
7. Security Advantages of Distroless Images
8. Multi-Stage Build + Distroless = Production Best Practice
9. Golang Practical Example
10. Image Size Comparison
11. Interview Questions & Answers
12. Key Takeaways

---

# 1️⃣ Introduction

One of the most common production issues in Docker environments is:

* Large Docker images
* Slow deployments
* Increased attack surface
* Higher storage costs
* Longer CI/CD execution times

Docker introduced **Multi-Stage Builds** to solve these issues.

Google later popularized **Distroless Images** to further improve:

* Security
* Performance
* Image size

Together, these techniques can reduce image sizes by **hundreds of MBs** and significantly improve production reliability.

---

# 2️⃣ The Problem with Traditional Docker Builds

Consider a simple Python Calculator application.

## Typical Docker Build Process

```text
FROM Ubuntu

Install Python
Install Pip
Install Packages

COPY Source Code

Build Application

CMD Run Application
```

---

## Architecture Diagram

```text
┌───────────────────────┐
│ Ubuntu Base Image     │
├───────────────────────┤
│ Python Runtime        │
├───────────────────────┤
│ Pip                   │
├───────────────────────┤
│ Build Dependencies    │
├───────────────────────┤
│ Source Code           │
├───────────────────────┤
│ Compiled Artifacts    │
├───────────────────────┤
│ Application Runtime   │
└───────────────────────┘
```

---

## Problem

Even after the application is built:

* Source code remains
* Build tools remain
* Package managers remain
* OS utilities remain

But the application only needs:

```text
Python Runtime + Application Binary
```

Everything else becomes unnecessary baggage.

---

# 3️⃣ What are Multi-Stage Docker Builds?

Multi-stage builds divide a Dockerfile into multiple phases.

Typically:

```text
Stage 1 → Build Stage
Stage 2 → Runtime Stage
```

---

## High-Level Workflow

```text
          Build Stage
┌──────────────────────────┐
│ Ubuntu                   │
│ Install Dependencies     │
│ Build Application        │
│ Generate Binary          │
└───────────┬──────────────┘
            │ COPY
            ▼

         Runtime Stage
┌──────────────────────────┐
│ Minimal Runtime Image    │
│ Copy Binary Only         │
│ Execute Application      │
└──────────────────────────┘
```

---

# 4️⃣ Multi-Stage Build Flow

## Stage 1

Purpose:

* Install tools
* Install libraries
* Compile application

```dockerfile
FROM ubuntu AS build

RUN install dependencies

COPY source .

RUN build application
```

---

## Stage 2

Purpose:

* Run application only

```dockerfile
FROM python:3.11-slim

COPY --from=build /app/binary .

CMD ["./binary"]
```

---

## Result

Only the final stage becomes the Docker image.

```text
Final Image Contains:

✓ Runtime
✓ Binary

Removed:

✗ Build Tools
✗ Package Managers
✗ Source Code
✗ Temporary Files
```

---

# 5️⃣ Real-World Enterprise Example

Imagine a Three-Tier Application.

```text
Frontend  → React
Backend   → Spring Boot
Database  → MySQL
```

---

## Traditional Approach

```text
Ubuntu
├── Java
├── NodeJS
├── React Build Tools
├── Maven
├── MySQL Tools
├── Source Code
├── Temporary Files
└── Final Binary
```

Image size may exceed:

```text
1 GB - 1.5 GB
```

---

## Multi-Stage Approach

### Stage 1

Build Frontend

```text
React Build
```

### Stage 2

Build Backend

```text
Spring Boot Build
```

### Stage 3

Runtime

```text
OpenJDK Runtime
+
Final JAR/WAR/EAR
```

---

## Final Architecture

```text
┌──────────────────┐
│ React Build      │
└──────┬───────────┘
       │
       ▼

┌──────────────────┐
│ Spring Build     │
└──────┬───────────┘
       │
       ▼

┌──────────────────┐
│ OpenJDK Runtime  │
│ Application JAR  │
└──────────────────┘
```

---

# 6️⃣ Distroless Images Explained

## Definition

A Distroless image is:

> A minimal container image containing only the runtime required to execute an application.

---

## What is Removed?

Distroless images intentionally remove:

```text
Shell
Bash
Curl
Wget
Find
Package Managers
APT
YUM
APK
```

---

## Example

A Python Distroless Image contains:

```text
Python Runtime
```

and almost nothing else.

---

## Visualization

### Traditional Runtime Image

```text
Python Runtime
Bash
Curl
Wget
APT
Find
OS Libraries
```

---

### Distroless Runtime Image

```text
Python Runtime
```

Only what is absolutely necessary.

---

# 7️⃣ Security Advantages

One of the biggest reasons enterprises adopt Distroless images is security.

---

## Traditional Image

```text
Ubuntu
├── Bash
├── Curl
├── Wget
├── Shell Utilities
└── Package Managers
```

Attackers can potentially exploit:

* Shell access
* Package managers
* Unnecessary binaries

---

## Distroless Image

```text
Application Runtime
```

No shell.

No package manager.

No debugging utilities.

---

### Security Benefit

```text
Smaller Surface Area
       ↓
Fewer Vulnerabilities
       ↓
Safer Containers
```

---

# 8️⃣ Multi-Stage Build + Distroless

This is considered a production-grade containerization pattern.

---

## Workflow

```text
Stage 1

Ubuntu
Build Dependencies
Compile Application

        │
        ▼

Stage 2

Distroless Runtime
Copy Binary

        │
        ▼

Production Container
```

---

## Benefits

| Benefit                | Multi-Stage | Distroless |
| ---------------------- | ----------- | ---------- |
| Smaller Image          | ✅           | ✅          |
| Faster Deployment      | ✅           | ✅          |
| Better Security        | ⚠️          | ✅          |
| Less Storage           | ✅           | ✅          |
| Reduced Attack Surface | ⚠️          | ✅          |
| Production Ready       | ✅           | ✅          |

---

# 9️⃣ Practical Golang Example

The course demonstrates this using Go.

Why Go?

Because Go produces a statically linked binary.

It often requires **no runtime environment**.

---

## Dockerfile Without Multi-Stage

```dockerfile
FROM ubuntu

RUN install golang

COPY calculator.go .

RUN go build calculator.go

ENTRYPOINT ["./calculator"]
```

---

## What Gets Included?

```text
Ubuntu
+
Go Compiler
+
Source Code
+
Build Files
+
Binary
```

---

# 1️⃣0️⃣ Dockerfile Using Multi-Stage + Distroless

```dockerfile
FROM ubuntu AS build

RUN install golang

COPY calculator.go .

RUN go build calculator.go
```

---

## Final Runtime Stage

```dockerfile
FROM scratch

COPY --from=build /calculator .

ENTRYPOINT ["./calculator"]
```

---

# What is Scratch?

`scratch` is the smallest possible Docker base image.

```text
No OS
No Runtime
No Shell
Nothing
```

Only your binary.

---

## Architecture

```text
┌────────────────────────┐
│ Ubuntu Build Stage     │
│ Install Go             │
│ Compile Binary         │
└───────────┬────────────┘
            │
            ▼

┌────────────────────────┐
│ Scratch Image          │
│ Copy Binary            │
│ Run Binary             │
└────────────────────────┘
```

---

# 1️⃣1️⃣ Actual Size Reduction

## Without Multi-Stage

```text
Image Size = 861 MB
```

---

## With Multi-Stage + Scratch

```text
Image Size = 1.83 MB
```

---

## Comparison Chart

```text
861 MB
████████████████████████████████████████

1.83 MB
█
```

---

## Percentage Reduction

Approximate reduction:

```text
861 MB → 1.83 MB

Reduction ≈ 99.8%
```

or

```text
~470x smaller
```

depending on comparison method.

---

# 1️⃣2️⃣ Finding Distroless Images

Google maintains official Distroless images.

Available runtimes:

```text
Java
Python
NodeJS
Go
Static Applications
```

---

## Example

Java Distroless:

```dockerfile
FROM gcr.io/distroless/java17
```

Python Distroless:

```dockerfile
FROM gcr.io/distroless/python3
```

---

# 🎯 Interview Questions

## Q1. What is a Multi-Stage Docker Build?

**Answer:**

A Multi-Stage Docker Build allows us to separate build-time dependencies from runtime dependencies by using multiple `FROM` statements in a Dockerfile.

This significantly reduces image size and improves maintainability.

---

## Q2. Why Use Multi-Stage Builds?

**Answer:**

Benefits:

* Smaller image size
* Faster deployments
* Reduced storage usage
* Cleaner Dockerfiles
* Improved security

---

## Q3. What is a Distroless Image?

**Answer:**

A Distroless image contains only the runtime necessary to execute an application and excludes shells, package managers, and OS utilities.

---

## Q4. What Security Benefits Do Distroless Images Provide?

**Answer:**

* Smaller attack surface
* Fewer CVEs
* No shell access
* No package manager exploitation
* Reduced OS vulnerabilities

---

## Q5. Can Multi-Stage Builds Have More Than Two Stages?

**Answer:**

Yes.

A Dockerfile can contain any number of stages:

```text
Frontend Build Stage
Backend Build Stage
Testing Stage
Packaging Stage
Runtime Stage
```

Only the final stage becomes the output image.

---

# 🏆 Key Takeaways

## Multi-Stage Builds

✅ Separate build and runtime environments

✅ Remove unnecessary dependencies

✅ Reduce image size drastically

---

## Distroless Images

✅ Minimal runtime images

✅ Strong security posture

✅ Reduced attack surface

---

## Best Production Pattern

```text
Build Stage
(Ubuntu / Rich Image)

        │
        ▼

Runtime Stage
(Distroless Image)

        │
        ▼

Production Container
```

---

# Final Summary

The combination of **Multi-Stage Docker Builds** and **Distroless Images** is one of the most important modern container optimization techniques.

It helps organizations:

* Reduce image sizes dramatically
* Improve CI/CD speed
* Enhance container security
* Lower infrastructure costs
* Follow cloud-native and Kubernetes best practices

> **Golden Rule:** Build with a rich image, run with a minimal image.
