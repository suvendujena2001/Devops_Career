# 🚀 Day 25 – Docker Containerization for Django

## Deploying a Django Web Application Using Docker

> **Objective:** Learn how a DevOps Engineer containerizes a real-world Django application using Docker, understands application workflows, creates Docker images, runs containers, performs port mapping, and deploys web applications consistently across environments.

---

# 📚 Table of Contents

1. Recap of Previous Sessions
2. Why Containerize Applications?
3. DevOps Perspective on Application Containerization
4. Understanding Django Application Architecture
5. Django Project vs Django Application
6. Workflow of a Django Application
7. Containerization Strategy
8. Writing the Dockerfile
9. ENTRYPOINT vs CMD
10. Building the Docker Image
11. Running the Container
12. Port Mapping Explained
13. AWS Security Group Configuration
14. Complete End-to-End Workflow
15. DevOps Interview Notes
16. Key Takeaways

---

# 🎯 Session Objective

In previous sessions:

### Day 23

* Introduction to Containers
* VM vs Containers
* Container Architecture
* Linux Kernel Sharing

### Day 24

* Docker Fundamentals
* Docker Lifecycle
* Docker Terminologies
* Docker Installation
* Docker Images
* Docker Containers
* Docker Registries

### Day 25 (Current Session)

> Containerize and Deploy a Django Web Application using Docker

---

# Why Containerization Matters?

Before Docker:

```text
Developer Machine
      │
      ▼
Works Fine
      │
      ▼
QA Machine
      │
      ▼
Application Fails
      │
      ▼
DevOps Team Debugging
```

Common Issues:

* Different Operating Systems
* Missing Dependencies
* Different Python Versions
* Different Package Versions
* Environment Mismatch

---

## Docker Solves This Problem

```text
Docker Image

+----------------------+
| Source Code          |
| Python               |
| Django               |
| Dependencies         |
| Runtime Environment  |
+----------------------+

           │

           ▼

Runs Everywhere
```

Whether the user is on:

* Linux
* Windows
* macOS
* AWS EC2

The application behaves exactly the same.

---

# 🏗 Understanding Django Architecture

Before containerizing any application, a DevOps Engineer should understand:

> How the application works.

You don't need to become a Django developer.

But you must understand:

* Application structure
* Dependencies
* Entry points
* Runtime requirements

---

# High-Level Django Workflow

```text
User Request
      │
      ▼
URL Routing
      │
      ▼
View Function
      │
      ▼
Template Rendering
      │
      ▼
HTML Response
      │
      ▼
Browser
```

---

# Django Project Structure

When creating a Django project:

```bash
django-admin startproject devops
```

A structure similar to this is generated:

```text
devops/
│
├── manage.py
│
└── devops/
    │
    ├── settings.py
    ├── urls.py
    ├── wsgi.py
    ├── asgi.py
    └── __init__.py
```

---

# Understanding Important Files

## settings.py

Central configuration file.

Contains:

* Database Configuration
* Installed Apps
* Middleware
* Security Settings
* Allowed Hosts
* Secret Keys

```text
settings.py
│
├── Database Settings
├── Security Config
├── Installed Apps
├── Middleware
└── Templates
```

---

## urls.py

Responsible for URL Routing.

Example:

```python
path('demo/', include(...))
```

Whenever user visits:

```text
http://server-ip:8000/demo
```

Django routes the request to the Demo Application.

---

# Django Project vs Django App

This distinction is extremely important.

---

## Django Project

```text
Project
│
├── Configuration
├── Security
├── URL Routing
├── Database Settings
└── Middleware
```

Created using:

```bash
django-admin startproject devops
```

---

## Django Application

Actual business logic.

Created using:

```bash
python manage.py startapp demo
```

Example Structure:

```text
demo/
│
├── views.py
├── models.py
├── admin.py
├── tests.py
└── templates/
```

---

# Django Request Flow

```text
Browser Request
       │
       ▼

urls.py
       │
       ▼

views.py
       │
       ▼

templates/index.html
       │
       ▼

Rendered HTML
       │
       ▼

Browser
```

