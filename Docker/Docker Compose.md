# Docker Compose: Complete Beginner-to-Intermediate Notes

## Managing Multi-Container Applications the Easy Way

---

# Table of Contents

1. Introduction to Docker
2. Why Docker Compose?
3. Docker vs Docker Compose
4. Understanding Multi-Container Applications
5. Problems with Traditional Docker Commands
6. How Docker Compose Solves These Problems
7. Docker Compose Architecture
8. Docker Compose Workflow
9. Practical Example: Nginx + Node.js + Redis
10. Docker Compose File Explained
11. Important Docker Compose Keywords
12. Docker Compose Commands
13. Docker Compose Use Cases
14. Docker Compose vs Kubernetes
15. Best Practices
16. Key Takeaways

---

# 1. Introduction to Docker

## What is Docker?

Docker is a containerization platform that helps manage the lifecycle of containers.

Using Docker, we can:

* Build container images
* Run containers
* Stop containers
* Remove containers
* Monitor containers
* Manage container networking
* Manage container storage

---

## Docker Lifecycle

```text
Application Code
       │
       ▼
 ┌─────────────┐
 │ Dockerfile  │
 └──────┬──────┘
        │
        ▼
 ┌─────────────┐
 │Docker Build │
 └──────┬──────┘
        │
        ▼
 ┌─────────────┐
 │ Docker Image│
 └──────┬──────┘
        │
        ▼
 ┌─────────────┐
 │ Docker Run  │
 └──────┬──────┘
        │
        ▼
 ┌─────────────┐
 │ Container   │
 └─────────────┘
```

---

## Example: Single Application

Suppose a developer creates a simple Python Flask calculator application.

### Docker Steps

### Step 1: Create Dockerfile

```dockerfile
FROM python:3.11

COPY . .

RUN pip install -r requirements.txt

CMD ["python","app.py"]
```

### Step 2: Build Image

```bash
docker build -t calculator .
```

### Step 3: Run Container

```bash
docker run -p 5000:5000 calculator
```

This works perfectly for **single-container applications**.

---

# 2. Why Docker Compose?

As applications grow, they rarely consist of a single service.

Modern applications contain:

* Frontend
* Backend APIs
* Databases
* Cache Servers
* Message Queues
* Authentication Services

Managing each container individually becomes difficult.

This is where Docker Compose comes in.

---

# 3. Docker vs Docker Compose

| Feature             | Docker                       | Docker Compose             |
| ------------------- | ---------------------------- | -------------------------- |
| Purpose             | Manage individual containers | Manage multiple containers |
| Configuration       | Commands                     | YAML File                  |
| Dependency Handling | Manual                       | Automatic                  |
| Startup Order       | Manual                       | Automatic                  |
| Multi-Service Apps  | Difficult                    | Easy                       |
| Developer Friendly  | Moderate                     | Excellent                  |

---

## Visual Comparison

### Using Docker

```text
docker build service1
docker build service2
docker build service3

docker run service1
docker run service2
docker run service3

Manage dependencies manually
```

### Using Docker Compose

```text
docker compose up
```

Everything starts automatically.

---

# 4. Understanding Multi-Container Applications

Consider an E-Commerce Platform.

```text
                     E-Commerce Application

 ┌──────────┐
 │ Frontend │
 └────┬─────┘
      │
      ▼

 ┌──────────┐
 │ Login    │
 └────┬─────┘
      │

 ┌──────────┐
 │ Catalog  │
 └────┬─────┘
      │

 ┌──────────┐
 │ Payments │
 └────┬─────┘
      │

 ┌──────────┐
 │ Orders   │
 └────┬─────┘
      │

 ┌──────────┐
 │ MySQL DB │
 └──────────┘

 ┌──────────┐
 │ Redis    │
 └──────────┘
```

Total Services:

* Frontend
* Login
* Catalog
* Orders
* Payments
* Database
* Redis

All containers must start in the correct order.

---

# 5. Problems with Traditional Docker Commands

Imagine managing 12 services manually.

## Problems

### 1. Too Many Commands

```bash
docker build service1
docker build service2
docker build service3
...
```

```bash
docker run service1
docker run service2
docker run service3
...
```

---

### 2. Dependency Management

Example:

