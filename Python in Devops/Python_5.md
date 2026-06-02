# 🚀 Day 5: Command Line Arguments & Environment Variables in Python

> **Python for DevOps Series – Day 5**
>
> Topics Covered:
>
> * Command Line Arguments
> * Environment Variables (Env Vars)
> * `sys` Module
> * `os` Module
> * Real-world DevOps Use Cases
> * Secure Handling of Sensitive Information

---

# 📚 Quick Recap of Day 4

Before learning today's concepts, let's quickly revise what was covered previously:

## Functions

Functions help us:

* Reuse code
* Improve readability
* Reduce duplication

Example:

```python
def add(a, b):
    return a + b
```

---

## Modules

Modules are Python files containing reusable code.

Examples:

```python
import math
import random
```

---

## Packages

A package is a collection of modules.

Example:

```bash
pip install requests
```

---

## Virtual Environments

Used to isolate project dependencies.

```bash
python -m venv myenv
source myenv/bin/activate
```

---

# 🎯 Why Do We Need Command Line Arguments?

Consider the calculator program:

```python
print(add(5, 10))
```

### Problem

The values are **hardcoded**.

Every time a user wants different numbers:

1. Open source code
2. Modify values
3. Save file
4. Run again

This is impractical.

---

# ❌ Hardcoded Approach

```python
print(add(5, 10))
```

### User Flow

```text
User
  ↓
Open Source Code
  ↓
Modify Values
  ↓
Run Program
```

Not scalable.

---

# ✅ Better Approach: Command Line Arguments

Instead of modifying source code:

```bash
python calculator.py 2 add 3
```

Now the user provides inputs while executing the program.

---

# 📌 What Are Command Line Arguments?

Command Line Arguments are values passed to a program during execution.

---

## Example

```bash
python calculator.py 2 add 3
```

Breakdown:

| Argument      | Meaning       |
| ------------- | ------------- |
| calculator.py | Program       |
| 2             | First Number  |
| add           | Operation     |
| 3             | Second Number |

---

# 🌍 Real DevOps Example

AWS CLI is a Python application.

```bash
aws s3 ls
```

Here:

```text
aws  → Python Program
s3   → Argument
ls   → Argument
```

This is a real-world example of command line arguments.

---

# 📦 Reading Command Line Arguments in Python

Python provides an inbuilt module:

```python
sys
```

No installation required.

Simply import it:

```python
import sys
```

---

# 🔍 sys.argv

`argv` means **argument vector**.

It stores all command-line arguments.

---

## Example

Command:

```bash
python calculator.py 2 add 3
```

Internally:

```python
sys.argv
```

becomes:

```python
[
  "calculator.py",
  "2",
  "add",
  "3"
]
```

---

# 📊 argv Index Mapping

```text
┌───────────────┬─────────────┐
│ Index         │ Value       │
├───────────────┼─────────────┤
│ sys.argv[0]   │ calculator.py
│ sys.argv[1]   │ 2
│ sys.argv[2]   │ add
│ sys.argv[3]   │ 3
└───────────────┴─────────────┘
```

---

# 🧩 Reading Arguments

```python
import sys

num1 = sys.argv[1]
operation = sys.argv[2]
num2 = sys.argv[3]
```

---

# ⚠ Important: Everything Is Read As String

Even if user enters:

```bash
python calculator.py 2 add 3
```

Python reads:

```python
"2"
"3"
```

as strings.

---

## Problem

```python
print("2" + "3")
```

Output:

```text
23
```

Instead of:

```text
5
```

---

# ✅ Solution: Type Conversion

Convert values into numbers.

---

## Integer

```python
num1 = int(sys.argv[1])
num2 = int(sys.argv[3])
```

---

## Float (Recommended)

```python
num1 = float(sys.argv[1])
num2 = float(sys.argv[3])
```

Allows:

```bash
python calculator.py 2.5 add 3.5
```

Output:

```text
6.0
```

---

# 🏗 Building Calculator Using CLI Arguments

## Step 1

Import module:

```python
import sys
```

---

## Step 2

Read arguments:

```python
num1 = float(sys.argv[1])
operation = sys.argv[2]
num2 = float(sys.argv[3])
```

---

## Step 3

Process operation:

```python
if operation == "add":
    output = num1 + num2
```

---

## Step 4

Print result:

```python
print(output)
```

---

# 📈 Program Flow Diagram

```text
User Executes

python calculator.py 2 add 3
              │
              ▼
       sys.argv Reads
              │
              ▼
     ["2","add","3"]
              │
              ▼
     Convert to Float
              │
              ▼
       Perform Addition
              │
              ▼
          Output = 5
```

---

# 💡 Assignment

Implement support for:

```text
add
sub
mul
```

Example:

```bash
python calculator.py 10 sub 5
```

Output:

```text
5
```

---

# 🔐 Environment Variables

Now let's move to a more important DevOps concept.

---

## What Are Environment Variables?

Environment Variables are system-level variables that store values which programs can access.

---

# Why Do We Need Them?

Consider:

```bash
python app.py mypassword
```

This exposes passwords directly.

Bad practice.

---

# Sensitive Information Examples

Never expose:

* Passwords
* API Keys
* Tokens
* Certificates
* Secrets

---

# ❌ Unsafe Method

```bash
python app.py mypassword
```

Anyone can see:

```text
mypassword
```

---

# ✅ Recommended Method

Store it in an environment variable.

```bash
export PASSWORD=abhishek
```

Then read it inside Python.

---

# 📊 Security Comparison

```text
COMMAND LINE ARGUMENTS

python app.py mypassword
                 ▲
                 │
         Visible to users
```

---

```text
ENVIRONMENT VARIABLES

export PASSWORD=abhishek

python app.py

Password hidden from execution command
```

---

# 🛠 Viewing Environment Variables

Linux:

```bash
env
```

Shows:

```text
PATH=...
HOME=...
USER=...
...
```

---

# Creating Environment Variables

```bash
export PASSWORD=abhishek
```

---

# Reading Environment Variables in Python

Python provides:

```python
os
```

module.

---

## Import Module

```python
import os
```

---

## Read Variable

```python
password = os.getenv("PASSWORD")
```

---

## Print Value

```python
print(password)
```

Output:

```text
abhishek
```

---

# 📦 Example

## Terminal

```bash
export API_TOKEN=xyz123abc
```

---

## Python

```python
import os

token = os.getenv("API_TOKEN")

print(token)
```

Output:

```text
xyz123abc
```

---

# 📊 Environment Variable Flow

```text
┌─────────────────────┐
│ Environment Variable│
│ API_TOKEN=xyz123abc │
└──────────┬──────────┘
           │
           ▼
     os.getenv()
           │
           ▼
      Python Program
           │
           ▼
      Uses Secret
```

---

# 🌍 Real DevOps Use Cases

Environment Variables are heavily used in:

### CI/CD Pipelines

* Jenkins
* GitHub Actions
* GitLab CI
* Azure DevOps

---

### Cloud Platforms

* AWS
* Azure
* GCP

---

### Containers

* Docker
* Kubernetes

---

### Applications

* Database Passwords
* API Keys
* OAuth Tokens
* SSL Certificates

---

# 🚀 CI/CD Example

## Unsafe

```yaml
script:
  - python app.py mypassword
```

Anyone reading pipeline file can see password.

---

## Secure

```yaml
script:
  - python app.py
```

Password stored as:

```bash
export PASSWORD=*****
```

Read using:

```python
os.getenv("PASSWORD")
```

---

# 🔄 Command Line Arguments vs Environment Variables

| Feature                    | Command Line Arguments | Environment Variables |
| -------------------------- | ---------------------- | --------------------- |
| User Input                 | Yes                    | No                    |
| Visible During Execution   | Yes                    | No                    |
| Good For Calculator Inputs | ✅                      | ❌                     |
| Good For Passwords         | ❌                      | ✅                     |
| Good For API Keys          | ❌                      | ✅                     |
| Good For Tokens            | ❌                      | ✅                     |
| Good For Secrets           | ❌                      | ✅                     |
| Frequently Used in DevOps  | ✅                      | ✅                     |

---

# 📝 Key Takeaways

## Command Line Arguments

* Passed while running the program.
* Read using `sys.argv`.
* Stored as strings.
* Convert to `int()` or `float()` when needed.
* Useful for dynamic user input.

Example:

```bash
python calculator.py 2 add 3
```

---

## Environment Variables

* Used for sensitive information.
* Read using `os.getenv()`.
* Common in DevOps automation.
* Prevent exposing secrets in code or pipelines.

Example:

```bash
export PASSWORD=mysecret
```

```python
import os

password = os.getenv("PASSWORD")
```

---

# 🎯 Final Rule to Remember

## Use Command Line Arguments For

✅ User Inputs

```bash
python calculator.py 2 add 3
```

---

## Use Environment Variables For

✅ Passwords
✅ API Keys
✅ Tokens
✅ Certificates
✅ Secrets

```bash
export API_TOKEN=abc123
```

```python
token = os.getenv("API_TOKEN")
```

---

# 🏆 Day 5 Summary

By the end of this lesson, you learned:

* How to pass values dynamically to Python programs.
* How to use `sys.argv`.
* Why command line arguments are important.
* How to use environment variables securely.
* How DevOps engineers protect secrets.
* Difference between CLI arguments and environment variables.
* Real-world AWS and CI/CD use cases.

These concepts form the foundation for automation, scripting, cloud operations, and DevOps workflows.
