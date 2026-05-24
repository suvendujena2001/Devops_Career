# Day 1 – Introduction to Python for DevOps Engineers

Based on: Abhishek Veeramalla

---

# 📘 Table of Contents

1. Introduction
2. Why DevOps Engineers Learn Python
3. Linux, Shell, and DevOps Connection
4. Shell Commands vs Shell Scripting
5. Shell Scripting vs Python Scripting
6. Real-Time DevOps Use Cases
7. APIs and JSON Handling
8. When to Use Shell vs Python
9. Installing Python
10. Using GitHub Codespaces
11. Running Your First Python Program
12. Important Commands
13. Key Takeaways
14. Interview Questions
15. Practice Assignment

---

# 1️⃣ Introduction

This session introduces:

* Why Python is important for DevOps Engineers
* Difference between Shell Scripting and Python
* Real-world automation examples
* Installing Python
* Running your first Python program

---

# 2️⃣ Why Python for DevOps?

DevOps engineers work heavily with:

* Infrastructure
* Automation
* Cloud systems
* Linux servers
* APIs
* Monitoring systems

Python helps automate these tasks efficiently.

---

# 🧠 Core Idea

> Shell scripting is excellent for system-level tasks.
> Python is excellent for complex automation and integrations.

---

# 3️⃣ Why DevOps Engineers Use Linux

Most production servers run Linux because:

✅ More secure
✅ Stable
✅ Lightweight
✅ Less vulnerable
✅ Preferred for cloud infrastructure

Examples:

* AWS EC2
* Azure VMs
* Google Cloud VMs

---

# 📊 Architecture Overview

```text
Developer Machine
       │
       │ SSH
       ▼
+-------------------+
| Linux Server      |
|-------------------|
| Files             |
| CPU               |
| Memory            |
| Logs              |
| Applications      |
+-------------------+
```

---

# 4️⃣ What is Shell?

A shell is a command-line interface used to interact with Linux systems.

Examples:

* Bash
* Zsh
* Sh

---

# 🔹 Shell Commands

Individual commands executed manually.

Example:

```bash
pwd
ls
df -h
free -m
mkdir test
```

---

# 🔹 Shell Scripting

A shell script is simply:

> A file containing multiple shell commands executed sequentially.

---

# 📂 Example Shell Script

```bash
#!/bin/bash

pwd
mkdir demo
df -h
free -m
```

Save as:

```bash
test.sh
```

Run using:

```bash
sh test.sh
```

---

# 📊 Shell Script Execution Flow

```text
Shell Script
    │
    ▼
Command 1 → Execute
Command 2 → Execute
Command 3 → Execute
Command 4 → Execute
```

---

# 5️⃣ Primary Purpose of Shell Scripting

Shell scripting is mainly used for:

✅ Linux administration
✅ File handling
✅ Monitoring
✅ Environment variables
✅ Log analysis
✅ Disk management

---

# 📌 Common DevOps Tasks Using Shell

| Task                      | Example              |
| ------------------------- | -------------------- |
| Check disk space          | `df -h`              |
| Check memory              | `free -m`            |
| Create directories        | `mkdir`              |
| Read logs                 | `grep ERROR app.log` |
| Set environment variables | `export ENV=prod`    |
| User management           | `useradd`            |

---

# 6️⃣ Why Python if Shell Already Exists?

Excellent question.

Shell scripting has limitations.

Python solves many advanced automation problems.

---

# ⚡ Two Main Reasons to Learn Python

## Reason 1: Cross-Platform Support

Shell scripts mainly work on Linux.

Python works on:

* Linux
* Windows
* MacOS

---

# 📊 Platform Compatibility

| Technology   | Linux | Windows | Mac |
| ------------ | ----- | ------- | --- |
| Shell Script | ✅     | ❌       | ⚠️  |
| Python       | ✅     | ✅       | ✅   |

---

# ⚠️ But This Is NOT the Main Reason

Because tools like:

* Ansible

already solve cross-platform automation.

And Ansible itself is written in Python.

---

# ⭐ REAL Main Reason

Python is better for:

✅ Complex logic
✅ API interactions
✅ JSON processing
✅ Error handling
✅ Data manipulation
✅ Automation workflows

---

# 7️⃣ Real-Time DevOps Use Case

## Scenario

You have:

* 20 GitHub repositories

Task:

* Fetch all issues
* Get issue creators
* Automate reporting