```text
Payments Service
      │
      ▼
 Requires Database
```

If Database is unavailable:

```text
Payments Service ❌
Catalog Service ❌
Orders Service ❌
```

---

### 3. Difficult Sharing

DevOps engineer writes:

```bash
shell_script.sh
```

Developers use:

```bash
./shell_script.sh
```

Over time:

* Docker commands change
* Scripts become outdated
* Maintenance increases

---

# 6. How Docker Compose Solves These Problems

Docker Compose uses a single YAML file.

```yaml
services:
  web:
  db:
  redis:
```

Benefits:

✅ Declarative

✅ Easy to read

✅ Easy to share

✅ Handles dependencies

✅ Standardized

✅ Version controlled

---

## One Command Deployment

Start everything:

```bash
docker compose up
```

Stop everything:

```bash
docker compose down
```

---

# 7. Docker Compose Architecture

```text
                docker-compose.yml
                         │
                         ▼

        ┌─────────────────────────┐
        │ Docker Compose Engine   │
        └─────────────┬───────────┘
                      │

       ┌──────────────┼──────────────┐
       ▼              ▼              ▼

   Container 1   Container 2   Container 3
     (Web)         (DB)         (Redis)
```

---

# 8. How Docker Compose Works

Docker Compose DOES NOT replace Docker.

It works on top of Docker.

```text
Docker Compose
      │
      ▼
 Reads YAML File
      │
      ▼
 Uses Dockerfiles
      │
      ▼
 Builds Images
      │
      ▼
 Runs Containers
```

---

# 9. Practical Example

# Nginx + Node.js + Redis

---

## Architecture

```text
                    User
                     │
                     ▼

               ┌──────────┐
               │ NGINX LB │
               └────┬─────┘
                    │

         ┌──────────┴──────────┐
         ▼                     ▼

   ┌──────────┐         ┌──────────┐
   │ Node App │         │ Node App │
   │  Web-1   │         │  Web-2   │
   └────┬─────┘         └────┬─────┘
        │                    │
        └─────────┬──────────┘
                  ▼

            ┌──────────┐
            │  Redis   │
            └──────────┘
```

---

## Request Flow

```text
Request #1 → NGINX → Web-1

Request #2 → NGINX → Web-2

Request #3 → NGINX → Web-1

Request #4 → NGINX → Web-2
```

Round Robin Load Balancing

---

## Redis Role

Redis stores:

```text
Total Website Visits
```

Example:

```text
Visit 1
Visit 2
Visit 3
Visit 4
```

Shared between both Node.js instances.

---

# 10. Dockerfiles Used

---

## Node.js Dockerfile

```dockerfile
FROM node:latest

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

CMD ["npm","start"]
```

---

## Nginx Dockerfile

```dockerfile
FROM nginx

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/conf.d/default.conf
```

---

# 11. Docker Compose File Explained

## Full Compose Example

```yaml
services:

  web1:
    build: ./web
    hostname: web1
    ports:
      - "81:5000"

  web2:
    build: ./web
    hostname: web2
    ports:
      - "82:5000"

  redis:
    image: redis
    ports:
      - "6379:6379"

  nginx:
    build: ./nginx
    ports:
      - "80:80"

    depends_on:
      - web1
      - web2
```

---

# 12. Important Compose Keywords

## services

Defines all containers.

```yaml
services:
```

---

## build

Location of Dockerfile.

```yaml
build: ./web
```

---

## image

Use an existing image.

```yaml
image: redis
```

---

## ports

Maps Host Port → Container Port

```yaml
ports:
  - "81:5000"
```

Meaning:

```text
Host Port      Container Port
    81    ───►      5000
```

---

## depends_on

Controls startup order.

```yaml
depends_on:
  - web1
  - web2
```

### Diagram

```text
web1
   │
   ▼

web2
   │
   ▼

nginx
```

Nginx starts after web1 and web2.

---

## networks

Creates custom Docker networks.

```yaml
networks:
  app-network:
```

---

## volumes

Persistent storage.

```yaml
volumes:
  - db-data:/var/lib/mysql
```

---

## restart

Container restart policies.

```yaml
restart: always
```

Options:

```text
always
unless-stopped
on-failure
no
```

---

