# Day 12 – Python for DevOps: File Operations in Python

## Updating Server Configuration Files Using Python

> **Learning Goal:** Understand Python File Operations and build a practical DevOps automation script that updates server configuration files automatically when certain thresholds are reached.

---

# 📖 Introduction

File Operations are one of the most important concepts in Python, especially for **DevOps Engineers**.

In real-world DevOps environments, automation frequently involves:

* Reading configuration files
* Updating application properties
* Modifying server settings
* Managing logs
* Creating and deleting files

Python provides a platform-independent and efficient way to perform these operations.

---

# 🎯 Why File Operations Matter in DevOps

## Real-World Use Cases

### 1. Automatic Server Configuration Updates

When a monitoring system detects that a server has reached a specific threshold (e.g., maximum connections), a Python script can automatically update the server configuration.

```text
Alert Triggered
       ↓
Python Script Runs
       ↓
Configuration File Updated
       ↓
Server Capacity Increased
```

---

### 2. Application Property Management

Suppose an application has a properties file:

```properties
db.timeout=30
db.pool.size=100
```

Python can automatically update these values based on operational requirements.

---

### 3. Log File Processing

Python can:

* Read logs
* Search for errors
* Generate reports
* Trigger alerts

---

# 🤔 Why Use Python Instead of Shell Scripts?

Many DevOps engineers ask:

> "Can't I do all of this using Shell Scripting?"

Yes, you can.

However, Python provides a significant advantage.

---

## Shell Script Limitation

### Linux Environment

```text
Linux Servers
      ↓
Shell Script
      ↓
Works Fine
```

### Windows Environment

```text
Windows Servers
       ↓
PowerShell Script
       ↓
Need Separate Maintenance
```

You end up maintaining:

```text
Linux Automation  → Shell Script
Windows Automation → PowerShell Script
```

---

## Python Advantage

```text
                Python
                   │
       ┌───────────┴───────────┐
       │                       │
   Linux Servers         Windows Servers
```

One script works across multiple platforms.

### Benefits

✅ Platform Independent

✅ Easy Maintenance

✅ Better Automation

✅ Rich Libraries

✅ Industry Standard for DevOps

---

# 📂 What Are File Operations?

File Operations allow Python programs to interact with files.

## Common Operations

```text
                 File Operations
                        │
        ┌───────────────┼───────────────┐
        │               │               │
      Read            Write          Delete
        │               │               │
   View Content    Modify Content   Remove File
```

---

# 🔥 Most Common File Operations

## 1. Read Operation

Used when you want to:

* View file contents
* Analyze data
* Extract information

### Workflow

```text
Open File
    ↓
Read Data
    ↓
Store in Variable
    ↓
Process Data
```

---

## 2. Write Operation

Used when you want to:

* Modify existing content
* Add new content
* Replace values

### Workflow

```text
Open File
    ↓
Modify Content
    ↓
Save Changes
```

---

# 📘 Python's Built-in `open()` Function

Python provides a built-in function called:

```python
open()
```

No installation is required.

---

## Syntax

```python
with open(file_path, mode) as file:
    # Perform operations
```

---

## Parameters

| Parameter | Description        |
| --------- | ------------------ |
| file_path | Path of the file   |
| mode      | Read or Write mode |

---

# 📖 Read Mode

Use:

```python
"r"
```

### Example

```python
with open("/opt/config.txt", "r") as file:
    data = file.readlines()
```

---

# ✍️ Write Mode

Use:

```python
"w"
```

### Example

```python
with open("/opt/config.txt", "w") as file:
    file.write("Updated Content")
```

---

# 🔄 Understanding `with`

The `with` statement automatically handles file closing.

Without `with`:

```python
file = open("test.txt", "r")

# operations

file.close()
```

With `with`:

```python
with open("test.txt", "r") as file:
    # operations
```

Python automatically closes the file.

✅ Cleaner

✅ Safer

✅ Recommended

---

# 🚀 DevOps Practical Scenario

## Problem Statement

You are given a task:

> Update a server configuration file whenever the maximum number of connections reaches a threshold.

---

## Existing Configuration

### `server.config`

```properties
port=8080
max_connections=200
timeout=30
ssl=true
```

---

## Requirement

When:

```text
max_connections = 200
```

Update it automatically to:

```text
max_connections = 500
```

---

# 📊 Solution Workflow

```text
             Read Config File
                     │
                     ▼
          Store All Lines in Memory
                     │
                     ▼
             Open File Again
               In Write Mode
                     │
                     ▼
        Check Each Line One-by-One
                     │
          ┌──────────┴─────────┐
          │                    │
          ▼                    ▼
     Key Found?             Not Found?
          │                    │
          ▼                    ▼
   Update Value         Write Same Line
          │                    │
          └──────────┬─────────┘
                     ▼
            Save Updated File
```

---

# 🧠 Algorithm

## Step 1

Read the file.

```text
server.config
      ↓
Read all lines
      ↓
Store in variable
```

---

## Step 2

Open the file again in write mode.

---

## Step 3

Iterate through every line.

```python
for line in lines:
```

---

## Step 4

Check whether the target key exists.

```python
if key in line:
```

---

## Step 5

Update the value.

---

## Step 6

Write unchanged lines back.

---

# 💻 Complete Python Program

```python
def update_server_config(file_path, key, value):

    # Read all lines
    with open(file_path, "r") as file:
        lines = file.readlines()

    # Open file in write mode
    with open(file_path, "w") as file:

        for line in lines:

            # Update target property
            if key in line:
                file.write(f"{key}={value}\n")

            # Keep other lines unchanged
            else:
                file.write(line)


# Function Call
update_server_config(
    "server.config",
    "max_connections",
    "1000"
)
```

---

# 🔍 Code Breakdown

## Function Definition

```python
def update_server_config(file_path, key, value):
```

### Inputs

| Parameter | Purpose              |
| --------- | -------------------- |
| file_path | Config file location |
| key       | Property to update   |
| value     | New value            |

---

## Reading the File

```python
with open(file_path, "r") as file:
    lines = file.readlines()
```

### What happens?

```text
server.config
      ↓
readlines()
      ↓
List of Lines
      ↓
Stored in Variable
```

Example:

```python
[
 "port=8080\n",
 "max_connections=200\n",
 "timeout=30\n"
]
```

---

## Writing the File

```python
with open(file_path, "w") as file:
```

This opens the file in write mode.

---

## Loop Through Lines

```python
for line in lines:
```

Python checks every line individually.

---

## Condition Check

```python
if key in line:
```

Example:

```python
key = "max_connections"
```

Python checks:

```text
Is max_connections present?
```

---

## Update Line

```python
file.write(f"{key}={value}\n")
```

Result:

```text
max_connections=1000
```

---

## Preserve Other Lines

```python
else:
    file.write(line)
```

All other settings remain unchanged.

---

# 📈 Before vs After

## Before Execution

```properties
port=8080
max_connections=200
timeout=30
ssl=true
```

---

## After Execution

```properties
port=8080
max_connections=1000
timeout=30
ssl=true
```

Only the targeted property changes.

---

# 🎯 Key Functions Learned

| Function    | Purpose                             |
| ----------- | ----------------------------------- |
| open()      | Open a file                         |
| readlines() | Read all lines into a list          |
| write()     | Write data into file                |
| with        | Automatically manage file resources |

---

# 🏆 DevOps Perspective

This simple file operation forms the foundation for many advanced DevOps automations:

### Infrastructure Automation

```text
Monitoring Alert
       ↓
Python Script
       ↓
Update Config
       ↓
Restart Service
```

---

### Auto Scaling

```text
Traffic Increase
       ↓
Threshold Reached
       ↓
Update Configuration
       ↓
Increase Capacity
```

---

### Configuration Management

```text
Configuration File
       ↓
Python Automation
       ↓
Consistent Environment
```

---

# 📝 Summary

## What We Learned Today

### File Operations

* Reading files
* Writing files
* Updating configuration files

### Python Concepts Used

* Functions
* File Handling
* Loops
* Conditional Statements (`if-else`)
* Built-in Functions

### Important Functions

```python
open()
readlines()
write()
```

### Important Modes

```python
"r"  # Read Mode
"w"  # Write Mode
```

---

# 🌟 Key Takeaway

> File Operations are a core DevOps automation skill. By combining Python file handling with loops, functions, and conditional statements, you can automatically manage server configurations, application settings, and infrastructure behavior across Linux and Windows environments using a single, maintainable script.
