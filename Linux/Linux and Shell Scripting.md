# Day 6 Notes — Linux Operating System & Shell Scripting

## DevOps Zero to Hero

> Instructor: Abhishek
> Topic: Linux OS Basics + Shell Scripting Fundamentals
> Focus: Understanding Linux, Operating Systems, and Essential Shell Commands for DevOps

---

# Table of Contents

1. Introduction to Operating Systems
2. Hardware vs Software
3. What is an Operating System?
4. Why Linux is Popular in DevOps
5. Linux Architecture
6. Understanding Shell & Shell Scripting
7. Linux File System Navigation
8. Important Linux Commands
9. File & Directory Operations
10. Monitoring CPU, Memory & Disk
11. Real-Time DevOps Relevance
12. Interview Questions
13. Summary Cheat Sheet

---

# 1. Introduction to Operating Systems

Modern computers and servers contain:

* CPU
* RAM
* Storage (Disk/SSD)
* Input/Output Devices

These are collectively called **Hardware**.

But hardware alone cannot perform tasks.
Applications like:

* Jenkins
* Python
* Java
* VS Code
* Browsers
* Games

need a platform to communicate with hardware.

That platform is called the **Operating System (OS)**.

---

# 2. Hardware vs Software

## Basic Computer Structure

```text
+----------------------+
|      USER            |
+----------------------+
           |
           v
+----------------------+
|   APPLICATIONS       |
| Jenkins, Python etc. |
+----------------------+
           |
           v
+----------------------+
| OPERATING SYSTEM     |
| Linux / Windows      |
+----------------------+
           |
           v
+----------------------+
| HARDWARE             |
| CPU | RAM | Disk     |
+----------------------+
```

---

# 3. What is an Operating System?

## Definition

An **Operating System** acts as a **bridge between software and hardware**.

It enables:

* Applications to use hardware resources
* Users to interact with computers
* Resource management
* Process execution

---

## Real-Life Example

Suppose you buy a laptop.

### What do you actually get?

| Component        | Example                 |
| ---------------- | ----------------------- |
| Hardware         | CPU, RAM, Disk          |
| Operating System | Windows/Linux/macOS     |
| Applications     | Browser, VS Code, Games |

Without the OS:

* Applications cannot access CPU/RAM
* Hardware cannot understand software requests

---

## Communication Flow

```text
USER
  |
  v
APPLICATION
  |
  v
OPERATING SYSTEM
  |
  v
HARDWARE
```

Response returns in reverse order.

---

# 4. Why Linux is Popular in DevOps

Linux dominates:

* Production servers
* Cloud environments
* Containers
* DevOps pipelines
* CI/CD systems

---

# Why Companies Prefer Linux

| Feature       | Linux            | Windows               |
| ------------- | ---------------- | --------------------- |
| Cost          | Free/Open Source | Paid/Proprietary      |
| Security      | Highly secure    | More vulnerable       |
| Performance   | Fast             | Comparatively heavier |
| Stability     | Excellent        | Moderate              |
| Customization | High             | Limited               |
| Server Usage  | Extremely common | Less common           |

---

# Key Reasons

## 1. Free & Open Source

Linux is open source.

Anyone can:

* Modify it
* Distribute it
* Create distributions

---

## 2. Security

Linux is:

* More secure by default
* Less affected by viruses
* Preferred for production systems

Windows often requires:

* Antivirus
* Anti-malware tools

---

## 3. Performance

Linux is:

* Lightweight
* Fast
* Efficient under heavy workloads

This makes it ideal for:

* Amazon
* Netflix
* Cloud platforms
* Kubernetes clusters

---

# Linux Distributions

A distribution (distro) is a version of Linux.

Popular distributions:

| Distribution | Usage             |
| ------------ | ----------------- |
| Ubuntu       | Most popular      |
| CentOS       | Enterprise        |
| Red Hat      | Commercial        |
| Debian       | Stable systems    |
| Alpine       | Containers/Docker |

---

# 5. Linux Architecture

## Linux OS Architecture Diagram

