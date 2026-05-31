
---

# Day 28: Docker Networking

## Bridge vs Host vs Overlay Networks

### Securing Containers with Custom Bridge Networks

---

# 📚 Learning Objectives

By the end of this lesson, you will understand:

* Why Docker Networking is required
* How containers communicate with:

  * Other containers
  * The Host Machine
* Different Docker network types:

  * Bridge Network
  * Host Network
  * Overlay Network
* Default Docker networking architecture
* Creating Custom Bridge Networks
* Container isolation using networking
* Practical Docker networking commands
* Common interview questions

---

# Why Do We Need Docker Networking?

Docker networking enables:

1. **Container-to-Container Communication**
2. **Container-to-Host Communication**
3. **External Access to Container Applications**
4. **Network Isolation and Security**

---

## Scenario 1: Containers Need Communication

Example:

```text
+------------------+       +------------------+
| Frontend         | ----> | Backend          |
| Container        |       | Container        |
+------------------+       +------------------+
```

Frontend must communicate with Backend APIs.

Without networking, communication is impossible.

---

## Scenario 2: Containers Need Isolation

Example:

```text
+------------------+       +------------------+
| Login Service    |       | Payment Service  |
| Container        |       | Container        |
+------------------+       +------------------+
```

Payment container contains:

* Credit Card Data
* Banking Information
* Financial Records

Login container users should **NOT** directly access Payment container.

Docker networking provides this logical isolation.

---

# Container Networking vs VM Networking

## Virtual Machines

Each VM contains:

```text
+----------------------+
| Application          |
+----------------------+
| Operating System     |
+----------------------+
```

Each VM gets:

* Dedicated Network Stack
* Dedicated IP
* Strong Isolation

Example:

```text
VM1 → 172.16.1.1
VM2 → 172.18.3.4
```

Different subnets provide isolation.

---

## Containers

Containers share the host OS.

```text
+---------------------+
| Container App       |
+---------------------+

+---------------------+
| Host OS             |
+---------------------+
```

Therefore networking must be managed carefully.

---

# How Does a Container Communicate with Host?

Suppose:

```text
Host IP       : 192.168.3.4
Container IP  : 172.17.0.2
```

Diagram:

```text
+--------------------+
| Host               |
| 192.168.3.4        |
+--------------------+

         ❌

+--------------------+
| Container          |
| 172.17.0.2         |
+--------------------+
```

Different subnets cannot communicate directly.

---

# Docker Solution: Bridge Network

Docker automatically creates a virtual bridge called:

```text
docker0
```

This acts like a network bridge between:

* Host
* Containers

---

## Docker Bridge Architecture

```text
               docker0
        +-------------------+
        | Virtual Bridge    |
        +-------------------+

          /            \
         /              \

+----------------+   +----------------+
| Container 1    |   | Host Machine   |
| 172.17.0.2     |   | 192.168.x.x    |
+----------------+   +----------------+
```

---

## Important Point

Without `docker0`:

```text
Container ❌ Host
```

Communication fails.

Applications inside containers become unreachable.

---

# Default Docker Network

Docker automatically creates:

```text
Bridge Network
```

when Docker is installed.

Check:

```bash
docker network ls
```

Output:

```text
NETWORK ID      NAME      DRIVER
xxxx            bridge    bridge
xxxx            host      host
xxxx            none      null
```

---

# Types of Docker Networks

---

# 1. Bridge Network

## Definition

Default Docker network.

Containers communicate through a virtual bridge called:

```text
docker0
```

---

## Architecture

```text
                 docker0
        +------------------------+

         |         |          |
         |         |          |

+----------+ +----------+ +----------+
| C1       | | C2       | | C3       |
+----------+ +----------+ +----------+
```

All containers attached to bridge can communicate.

---

## Advantages

✅ Easy setup

✅ Default network

✅ Container communication

✅ Suitable for single-host deployments

---

## Disadvantages

❌ Less secure

❌ All containers share same bridge

❌ Easier lateral movement if compromised

---

# 2. Host Network

## Definition

Container directly uses host networking.

No Docker bridge created.

---

## Architecture

```text
+--------------------------------+
| Host Network                   |
|                                |
| 192.168.x.x                    |
|                                |
|  Host Process                  |
|  Container Process             |
+--------------------------------+
```

Container shares host network stack.

---

## Benefits

✅ High performance

✅ No NAT overhead

---

## Drawbacks

❌ Least secure

❌ No network isolation

❌ Port conflicts possible

❌ Host compromise affects containers

---

## Example

```bash
docker run -d \
--name host-demo \
--network host \
nginx
```

---

# 3. Overlay Network

## Definition

Used for multi-host communication.

Mostly used in:

* Docker Swarm
* Kubernetes
* Container Orchestration Platforms

---

## Architecture

```text
      Overlay Network

+-----------+       +-----------+
| Host A    |       | Host B    |
| C1        | <---> | C2        |
+-----------+       +-----------+
```

Creates a virtual network spanning multiple hosts.

---

## Use Cases

✅ Docker Swarm

✅ Kubernetes

✅ Clustered Environments

---

# Security Problem with Default Bridge Network

Suppose:

```text
Login Container
Logout Container
Finance Container
```