---

# ❌ Manual Process

```text
Login to GitHub
    ↓
Open Repository
    ↓
Open Issues
    ↓
Check Author
    ↓
Repeat 20 times
```

Very inefficient.

---

# ✅ Automated Process

```text
Python Script
      │
      ▼
GitHub API
      │
      ▼
JSON Response
      │
      ▼
Extract Author Names
      │
      ▼
Generate Report
```

---

# 8️⃣ API Concept Explained

API = Application Programming Interface

APIs allow applications to communicate.

Example:

```text
Python Script  ---> GitHub API
```

---

# 📌 Example GitHub API

```text
api.github.com/issues
```

---

# 9️⃣ What Happens During API Communication?

## Step-by-Step Flow

```text
1. Send Request
       │
       ▼
2. GitHub API Processes Request
       │
       ▼
3. Returns JSON Data
       │
       ▼
4. Python Processes JSON
       │
       ▼
5. Extract Required Information
```

---

# 🔟 What is JSON?

JSON = JavaScript Object Notation

Used for data exchange.

Example:

```json
{
  "issue": "Bug in login",
  "author": "john"
}
```

---

# 1️⃣1️⃣ Why Python Wins for JSON Processing

Shell scripting can process JSON using:

* `curl`
* `jq`

But Python provides:

* Rich libraries
* Simpler syntax
* Easier iteration
* Better maintainability

---

# 📊 Shell vs Python for API Work

| Feature        | Shell     | Python        |
| -------------- | --------- | ------------- |
| API calls      | `curl`    | `requests`    |
| JSON parsing   | `jq`      | `json` module |
| Readability    | Medium    | High          |
| Complex logic  | Difficult | Easy          |
| Error handling | Limited   | Excellent     |

---

# 1️⃣2️⃣ Python Modules Mentioned

| Module     | Purpose           |
| ---------- | ----------------- |
| `requests` | API communication |
| `json`     | JSON processing   |

---

# 📌 Python Advantage

Python can:

* Serialize JSON
* Convert JSON into dictionaries
* Iterate nested data easily

---

# 📊 JSON Handling Concept

```text
JSON Response
      │
      ▼
Python Dictionary
      │
      ▼
Loop Through Data
      │
      ▼
Extract Required Values
```

---

# 1️⃣3️⃣ Shell vs Python Summary

# ✅ Use Shell Scripting When

* Working with Linux systems
* Managing files/folders
* Monitoring systems
* Reading logs
* Simple automation

---

# ✅ Use Python When

* Working with APIs
* Processing JSON
* Writing complex automation
* Building integrations
* Handling errors
* Data processing

---

# 📊 Final Comparison Table

| Category        | Shell Script      | Python              |
| --------------- | ----------------- | ------------------- |
| Best For        | Linux admin tasks | Advanced automation |
| Complexity      | Simple            | Complex             |
| API Handling    | Harder            | Easier              |
| JSON Processing | Limited           | Excellent           |
| Cross-platform  | Weak              | Strong              |
| Readability     | Moderate          | High                |
| Error Handling  | Basic             | Advanced            |

---

# 1️⃣4️⃣ DevOps + Python Real-Time Use Cases

---

## ☁️ AWS Lambda Automation

```text
Lambda Function
      │
      ▼
Talks to AWS APIs
      │
      ▼
Performs Automation
```

Examples:

* S3 operations
* EC2 management
* CloudWatch automation

---

## 📊 Monitoring Scripts

Python can automate:

* CPU monitoring
* Memory tracking
* Slack alerts
* Email notifications

---

## 📂 Log Processing

Analyze huge log files efficiently.

---

## 🔗 Third-Party Integrations

Integrate with:

* GitHub
* Jira
* Slack
* AWS
* Azure
* Kubernetes

---

# 1️⃣5️⃣ Installing Python

---

# Option 1 — GitHub Codespaces (Recommended)

## What is Codespaces?