```text
+------------------------------------------------+
| User Applications                              |
| Browsers, Jenkins, Python, Java                |
+------------------------------------------------+
| System Software / Compilers / User Processes   |
+------------------------------------------------+
| System Libraries                               |
| libc, GNU libraries etc.                       |
+------------------------------------------------+
| KERNEL                                         |
+------------------------------------------------+
| Hardware                                       |
| CPU | RAM | Disk | Devices                     |
+------------------------------------------------+
```

---

# Kernel — Heart of Linux

The **Kernel** is the core component of Linux.

It manages communication between:

* Hardware
* Software

---

# Responsibilities of Kernel

| Responsibility     | Description               |
| ------------------ | ------------------------- |
| Device Management  | Controls hardware devices |
| Memory Management  | RAM allocation            |
| Process Management | Handles running processes |
| System Calls       | Communication interface   |

---

## Kernel Responsibilities Diagram

```text
                +-------------+
                |   KERNEL    |
                +-------------+
                 /    |    \
                /     |     \
               v      v      v
        Device  Memory  Process
       Management Mgmt   Mgmt
```

---

# System Libraries

Libraries provide functions required by applications.

Examples:

* libc
* GNU libraries

Applications use libraries to interact with the kernel.

---

# Compilers & System Software

Linux also includes:

| Component        | Purpose           |
| ---------------- | ----------------- |
| GCC Compiler     | Compiles programs |
| Shell            | Command execution |
| System Utilities | OS management     |

---

# 6. Understanding Shell & Shell Scripting

# What is a Shell?

A **Shell** is a command-line interface used to communicate with Linux.

Instead of GUI clicks, we use commands.

---

## Shell Interaction

```text
USER
  |
  v
SHELL
  |
  v
OPERATING SYSTEM
```

---

# Why Shell is Important in DevOps

Production servers usually:

* Do NOT have GUI
* Are remotely managed
* Require command-line operations

Hence shell scripting is essential.

---

# Common Shells

| Shell | Description    |
| ----- | -------------- |
| bash  | Most popular   |
| sh    | Basic shell    |
| zsh   | Advanced shell |
| ksh   | Korn shell     |

Recommended: **bash**

---

# 7. Linux File System Navigation

# Important Commands

---

# `pwd` — Present Working Directory

Shows current directory.

```bash
pwd
```

Example output:

```bash
/home/ubuntu
```

---

# `ls` — List Files & Folders

```bash
ls
```

Lists contents of current directory.

---

# `cd` — Change Directory

```bash
cd folder_name
```

Move into a directory.

---

# Going Back

```bash
cd ..
```

Move one directory back.

---

# Multiple Directory Navigation

```bash
cd ../../
```

Move back two directories.

---

# Navigation Flow

```text
/home
   |
   +---- ubuntu
            |
            +---- projects
                    |
                    +---- scripts
```

---

# 8. Important Linux Commands

# `ls -ltr`

Detailed listing.

```bash
ls -ltr
```

Provides:

* File permissions
* Owner
* Group
* Size
* Timestamp

---

# Understanding Output

Example:

```bash
drwxr-xr-x
```

Meaning:

| Symbol | Meaning            |
| ------ | ------------------ |
| d      | Directory          |
| rwx    | Owner permissions  |
| r-x    | Group permissions  |
| r-x    | Others permissions |

---

# 9. File & Directory Operations

# Create File — `touch`

```bash
touch file.txt
```

Creates empty file.

---

# Create/Edit File — `vi`

```bash
vi test.txt
```

---

# VI Editor Workflow

## Step 1 — Open File

```bash
vi test.txt
```

## Step 2 — Insert Mode

Press:

```text
i
```

## Step 3 — Write Content

```text
Hello World
```

## Step 4 — Save & Exit

Press:

```text
Esc
```

Then:

```bash
:wq
```

---

# Read File — `cat`

```bash
cat test.txt
```

Displays file content.

---

# Create Directory — `mkdir`

```bash
mkdir project
```

---

# Remove File — `rm`

```bash
rm file.txt
```

---

# Remove Directory — `rm -r`

```bash
rm -r project
```

---

# File Operations Summary

| Command | Purpose          |
| ------- | ---------------- |
| touch   | Create file      |
| vi      | Edit file        |
| cat     | Read file        |
| mkdir   | Create directory |
| rm      | Remove file      |
| rm -r   | Remove directory |