---

# Example View

```python
def index(request):
    return render(request, "index.html")
```

The view:

1. Receives request
2. Loads template
3. Returns HTML response

---

# 📦 Requirements File

Python applications generally have:

```text
requirements.txt
```

Example:

```text
Django
tzdata
```

Real-world applications may contain:

```text
100+
dependencies
```

---

# Why DevOps Engineers Must Understand Applications

Many beginners ask:

> Do DevOps Engineers need programming knowledge?

### Correct Answer

✅ You don't need to be an expert developer.

But you must understand:

* Application flow
* Dependencies
* Runtime requirements
* Build process
* Deployment process

---

# Docker Containerization Strategy

When given a Django application:

```text
Developer
      │
      ▼
Provides Source Code
      │
      ▼
DevOps Engineer
      │
      ▼
Creates Dockerfile
      │
      ▼
Builds Image
      │
      ▼
Runs Container
```

---

# Dockerfile for Django

```dockerfile
FROM ubuntu

WORKDIR /app

COPY requirements.txt .

COPY . .

RUN apt-get update && \
    apt-get install python3 python3-pip -y

RUN pip install -r requirements.txt

ENTRYPOINT ["python3"]

CMD ["manage.py","runserver","0.0.0.0:8000"]
```

---

# Dockerfile Breakdown

---

## Step 1: Choose Base Image

```dockerfile
FROM ubuntu
```

Base operating system for the container.

```text
Container
│
└── Ubuntu Base Image
```

---

## Step 2: Create Working Directory

```dockerfile
WORKDIR /app
```

Equivalent to:

```bash
cd /app
```

inside the container.

---

## Step 3: Copy Dependencies

```dockerfile
COPY requirements.txt .
```

Copies dependency list.

---

## Step 4: Copy Source Code

```dockerfile
COPY . .
```

Copies application source code.

---

## Step 5: Install Python

```dockerfile
RUN apt-get install python3 python3-pip
```

Installs:

* Python Runtime
* Pip Package Manager

---

## Step 6: Install Dependencies

```dockerfile
RUN pip install -r requirements.txt
```

Installs:

* Django
* tzdata
* Other project dependencies

---

# ENTRYPOINT vs CMD

One of the most important Docker interview questions.

---

## ENTRYPOINT

Defines:

> What executable must always run.

Example:

```dockerfile
ENTRYPOINT ["python3"]
```

This cannot be overridden easily.

---

## CMD

Defines:

> Default arguments passed to ENTRYPOINT.

Example:

```dockerfile
CMD ["manage.py","runserver","0.0.0.0:8000"]
```

---

# Combined Execution

Docker internally executes:

```bash
python3 manage.py runserver 0.0.0.0:8000
```

---

## Visualization

```text
ENTRYPOINT
      │
      ▼

python3

      +

CMD
      │
      ▼

manage.py runserver 0.0.0.0:8000

      │
      ▼

Final Command

python3 manage.py runserver 0.0.0.0:8000
```

---

# Why Separate ENTRYPOINT and CMD?

Suppose:

```text
Port 8000 already occupied
```

User may want:

```text
Port 8080
```

Only CMD arguments need modification.

Python executable remains unchanged.

---

# Build Docker Image

Inside project directory:

```bash
docker build -t django-web-app .
```

---

## Build Flow

```text
Dockerfile
      │
      ▼
docker build
      │
      ▼
Docker Image
```

---

# Verify Image

```bash
docker images
```

Example Output:

```text
REPOSITORY        TAG
django-web-app    latest
```

---

# Running the Container

Most beginners make a mistake here.

---

## Incorrect Command

```bash
docker run -it django-web-app
```

Container runs.

But web application isn't accessible.

Why?

---

# Networking Problem

Application runs inside container.

```text
Container

Port 8000
```

But EC2 instance doesn't know about it.

---

## Solution: Port Mapping

```bash
docker run -it -p 8000:8000 django-web-app
```

