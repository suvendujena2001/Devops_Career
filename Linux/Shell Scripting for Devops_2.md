# Advanced Shell Scripting for DevOps — Zero to Hero (Comprehensive Notes)

# Table of Contents

1. Introduction to Advanced Shell Scripting
2. What is Bash Scripting?
3. Importance of Shell Scripting in DevOps
4. Shell Script Structure
5. Shebang (`#!`) Explained
6. Metadata & Documentation Best Practices
7. Important System Monitoring Commands
8. Building a Node Health Monitoring Script
9. Debugging Shell Scripts
10. `set -x`, `set -e`, `set -o pipefail`
11. Processes in Linux
12. `ps -ef` Command
13. Pipes (`|`) in Linux
14. `grep` Command
15. `awk` Command
16. Combining `ps`, `grep`, and `awk`
17. Linux Streams (`stdin`, `stdout`, `stderr`)
18. Important Interview Question on Pipes
19. Log Monitoring in DevOps
20. `curl` vs `wget`
21. `find` Command
22. Root User & `sudo`
23. If-Else Conditions in Shell Scripting
24. For Loops
25. Linux Signals
26. Trap Command
27. Real-World DevOps Scenarios
28. Best Practices for Shell Scripting
29. Common Interview Questions
30. Final DevOps Notes Summary

---

# 1. Introduction to Advanced Shell Scripting

Shell scripting is one of the most important skills for a DevOps Engineer.

It is used for:

* Automation
* Server administration
* Monitoring
* Deployment
* Troubleshooting
* CI/CD pipelines
* Infrastructure management

---

# 2. What is Bash Scripting?

Bash = **Bourne Again Shell**

It is the most widely used shell in Linux systems.

---

# 3. Importance of Shell Scripting in DevOps

## Why DevOps Engineers Use Shell Scripts

| Use Case        | Example                 |
| --------------- | ----------------------- |
| Monitoring      | CPU, Memory, Disk usage |
| Automation      | Deploy applications     |
| Troubleshooting | Analyze logs            |
| Maintenance     | Cleanup temp files      |
| CI/CD           | Jenkins scripts         |
| Kubernetes      | Cluster automation      |

---

# 4. Shell Script Structure

Basic Structure:

```bash
#!/bin/bash

# Author: Abhishek
# Date: 1 Dec 2023
# Version: v1
# Purpose: Node Health Monitoring Script

echo "Hello World"
```

---

# 5. Shebang (`#!`) Explained

```bash
#!/bin/bash
```

## Purpose

Defines which interpreter should execute the script.

---

## Why NOT use `#!/bin/sh`

`sh` may point to:

* bash
* dash
* other shells

This can break scripts unexpectedly.

---

## Best Practice

✅ Always specify exact interpreter:

```bash
#!/bin/bash
```

---

# 6. Metadata & Documentation Best Practices

Always add:

* Author
* Date
* Purpose
* Version
* Prerequisites

---

## Example

```bash
# Author: Abhishek
# Date: 1 Dec 2023
# Version: v1
# Purpose: Outputs node health
```

---

# 7. Important System Monitoring Commands

## Disk Space

```bash
df -h
```

### Meaning

| Option | Meaning         |
| ------ | --------------- |
| `df`   | Disk filesystem |
| `-h`   | Human readable  |

---

## Memory

```bash
free -g
```

| Option | Meaning     |
| ------ | ----------- |
| `free` | Memory info |
| `-g`   | Show in GB  |

---

## CPU Count

```bash
nproc
```

Returns number of CPUs.

---

## Process Monitoring

```bash
top
```

Shows:

* Running processes
* CPU usage
* Memory usage
* Load average

---

# 8. Building a Node Health Monitoring Script

## Basic Script

```bash
#!/bin/bash

df -h
free -g
nproc
```

---

# 9. Debugging Shell Scripts

## Problem

Without labels, output becomes confusing.

---