---

# 10. Monitoring CPU, Memory & Disk

System monitoring is critical in DevOps.

---

# Memory Usage — `free -m`

```bash
free -m
```

Shows memory in MB.

---

# CPU Information — `nproc`

```bash
nproc
```

Displays number of CPUs.

---

# Disk Usage — `df -h`

```bash
df -h
```

Shows disk usage in human-readable format.

---

# System Monitoring — `top`

```bash
top
```

Displays:

* CPU usage
* Memory usage
* Running processes
* Load average

---

# Monitoring Diagram

```text
+----------------------+
|        top           |
+----------------------+
| CPU Usage            |
| Memory Usage         |
| Disk Usage           |
| Running Processes    |
+----------------------+
```

---

# Important Monitoring Commands

| Command | Purpose             |
| ------- | ------------------- |
| free -m | Memory info         |
| nproc   | CPU count           |
| df -h   | Disk usage          |
| top     | Complete monitoring |

---

# 11. Real-Time DevOps Relevance

Shell scripting is heavily used in:

* Automation
* CI/CD pipelines
* Monitoring
* Server management
* Deployment scripts
* Backup scripts
* Log analysis

---

# Example DevOps Workflow

```text
Developer Pushes Code
          |
          v
CI/CD Pipeline Triggered
          |
          v
Shell Script Executes
          |
          v
Application Deployed
          |
          v
Monitoring & Alerts
```

---

# Why Shell Scripting Matters

Without shell scripting:

* Automation becomes difficult
* Server management becomes slow
* DevOps efficiency decreases

---

# 12. Common Interview Questions

# OS & Linux

### Q1. What is an Operating System?

An OS acts as a bridge between software and hardware.

---

### Q2. Why is Linux preferred in production?

Because it is:

* Free
* Secure
* Fast
* Stable

---

### Q3. What is Kernel?

Kernel is the core component of Linux responsible for:

* Process management
* Memory management
* Device management
* System calls

---

# Shell Commands

### Q4. How do you check current directory?

```bash
pwd
```

---

### Q5. How do you list files?

```bash
ls
```

---

### Q6. How do you monitor system performance?

```bash
top
```

---

### Q7. How do you check disk usage?

```bash
df -h
```

---

### Q8. How do you check memory usage?

```bash
free -m
```

---

# 13. Linux Command Cheat Sheet

# Navigation

| Command | Description       |
| ------- | ----------------- |
| pwd     | Current directory |
| ls      | List files        |
| cd      | Change directory  |

---

# File Operations

| Command | Description      |
| ------- | ---------------- |
| touch   | Create file      |
| vi      | Edit file        |
| cat     | Read file        |
| rm      | Delete file      |
| mkdir   | Create directory |

---

# Monitoring

| Command | Description     |
| ------- | --------------- |
| free -m | Memory usage    |
| nproc   | CPU count       |
| df -h   | Disk usage      |
| top     | Full monitoring |

---

# Key Takeaways

## You Learned

✅ What an Operating System is
✅ Linux fundamentals
✅ Why Linux dominates DevOps
✅ Linux architecture
✅ Kernel responsibilities
✅ Shell basics
✅ Essential Linux commands
✅ File management
✅ System monitoring
✅ Real-world DevOps relevance

---

# Final Summary Diagram

```text
                  LINUX ECOSYSTEM

+------------------------------------------------+
| USER                                           |
+------------------------------------------------+
                    |
                    v
+------------------------------------------------+
| SHELL (bash)                                   |
+------------------------------------------------+
                    |
                    v
+------------------------------------------------+
| OPERATING SYSTEM                               |
| Kernel + Libraries + System Software           |
+------------------------------------------------+
                    |
                    v
+------------------------------------------------+
| HARDWARE                                       |
| CPU | RAM | Disk                               |
+------------------------------------------------+
```

---

# Recommended Practice

Practice these commands daily:

```bash
pwd
ls
cd
mkdir
touch
vi
cat
rm
free -m
df -h
top
```

---

# Pro Tip for DevOps Engineers

> "A DevOps engineer who is strong in Linux and shell scripting becomes significantly more effective in automation, troubleshooting, and infrastructure management."

---