Cloud-based development environment provided by [GitHub](https://github.com?utm_source=chatgpt.com)

---

# ✅ Advantages

* No installation needed
* Free usage available
* Python pre-installed
* VS Code included
* Works from browser

---

# 📊 Codespaces Workflow

```text
GitHub Repository
        │
        ▼
Create Codespace
        │
        ▼
Cloud VM Starts
        │
        ▼
Python Already Installed
        │
        ▼
Start Coding
```

---

# 📌 Important Notes

* Around 60 free hours/month
* Requires GitHub account
* Good for office laptops with restrictions

---

# 1️⃣6️⃣ Verify Python Installation

Run:

```bash
python --version
```

Expected output:

```bash
Python 3.x.x
```

---

# 1️⃣7️⃣ Running First Python Program

---

# 📂 Example File

```python
print("Hello World")
```

Save as:

```text
hello.py
```

Run:

```bash
python hello.py
```

---

# 📊 Execution Flow

```text
Python File
     │
     ▼
Python Interpreter
     │
     ▼
Program Executes
     │
     ▼
Output Displayed
```

---

# 1️⃣8️⃣ Installing Python on Windows

---

# Steps

1. Open official Python website

   [Python Downloads](https://www.python.org/downloads/?utm_source=chatgpt.com)

2. Download latest version

3. Choose correct architecture

Usually:

* 64-bit installer

4. Install Python

5. Enable:

```text
Add Python to PATH
```

---

# ⚠️ Common Windows Issues

| Problem               | Solution         |
| --------------------- | ---------------- |
| Python not recognized | Add to PATH      |
| Wrong installer       | Use 64-bit       |
| No terminal           | Install Git Bash |

---

# Recommended Tools

| Tool     | Purpose         |
| -------- | --------------- |
| Git Bash | Better terminal |
| VS Code  | Code editor     |

---

# 1️⃣9️⃣ Installing Python on MacOS

Run:

```bash
brew install python
```

---

# 2️⃣0️⃣ Visual Studio Code

Recommended editor:

[Visual Studio Code](https://code.visualstudio.com?utm_source=chatgpt.com)

---

# Why VS Code?

✅ Lightweight
✅ Extensions support
✅ Integrated terminal
✅ Excellent Python support

---

# 2️⃣1️⃣ Important Commands from Session

| Command               | Purpose               |
| --------------------- | --------------------- |
| `python --version`    | Check Python version  |
| `sh test.sh`          | Run shell script      |
| `brew install python` | Install Python on Mac |
| `python hello.py`     | Run Python program    |

---

# 2️⃣2️⃣ Key Takeaways

---

# 🎯 Most Important Concepts

✅ DevOps engineers heavily use Linux
✅ Shell scripting is essential for Linux administration
✅ Python is powerful for complex automation
✅ APIs and JSON are core concepts in DevOps
✅ Python simplifies API interaction
✅ Codespaces removes installation difficulties
✅ VS Code is the preferred editor

---

# 📌 Golden Rule

## Use Shell for:

* System-level tasks

## Use Python for:

* Advanced automation and integrations

---

# 2️⃣3️⃣ Interview Questions

---

## Q1. Why is Python preferred in DevOps?

Because it simplifies:

* API interactions
* Automation
* JSON handling
* Complex scripting

---

## Q2. Difference between Shell and Python?

| Shell          | Python              |
| -------------- | ------------------- |
| System tasks   | Advanced automation |
| Linux-focused  | Cross-platform      |
| Simple scripts | Complex programs    |

---

## Q3. What is JSON?

A lightweight data exchange format commonly used in APIs.

---

## Q4. What is an API?

A mechanism allowing applications to communicate.

---

# 2️⃣4️⃣ Practice Assignment

---

# ✅ Tasks

1. Install Python
2. Verify installation
3. Create:

   * Shell script
   * Python script
4. Run:

   * `hello.sh`
   * `hello.py`
5. Explore GitHub Codespaces

---

# 📂 Sample Python Program

```python
print("Hello DevOps Engineers!")
```

---

# 📂 Sample Shell Script

```bash
#!/bin/bash

echo "Hello DevOps Engineers!"
```

---

# 2️⃣5️⃣ Final Learning Mindmap

```text
                 DevOps Automation
                         │
        ┌────────────────┴────────────────┐
        │                                 │
   Shell Scripting                    Python
        │                                 │
 Linux Tasks                      Advanced Logic
 System Admin                     APIs
 Monitoring                        JSON
 Logs                              Automation
 Files                             Cloud Integrations
```

---

# 🚀 End of Day 1 Notes

Next Learning Topics Likely Include:

* Variables
* Data Types
* Operators
* Python Basics
* Loops
* Functions

---

# ⭐ Recommended Action

Practice immediately:

```bash
python hello.py
```

Consistency matters more than speed.