---

# Port Mapping Diagram

```text
Browser
   │
   ▼

EC2 Instance
Port 8000
   │
   ▼

Docker Container
Port 8000
```

---

# Port Mapping Syntax

```bash
-p HOST_PORT:CONTAINER_PORT
```

Example:

```bash
-p 8080:8000
```

Meaning:

```text
Host Port      : 8080
Container Port : 8000
```

---

# Complete Runtime Flow

```text
Browser
   │
   ▼

EC2 Public IP
   │
   ▼

Port 8000
   │
   ▼

Docker Host
   │
   ▼

Docker Container
   │
   ▼

Django Server
```

---

# AWS Security Group Configuration

Even after port mapping, application may not work.

Reason:

> AWS Security Group Blocking Traffic

---

## Allow Inbound Rule

Navigate:

```text
EC2
 └── Security Groups
      └── Inbound Rules
```

Add:

```text
Type      : Custom TCP
Port      : 8000
Source    : Anywhere
```

---

# AWS Networking Flow

```text
Internet
    │
    ▼

Security Group
    │
    ▼

EC2 Instance
    │
    ▼

Docker Port Mapping
    │
    ▼

Container
```

---

# End-to-End Containerization Workflow

```text
Write Django Application
            │
            ▼

requirements.txt
            │
            ▼

Create Dockerfile
            │
            ▼

docker build
            │
            ▼

Docker Image
            │
            ▼

docker run -p 8000:8000
            │
            ▼

Docker Container
            │
            ▼

Open Browser
            │
            ▼

Django Application Running
```

---

# DevOps Engineer's Responsibility

When a developer says:

> "Please containerize this application."

Your job is:

### Step 1

Understand application structure.

### Step 2

Identify dependencies.

### Step 3

Identify runtime requirements.

### Step 4

Create Dockerfile.

### Step 5

Build Docker Image.

### Step 6

Run Container.

### Step 7

Expose Ports.

### Step 8

Deploy Successfully.

---

# Common Mistakes

### ❌ Forgetting requirements.txt

Results:

```text
ModuleNotFoundError
```

---

### ❌ Forgetting pip install

Results:

```text
Django not found
```

---

### ❌ Forgetting Port Mapping

Results:

```text
Container Running
Application Not Accessible
```

---

### ❌ Forgetting Security Group Rule

Results:

```text
Connection Timed Out
```

---

### ❌ Incorrect ENTRYPOINT

Results:

```text
Container Fails To Start
```

---

# 🎤 Important Interview Questions

## Q1. Why Containerize a Django Application?

To package:

* Source Code
* Runtime
* Dependencies
* Configuration

into a portable environment.

---

## Q2. Difference Between ENTRYPOINT and CMD?

| ENTRYPOINT           | CMD                |
| -------------------- | ------------------ |
| Mandatory executable | Default arguments  |
| Harder to override   | Easy to override   |
| Defines runtime      | Defines parameters |

---

## Q3. Why Copy requirements.txt First?

To install dependencies required by the application.

---

## Q4. Why Use Port Mapping?

To expose container services to the host machine.

---

## Q5. Why Is Programming Knowledge Important for DevOps?

Because DevOps Engineers must understand:

* Application architecture
* Dependencies
* Deployment requirements
* Runtime behavior

---

# 🏆 Key Takeaways

✅ Docker removes environment inconsistencies

✅ DevOps Engineers should understand application workflows

✅ Django projects contain configuration

✅ Django apps contain business logic

✅ Dockerfile is the blueprint of containerization

✅ ENTRYPOINT defines executable

✅ CMD defines configurable arguments

✅ Port mapping is mandatory for web applications

✅ Security Groups must allow application traffic

✅ Once you can containerize one Django application, you can containerize most Django applications using the same workflow

---

# 💡 One-Line Summary

> **Containerization transforms a Django application into a portable, self-contained package that includes source code, dependencies, and runtime, enabling it to run consistently across any environment with a simple Docker build and Docker run command.** 🚀🐳
