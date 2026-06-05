# Day 7: Conditional Handling in Python (IF, ELIF, ELSE)

## 🎯 Learning Objectives

By the end of this lesson, you will understand:

* What conditional handling is
* Why conditional statements are required
* How to use:

  * `if`
  * `else`
  * `elif`
* Real-world DevOps use cases
* Best practices for writing conditional logic
* Common interview questions

---

# 📖 Recap of Previous Lessons

| Day   | Topic                                 | Key Concepts                                           |
| ----- | ------------------------------------- | ------------------------------------------------------ |
| Day 1 | Introduction to Python                | Installation, Configuration, Python vs Shell Scripting |
| Day 2 | Data Types                            | Strings, Numbers, String Functions                     |
| Day 3 | Variables & Keywords                  | Naming Conventions, Best Practices                     |
| Day 4 | Functions, Modules & Packages         | Code Reusability                                       |
| Day 5 | Environment Variables & CLI Arguments | Dynamic Inputs                                         |
| Day 6 | Operators                             | Comparison, Arithmetic, Logical Operators              |
| Day 7 | Conditional Handling                  | IF, ELSE, ELIF                                         |

---

# What is Conditional Handling?

Conditional handling allows a program to execute specific code blocks only when certain conditions are satisfied.

Instead of executing every line of code sequentially, Python evaluates a condition and decides which block should run.

---

## Real-Life Analogy

Imagine a security guard at a company entrance.

### Condition:

If the employee has an ID card:

* Allow entry

Else:

* Deny entry

```text
                Employee Arrives
                        │
                        ▼
            Has Valid ID Card?
                 /          \
               Yes           No
                │             │
                ▼             ▼
        Allow Entry      Deny Entry
```

This decision-making process is exactly what conditional handling does.

---

# Why Do We Need Conditional Handling?

Without conditions:

```python
print("Create EC2 Instance")
```

The statement always executes.

But in real-world scenarios:

* Create an EC2 instance only if it is allowed.
* Deploy code only if tests pass.
* Send alerts only if CPU usage exceeds a threshold.
* Restart a service only if it is down.

Conditional statements make programs intelligent.

---

# The IF Statement

## Syntax

```python
if condition:
    # code block
```

### Flow Diagram

```text
           Start
             │
             ▼
      Condition True?
         /      \
       Yes      No
        │        │
        ▼        ▼
 Execute Block  Skip
        │
        ▼
       End
```

---

## Example 1: Simple Condition

```python
instance_type = "t2.micro"

if instance_type == "t2.micro":
    print("We will create the instance for you.")
```

### Output

```text
We will create the instance for you.
```

---

# Reading Input from Command Line

```python
import sys

instance_type = sys.argv[1]
```

### Explanation

```text
sys.argv
│
├── argv[0] → Script Name
└── argv[1] → First User Input
```

Example:

```bash
python app.py t2.micro
```

Then:

```python
sys.argv[1]
```

contains:

```text
t2.micro
```

---

# DevOps Example Using IF

```python
import sys

instance_type = sys.argv[1]

if instance_type == "t2.micro":
    print("We will create the instance for you.")
```

### Execution

```bash
python app.py t2.micro
```

### Output

```text
We will create the instance for you.
```

---

### Execution with Different Input

```bash
python app.py t2.medium
```

### Output

```text
(No Output)
```

Reason:

The condition is not satisfied.

---

# The ELSE Statement

Sometimes we want another action when the condition fails.

For this purpose, Python provides:

```python
else
```

---

## Syntax

```python
if condition:
    # block 1
else:
    # block 2
```

---

## Flow Diagram

```text
               Condition?
               /       \
            True      False
              │          │
              ▼          ▼
        Execute IF   Execute ELSE
              │          │
              └────┬─────┘
                   ▼
                  End
```

---

## Example

```python
import sys

instance_type = sys.argv[1]

if instance_type == "t2.micro":
    print("We will create the instance for you.")
else:
    print("Instance creation is not allowed.")
```

---

### Execution

```bash
python app.py t2.medium
```

### Output

```text
Instance creation is not allowed.
```

---

# The ELIF Statement

## Problem

Suppose we have multiple instance types:

* t2.micro
* t2.medium
* t2.xlarge

Each should display different pricing.

Using only IF and ELSE is insufficient.

---

## Solution

Use:

```python
elif
```

which means:

> Else If

---

# Syntax

```python
if condition1:
    code
elif condition2:
    code
elif condition3:
    code
else:
    code
```

---

# Decision Tree