# 13. Common Docker Compose Commands

## Start Services

```bash
docker compose up
```

---

## Start in Background

```bash
docker compose up -d
```

---

## Stop Services

```bash
docker compose stop
```

---

## Remove Services

```bash
docker compose down
```

---

## View Logs

```bash
docker compose logs
```

---

## Restart Services

```bash
docker compose restart
```

---

## Check Running Containers

```bash
docker compose ps
```

---

# 14. Real-World Use Cases

## 1. Local Development

Most common use case.

Developer simply runs:

```bash
docker compose up
```

Benefits:

* Fast setup
* Consistent environments
* Easy debugging

---

## 2. CI/CD Pipelines

Example:

```text
Pull Request
      │
      ▼

 Build Containers
      │
      ▼

 docker compose up
      │
      ▼

 Run Tests
      │
      ▼

 docker compose down
```

Useful when:

* Kubernetes is unnecessary
* Temporary test environments are needed

---

## 3. QA Testing

QA teams can:

```bash
docker compose up
```

Verify features before deployment.

---

## 4. Proof of Concepts

Ideal for:

* Hackathons
* Demos
* Learning projects
* Internal tools

---

# 15. Docker Compose vs Kubernetes

This is one of the most misunderstood comparisons.

---

## Docker Compose

Purpose:

```text
Run Multiple Containers
```

Scope:

```text
Single Machine
```

Examples:

* Laptop
* VM
* Local Testing

---

## Kubernetes

Purpose:

```text
Container Orchestration
```

Scope:

```text
Multiple Machines
(Cluster)
```

---

## Kubernetes Features

* Auto-healing
* Auto-scaling
* Rolling updates
* Service discovery
* Advanced networking
* Load balancing
* High availability

---

## Visual Comparison

### Docker Compose

```text
          Single Machine

 ┌───────────────────────┐
 │ Web │ DB │ Redis │ MQ │
 └───────────────────────┘
```

---

### Kubernetes

```text
            Cluster

 ┌─────────┐
 │ Node-1  │
 └────┬────┘
      │

 ┌────▼────┐
 │ Master  │
 └────┬────┘

 ┌────▼────┐
 │ Node-2  │
 └─────────┘

 ┌─────────┐
 │ Node-3  │
 └─────────┘
```

---

## Correct Comparison

```text
Docker Compose  ❌ Kubernetes

Docker Swarm    ✅ Kubernetes
```

Reason:

Both Docker Swarm and Kubernetes are orchestration platforms.

---

# 16. Best Practices

### Keep Services Independent

Each service should perform one responsibility.

---

### Use Environment Variables

```yaml
environment:
  DB_HOST: mysql
```

---

### Use Named Volumes

```yaml
volumes:
  mysql-data:
```

---

### Define Dependencies

```yaml
depends_on:
```

Avoid startup failures.

---

### Use Version Control

Store:

```text
docker-compose.yml
Dockerfile
```

inside Git repositories.

---

# Quick Revision Sheet

## Docker

```text
Single Container Management
```

Commands:

```bash
docker build
docker run
docker stop
docker rm
```

---

## Docker Compose

```text
Multi-Container Management
```

Configuration:

```yaml
docker-compose.yml
```

Commands:

```bash
docker compose up
docker compose down
```

---

## Most Important Benefits

✅ Single command deployment

✅ Dependency management

✅ Standardized configuration

✅ Easy collaboration

✅ Developer-friendly

✅ Perfect for local development

✅ Great for CI/CD testing

---

# Final Summary

Docker Compose is a lightweight tool built on top of Docker that simplifies the management of multi-container applications through a declarative YAML configuration file. Instead of manually executing numerous `docker build` and `docker run` commands, developers and DevOps engineers can define services, networking, storage, dependencies, and startup behavior in a single file and manage the entire application lifecycle using simple commands such as:

```bash
docker compose up
docker compose down
```

It is ideal for:

* Local Development
* Testing Environments
* CI/CD Pipelines
* Learning and Prototyping

For large-scale production orchestration across multiple machines, Kubernetes (or Docker Swarm) should be considered instead.

> **Golden Rule:**
> **Docker manages containers. Docker Compose manages multiple containers. Kubernetes manages container clusters.** 🚀