All use same bridge.

---

## Default Situation

```text
                  docker0

        +----------------------+

         |        |         |

         |        |         |

+---------+ +---------+ +---------+
| Login   | | Logout  | | Finance |
+---------+ +---------+ +---------+
```

All containers can communicate.

---

## Risk

If Login container is compromised:

```text
Attacker
   ↓
Login Container
   ↓
Finance Container
```

Sensitive data may be exposed.

---

# Solution: Custom Bridge Network

Docker allows creation of custom bridge networks.

This provides logical isolation.

---

# Secure Architecture

```text
              Host

       +-------------------+
       |                   |
       +-------------------+

         /            \

        /              \

docker0          secure-network

   |                   |

   |                   |

+-------+         +-----------+
| Login |         | Finance   |
|Logout |         | Container |
+-------+         +-----------+
```

Finance container becomes isolated.

---

# Creating Custom Bridge Network

## Command

```bash
docker network create secure-network
```

By default:

```bash
docker network create
```

creates a bridge network.

---

# Verify Network

```bash
docker network ls
```

Output:

```text
bridge
host
none
secure-network
```

---

# Running Containers

## Login Container

```bash
docker run -d \
--name login \
nginx
```

Uses default bridge.

---

## Logout Container

```bash
docker run -d \
--name logout \
nginx
```

Uses default bridge.

---

## Finance Container

```bash
docker run -d \
--name finance \
--network secure-network \
nginx
```

Uses custom bridge.

---

# Inspect Container Network

## Login Container

```bash
docker inspect login
```

Output:

```text
Network:
Bridge

IP:
172.17.x.x
```

---

## Finance Container

```bash
docker inspect finance
```

Output:

```text
Network:
secure-network

IP:
172.19.x.x
```

Notice:

```text
172.17.x.x
```

vs

```text
172.19.x.x
```

Different subnets.

---

# Testing Communication

## Login → Logout

```bash
ping <logout-ip>
```

Result:

```text
✅ Success
```

Because both use default bridge.

---

## Login → Finance

```bash
ping <finance-ip>
```

Result:

```text
❌ Failure
```

Because Finance belongs to secure-network.

---

# Container Communication Matrix

| Source  | Destination | Result    |
| ------- | ----------- | --------- |
| Login   | Logout      | ✅ Allowed |
| Logout  | Login       | ✅ Allowed |
| Login   | Finance     | ❌ Blocked |
| Logout  | Finance     | ❌ Blocked |
| Finance | Login       | ❌ Blocked |

---

# Host Network Example

Run container:

```bash
docker run -d \
--name host-demo \
--network host \
nginx
```

---

## Inspect

```bash
docker inspect host-demo
```

Output:

```text
Network:
host
```

No dedicated container IP.

Container directly uses host networking.

---

# Docker Networking Summary

```text
                    Docker Networking

                           |
        ----------------------------------------
        |                  |                  |
        |                  |                  |
      Bridge            Host             Overlay
        |                  |                  |
 Single Host       Host Network      Multi Host
 Default           Shared Stack      Cluster Network
 Secure            Least Secure      Kubernetes/Swarm
 Customizable      Fastest           Distributed
```

---

# Important Commands Cheat Sheet

## List Networks

```bash
docker network ls
```

---

## Create Network

```bash
docker network create secure-network
```

---

## Remove Network

```bash
docker network rm secure-network
```

---

## Inspect Network

```bash
docker network inspect secure-network
```

---

## Run Container on Custom Network

```bash
docker run -d \
--network secure-network \
nginx
```

---

## Run Container on Host Network

```bash
docker run -d \
--network host \
nginx
```

---

## Inspect Container

```bash
docker inspect <container-name>
```

---

# Interview Questions

### Q1. What is Docker Networking?

Docker networking enables communication between:

* Containers
* Host Machine
* External Systems

while also providing network isolation.

---

### Q2. What is the default Docker network?

**Bridge Network (`docker0`)**

---

### Q3. What is `docker0`?

A virtual bridge interface created by Docker to connect containers with the host and other containers.

---

### Q4. Difference between Bridge and Host Network?

| Bridge       | Host              |
| ------------ | ----------------- |
| Isolated     | Shared            |
| Uses docker0 | Uses host network |
| More secure  | Less secure       |
| Default mode | Explicit mode     |

---

### Q5. Why use Custom Bridge Networks?

To:

* Improve security
* Isolate workloads
* Restrict container communication

---

### Q6. What is Overlay Network?

A distributed network spanning multiple hosts, commonly used in Docker Swarm and Kubernetes.

---

# Key Takeaways

✅ Docker uses **Bridge Network** by default.

✅ `docker0` is the virtual bridge connecting containers and host.

✅ Containers on the same bridge can communicate.

✅ Host networking shares the host's network stack and is less secure.

✅ Overlay networking is used in multi-host container orchestration.

✅ Custom bridge networks provide logical isolation and better security.

✅ Sensitive applications (Finance, Payments, Databases) should be placed on dedicated custom bridge networks.

---

# One-Line Exam Revision

> **Bridge Network = Default communication, Host Network = Shared host networking, Overlay Network = Multi-host networking, Custom Bridge Network = Secure container isolation.** 🚀