## Solution 1 — Use `echo`

```bash
echo "Disk Space"
df -h

echo "Memory"
free -g
```

---

# 10. `set -x`, `set -e`, `set -o pipefail`

---

# `set -x`

## Purpose

Runs script in debug mode.

```bash
set -x
```

---

## Output Example

```bash
+ df -h
+ free -g
```

---

## Flow Diagram

```text
Command Executes
       ↓
set -x intercepts
       ↓
Prints command
       ↓
Prints output
```

---

# `set -e`

## Purpose

Exit immediately if any command fails.

```bash
set -e
```

---

## Why Important?

Without `set -e`:

```text
Step 1 ❌ Failed
Step 2 ✅ Executed
Step 3 ✅ Executed
```

This creates inconsistent systems.

---

## With `set -e`

```text
Step 1 ❌ Failed
SCRIPT TERMINATES
```

---

# `set -o pipefail`

## Problem with Pipes

```bash
wrongcommand | echo "hello"
```

Without pipefail:

* script may continue

---

## Solution

```bash
set -o pipefail
```

Now:

* any failure inside pipeline fails entire script

---

# Recommended Combination

```bash
set -x
set -e
set -o pipefail
```

---

# 11. Processes in Linux

Every running application is a process.

Examples:

* Chrome
* Java App
* MySQL
* Nginx
* Docker

---

# Process Architecture

```text
Linux System
│
├── System Processes
├── User Processes
├── Background Services
└── Application Processes
```

---

# 12. `ps -ef` Command

## Purpose

Shows all running processes.

```bash
ps -ef
```

---

## Breakdown

| Option | Meaning       |
| ------ | ------------- |
| `-e`   | Every process |
| `-f`   | Full format   |

---

# 13. Pipes (`|`) in Linux

## Purpose

Send output of one command to another.

---

## Syntax

```bash
command1 | command2
```

---

## Flow Diagram

```text
Command 1 Output
        ↓
      PIPE
        ↓
Command 2 Input
```

---

# Example

```bash
ps -ef | grep amazon
```

---

# 14. `grep` Command

## Purpose

Filters matching lines.

---

## Example

```bash
grep error logfile.log
```

---

## Real DevOps Usage

| Scenario       | Command    |
| -------------- | ---------- |
| Search logs    | grep ERROR |
| Find process   | grep java  |
| Search configs | grep port  |

---

# 15. `awk` Command

## Purpose

Extract columns/fields from output.

---

# Example

```bash
ps -ef | awk '{print $2}'
```

Returns:

* Process IDs

---

# How awk Works

```text
Row:
root 123 java app

Columns:
$1    $2   $3  $4
```

---

# Common awk Usage

| Command | Meaning       |
| ------- | ------------- |
| `$1`    | First column  |
| `$2`    | Second column |
| `$NF`   | Last column   |

---

# 16. Combining `ps`, `grep`, and `awk`

## Example

```bash
ps -ef | grep amazon | awk '{print $2}'
```

---

## Execution Flow

```text
ps -ef
   ↓
grep amazon
   ↓
awk extracts PID
```

---

# 17. Linux Streams

Linux has 3 streams.

| Stream | Meaning         |
| ------ | --------------- |
| stdin  | Standard Input  |
| stdout | Standard Output |
| stderr | Standard Error  |

---

# Diagram

```text
Keyboard → stdin
Program → stdout
Errors  → stderr
```

---

# 18. Important Interview Question on Pipes

## Question

```bash
date | echo "Today"
```

Why doesn't date output appear?

---

## Answer

`echo` does not consume stdin from pipe.

Pipe works only when:

* second command reads stdin

---

# 19. Log Monitoring in DevOps

One of the most common DevOps activities.

---

# Typical Log Levels

| Level | Meaning          |
| ----- | ---------------- |
| INFO  | General info     |
| DEBUG | Debugging        |
| WARN  | Warning          |
| ERROR | Failures         |
| TRACE | Detailed tracing |