```text
                       Start
                          │
                          ▼
             instance_type == t2.micro ?
                     /            \
                  Yes              No
                   │                │
                   ▼                ▼
               $2/day      instance_type == t2.medium ?
                                     /          \
                                   Yes          No
                                    │            │
                                    ▼            ▼
                                $4/day   instance_type == t2.xlarge ?
                                                 /        \
                                               Yes        No
                                                │          │
                                                ▼          ▼
                                            $8/day    Invalid Input
```

---

# Complete Example

```python
import sys

instance_type = sys.argv[1]

if instance_type == "t2.micro":
    print("This will charge $2 per day.")

elif instance_type == "t2.medium":
    print("This will charge $4 per day.")

elif instance_type == "t2.xlarge":
    print("This will charge $8 per day.")

else:
    print("Please provide a valid instance type.")
```

---

# Program Execution Examples

---

## Case 1

```bash
python app.py t2.micro
```

Output:

```text
This will charge $2 per day.
```

---

## Case 2

```bash
python app.py t2.medium
```

Output:

```text
This will charge $4 per day.
```

---

## Case 3

```bash
python app.py t2.xlarge
```

Output:

```text
This will charge $8 per day.
```

---

## Case 4

```bash
python app.py t2.xyz
```

Output:

```text
Please provide a valid instance type.
```

---

# Understanding Execution Flow

Important Concept:

Only one matching block executes.

Example:

```python
if condition1:
    print("A")

elif condition2:
    print("B")

elif condition3:
    print("C")

else:
    print("D")
```

Python checks from top to bottom.

```text
Condition1 True?
      │
     Yes
      │
 Execute A
      │
     Stop
```

Remaining conditions are skipped.

---

# Indentation Rules

Python uses indentation to identify code blocks.

Correct:

```python
if condition:
    print("Hello")
```

Incorrect:

```python
if condition:
print("Hello")
```

### Visual Representation

```text
if condition:
│
├── statement 1
├── statement 2
├── statement 3
└── statement 4
```

All statements must have consistent indentation.

---

# DevOps Use Cases

## 1. EC2 Instance Validation

```python
if instance_type == "t2.micro":
    print("Create instance")
```

---

## 2. Deployment Validation

```python
if tests_passed:
    deploy_application()
else:
    stop_deployment()
```

---

## 3. CPU Monitoring

```python
if cpu_usage > 80:
    send_alert()
```

---

## 4. Service Monitoring

```python
if service_status == "down":
    restart_service()
```

---

## 5. Backup Verification

```python
if backup_success:
    print("Backup completed")
else:
    print("Backup failed")
```

---

# Interview Questions

### Q1: What is conditional handling?

Conditional handling is a programming technique used to execute specific blocks of code based on whether a condition evaluates to True or False.

---

### Q2: Difference between = and == ?

| Operator | Meaning             |
| -------- | ------------------- |
| =        | Assignment Operator |
| ==       | Comparison Operator |

Example:

```python
x = 10
```

Assigns value.

```python
x == 10
```

Compares value.

---

### Q3: What is the purpose of ELSE?

ELSE executes when all previous conditions evaluate to False.

---

### Q4: What is ELIF?

ELIF stands for "Else If" and is used to handle multiple conditions.

---

### Q5: How many ELIF blocks can be used?

Unlimited.

Example:

```python
if:
elif:
elif:
elif:
elif:
else:
```

---

# Best Practices

✅ Keep conditions simple

```python
if cpu_usage > 80:
```

---

✅ Use meaningful variable names

```python
instance_type
```

instead of

```python
x
```

---

✅ Always handle invalid input

```python
else:
    print("Invalid Input")
```

---

✅ Maintain proper indentation

Python is indentation-sensitive.

---

# Summary

## Conditional Statements Overview

```text
IF
│
├── Executes when condition is TRUE
│
ELIF
│
├── Checks additional conditions
│
ELSE
│
└── Executes when all conditions fail
```

---

## Complete Flow

```text
                 Start
                    │
                    ▼
              IF Condition
              /          \
          True            False
            │                │
            ▼                ▼
       Execute IF       ELIF Check
                              │
                       ┌──────┴──────┐
                       ▼             ▼
                     True         False
                       │             │
                       ▼             ▼
                 Execute ELIF    ELSE
                       │             │
                       └──────┬──────┘
                              ▼
                             End
```

---

# Key Takeaways

* Conditional handling enables decision-making in programs.
* `if` executes code when a condition is True.
* `else` executes when the condition is False.
* `elif` helps manage multiple conditions.
* Widely used in DevOps automation scripts.
* Proper indentation is mandatory in Python.
* Only the first matching condition executes.

🚀 Conditional handling is one of the most fundamental concepts in Python and forms the foundation for writing intelligent automation scripts in DevOps.