---

# Log Analysis Flow

```text
Application
    ↓
Generates Logs
    ↓
Stored in:
- S3
- Blob Storage
- Servers
    ↓
DevOps analyzes logs
```

---

# Search Errors

```bash
cat logfile.log | grep ERROR
```

---

# 20. `curl` vs `wget`

---

# curl

## Purpose

Retrieve data from internet/APIs.

```bash
curl https://example.com
```

---

## Uses

* API calls
* Download content
* Access logs
* Testing endpoints

---

# wget

## Purpose

Download files.

```bash
wget https://example.com/file.zip
```

---

# Difference Table

| Feature         | curl     | wget |
| --------------- | -------- | ---- |
| Displays output | ✅        | ❌    |
| Downloads file  | Optional | ✅    |
| API testing     | ✅        | ❌    |
| Resume download | Limited  | ✅    |

---

# Architecture Diagram

```text
curl
  ↓
Gets response directly

wget
  ↓
Downloads file to disk
```

---

# 21. `find` Command

## Purpose

Search files/directories.

---

# Syntax

```bash
find / -name filename
```

---

# Example

```bash
find / -name pam.d
```

---

# Search Flow

```text
Start Directory
      ↓
Recursively Search
      ↓
Match Filename
      ↓
Return Path
```

---

# 22. Root User & `sudo`

---

# Root User

Most powerful Linux user.

⚠ Dangerous:

* Can delete entire filesystem

---

# Switch to Root

```bash
sudo su -
```

---

# Breakdown

| Command | Meaning                    |
| ------- | -------------------------- |
| sudo    | Execute as privileged user |
| su      | Switch user                |
| -       | Login shell                |

---

# 23. If-Else Conditions in Shell Scripting

---

# Syntax

```bash
if [ condition ]
then
    commands
else
    commands
fi
```

---

# Example

```bash
a=4
b=10

if [ $a -gt $b ]
then
    echo "a is greater"
else
    echo "b is greater"
fi
```

---

# Flowchart

```text
        Condition
         /   \
      True   False
       /       \
Execute A   Execute B
```

---

# Common Operators

| Operator | Meaning      |
| -------- | ------------ |
| `-gt`    | Greater than |
| `-lt`    | Less than    |
| `-eq`    | Equal        |

---

# 24. For Loops

---

# Purpose

Repeat tasks automatically.

---

# Syntax

```bash
for i in {1..10}
do
    echo $i
done
```

---

# Execution Flow

```text
Initialize variable
       ↓
Check condition
       ↓
Execute block
       ↓
Increment
       ↓
Repeat
```

---

# Real DevOps Usage

| Use Case         | Example            |
| ---------------- | ------------------ |
| Create users     | Bulk onboarding    |
| Deploy apps      | Multiple servers   |
| Restart services | Cluster management |

---

# 25. Linux Signals

Signals are notifications sent to processes.

---

# Examples

| Signal  | Purpose       |
| ------- | ------------- |
| SIGINT  | Ctrl + C      |
| SIGKILL | Force kill    |
| SIGTERM | Graceful stop |

---

# Kill Process

```bash
kill -9 PID
```

---

# Signal Flow

```text
User Action
    ↓
Kernel receives signal
    ↓
Process reacts
```

---

# 26. Trap Command

---

# Purpose

Capture/intercept signals.

---

# Syntax

```bash
trap "echo Don't press Ctrl+C" SIGINT
```

---

# Example

```bash
#!/bin/bash

trap "echo 'Interrupt blocked'" SIGINT

while true
do
    echo "Running..."
    sleep 1
done
```

---

# Trap Flow Diagram

```text
Ctrl+C Pressed
      ↓
Signal Generated
      ↓
trap intercepts
      ↓
Custom Action Executes
```

---

# Real DevOps Scenario

Suppose:

* Script inserts database records
* User presses Ctrl+C midway

Without trap:

* Partial data corruption

With trap:

* Cleanup operations execute safely

---

# Example Cleanup

```bash
trap "rm -rf /tmp/data" SIGINT
```

---

# 27. Real-World DevOps Scenarios

---

# Scenario 1 — Server Health Monitoring

```bash
#!/bin/bash

set -e
set -x
set -o pipefail

echo "Disk Usage"
df -h

echo "Memory"
free -g

echo "CPU"
nproc
```

---

# Scenario 2 — Monitor Java Process

```bash
ps -ef | grep java
```

---

# Scenario 3 — Extract PID

```bash
ps -ef | grep java | awk '{print $2}'
```

---

# Scenario 4 — Analyze Errors

```bash
curl logfile_url | grep ERROR
```

---

# 28. Best Practices for Shell Scripting

---

# Recommended Practices

| Practice          | Why Important           |
| ----------------- | ----------------------- |
| Use `#!/bin/bash` | Correct interpreter     |
| Add metadata      | Documentation           |
| Use `set -e`      | Stop on errors          |
| Use `pipefail`    | Catch pipeline failures |
| Use comments      | Readability             |
| Validate inputs   | Avoid crashes           |
| Use functions     | Modular scripts         |

---

# Recommended Script Template

```bash
#!/bin/bash

# Author:
# Date:
# Version:
# Purpose:

set -e
set -x
set -o pipefail
```

---

# 29. Common Interview Questions

---

# Q1. Difference between `curl` and `wget`

| curl              | wget          |
| ----------------- | ------------- |
| API communication | File download |
| Prints output     | Saves files   |

---

# Q2. Purpose of `set -e`

Stops script immediately on failure.

---

# Q3. Purpose of `pipefail`

Detect failures inside pipelines.

---

# Q4. Difference between `grep` and `awk`

| grep          | awk              |
| ------------- | ---------------- |
| Filters lines | Extracts columns |

---

# Q5. What is `trap`?

Captures Linux signals and executes custom actions.

---

# Q6. Purpose of `ps -ef`

Displays all processes in full format.

---

# 30. Final DevOps Notes Summary

---

# Key Concepts Learned

✅ Bash scripting fundamentals
✅ Node health scripts
✅ Debugging techniques
✅ Process monitoring
✅ grep and awk
✅ Linux pipes
✅ curl & wget
✅ Log analysis
✅ find command
✅ if-else conditions
✅ for loops
✅ Linux signals
✅ trap command
✅ DevOps real-world practices

---

# Complete DevOps Command Cheat Sheet

| Command   | Purpose            |
| --------- | ------------------ |
| `df -h`   | Disk usage         |
| `free -g` | Memory             |
| `nproc`   | CPU count          |
| `top`     | Process monitoring |
| `ps -ef`  | List processes     |
| `grep`    | Filter text        |
| `awk`     | Extract columns    |
| `curl`    | Fetch data         |
| `wget`    | Download files     |
| `find`    | Search files       |
| `kill -9` | Kill process       |
| `trap`    | Capture signals    |

---

# Final Architecture Overview

```text
                 DevOps Shell Scripting
                          │
 ┌────────────────────────┼────────────────────────┐
 │                        │                        │
Monitoring           Automation               Troubleshooting
 │                        │                        │
df/free/top          Loops/Conditions         grep/logs
 │                        │                        │
Health Scripts       Bulk Operations          Error Analysis
 │                        │                        │
Processes             CI/CD                   Incident Response
```

---

# Conclusion

Shell scripting is one of the most powerful skills for DevOps Engineers.
Mastering commands like:

* `grep`
* `awk`
* `curl`
* `find`
* `ps`
* loops
* traps

will significantly improve your:

* automation skills
* troubleshooting abilities
* infrastructure management expertise

These concepts are heavily used in:

* Linux Administration
* Kubernetes
* CI/CD
* Cloud Engineering
* Site Reliability Engineering (SRE)

---
