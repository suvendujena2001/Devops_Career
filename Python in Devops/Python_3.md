# Day 3 — Keywords, Variables & Best Practices in Python

## Python for DevOps Notes

> Based on the session: **Keywords, Variables & Best Practices | Global vs Local**

---

# Table of Contents

1. Introduction
2. Recap of Day 1 & Day 2
3. The 4 Strong Pillars of Programming
4. What are Keywords?
5. Important Python Keywords
6. Real-Time DevOps Examples of Keywords
7. Understanding Variables
8. Why Variables are Important
9. Hardcoding vs Variables
10. Variable Declaration in Python
11. Dynamic Typing in Python
12. Global vs Local Variables
13. Variable Naming Best Practices
14. Snake Case vs Camel Case
15. Common Mistakes
16. Interview Tips
17. Summary
18. Practice Exercises

---

# 1. Introduction

In this session, the focus is on:

* **Keywords**
* **Variables**
* **Variable Scope**
* **Best Practices**

These are foundational concepts in Python and are extremely important for:

* Writing scripts
* Automation
* DevOps tasks
* Interviews
* Real-world projects

---

# 2. Recap of Previous Classes

## Day 1 Covered

* Introduction to Python for DevOps
* Why Python is used in DevOps
* Shell Scripting vs Python
* Installing Python
* Running first Python script

---

## Day 2 Covered

* Data Types
* String Data Type
* Numeric Data Types
* String Manipulation
* Built-in String Functions

---

# 3. The 4 Strong Pillars of Programming

According to the instructor, every programming language is built on **4 major pillars**.

```text
                PROGRAMMING MASTERY
                         |
    ------------------------------------------------
    |                |              |              |
 Keywords         Data Types     Operators    Logical Thinking
```

---

## 1. Keywords

Reserved words used to build logic in programs.

Examples:

* `if`
* `for`
* `while`
* `try`
* `class`

---

## 2. Data Types

Defines what kind of data a variable stores.

Examples:

* String
* Integer
* Float
* Boolean
* List
* Dictionary

---

## 3. Operators

Used for:

* Arithmetic
* Comparison
* Assignment
* Logical operations

Examples:

```python
+
-
*
/
==
=
and
or
```

---

## 4. Logical Reasoning

The ability to:

* Think programmatically
* Solve problems
* Build automation logic

> This improves mainly through practice.

---

# 4. What are Keywords?

## Definition

Keywords are **reserved words in Python** that have special meanings.

They are used to create:

* Conditions
* Loops
* Functions
* Classes
* Error handling
* Logic

---

# Real-Life Analogy

Without verbs, English sentences are incomplete.

Similarly:

> Without keywords, Python programs cannot be written properly.

---

# Example

## Printing Numbers 1 to 100

Instead of:

```python
print(1)
print(2)
print(3)
```

We use:

```python
for i in range(1, 101):
    print(i)
```

Here:

* `for` → keyword
* `in` → keyword

---

# 5. Important Python Keywords

## Commonly Used Python Keywords

| Keyword    | Purpose                 |
| ---------- | ----------------------- |
| `and`      | Logical AND             |
| `or`       | Logical OR              |
| `not`      | Logical NOT             |
| `if`       | Condition               |
| `elif`     | Else-if condition       |
| `else`     | Alternative condition   |
| `for`      | Loop                    |
| `while`    | Loop                    |
| `try`      | Error handling          |
| `except`   | Catch errors            |
| `finally`  | Cleanup block           |
| `def`      | Define function         |
| `return`   | Return value            |
| `class`    | Create class            |
| `import`   | Import module           |
| `from`     | Import specific item    |
| `True`     | Boolean true            |
| `False`    | Boolean false           |
| `None`     | Null/empty value        |
| `is`       | Identity comparison     |
| `lambda`   | Anonymous function      |
| `with`     | Context manager         |
| `global`   | Declare global variable |
| `nonlocal` | Non-local scope         |

---

# 6. Real-Time DevOps Examples of Keywords

---

## Example 1 — `if`

### Scenario

Print only S3 buckets containing `"abhishek"`.

```python
if "abhishek" in bucket_name:
    print(bucket_name)
```

---

## Example 2 — `for`

### Scenario

Loop through EC2 instances.

```python
for instance in instances:
    print(instance)
```

---

## Example 3 — `try-except`

### Scenario

Handle API errors safely.

```python
try:
    connect_aws()
except:
    print("Connection failed")
```

---

# 7. Understanding Variables

---

# What is a Variable?

A variable is a **container used to store data**.

---

# Example Without Variable

```python
print("Abhishek")
```

---

# Example With Variable

```python
name = "Abhishek"
print(name)
```

---

# Diagram — Variable Storage

```text
+------------------+
| Variable: name   |
+------------------+
| Value: Abhishek  |
+------------------+
```

---

# 8. Why Variables are Important

Variables help in:

* Reusability
* Easy maintenance
* Reducing errors
* Cleaner code

---

# Hardcoding Problem

Suppose a script contains:

```python
print("project-xyz-instance")
```

in 100 places.

If the instance name changes:

```text
project-xyz-instance
        ↓
project-abc-instance
```

You must update all 100 places manually.

---

# Better Approach Using Variables

```python
ec2_instance_name = "project-xyz-instance"

print(ec2_instance_name)
print(ec2_instance_name)
print(ec2_instance_name)
```

Now update only once:

```python
ec2_instance_name = "project-abc-instance"
```

Everything updates automatically.

---

# Advantages of Variables

| Advantage        | Explanation                   |
| ---------------- | ----------------------------- |
| Reusability      | Use same value multiple times |
| Easy maintenance | Change value once             |
| Reduces errors   | Avoid missing updates         |
| Cleaner code     | More readable                 |

---

# 9. Hardcoding vs Variables

| Hardcoding            | Variables        |
| --------------------- | ---------------- |
| Difficult to maintain | Easy to maintain |
| Repeated values       | Reusable         |
| Error-prone           | Safer            |
| Less readable         | Cleaner code     |

---

# 10. Variable Declaration in Python

---

# Syntax

```python
variable_name = value
```

---

# Example

```python
name = "Abhishek"
age = 25
is_devops_engineer = True
```

---

# Understanding the Assignment

```python
name = "Abhishek"
```

Means:

```text
Variable name ---> stores ---> "Abhishek"
```

---

# Important Point

Python does NOT require explicit type declaration.

Unlike Go:

```go
var name string = "Abhishek"
```

Python simply uses:

```python
name = "Abhishek"
```

---

# 11. Dynamic Typing in Python

Python is a:

# Dynamically Typed Language

Meaning:

* No need to declare variable type manually.
* Python automatically detects type.

---

# Example

```python
name = "Abhishek"   # string
age = 25            # integer
salary = 99.5       # float
is_active = True    # boolean
```

---

# Comparison Diagram

## Static Typing (Go/Java)

```text
Declare Type → Create Variable → Assign Value
```

Example:

```go
var age int = 25
```

---

## Dynamic Typing (Python)

```text
Create Variable → Assign Value
```

Example:

```python
age = 25
```

---

# 12. Global vs Local Variables

This is one of the most important concepts.

---

# Global Variables

Variables declared outside functions.

Accessible everywhere.

---

# Local Variables

Variables declared inside functions.

Accessible only inside that function.

---

# Diagram — Scope

```text
GLOBAL SCOPE
---------------------------------
a = 10
b = 5

    FUNCTION 1
    ----------------
    can access a,b

    FUNCTION 2
    ----------------
    can access a,b
```

---

# Example — Global Variables

```python
a = 10
b = 5

def addition():
    print(a + b)

def subtraction():
    print(a - b)

addition()
subtraction()
```

---

# Output

```text
15
5
```

---

# Why?

Because:

```python
a = 10
b = 5
```

are globally accessible.

---

# Example — Local Variables

```python
def addition():
    a = 10
    b = 5
    print(a + b)

def subtraction():
    print(a - b)

addition()
subtraction()
```

---

# Output

```text
15
NameError: name 'a' is not defined
```

---

# Why Error Happens

Variables:

```python
a = 10
b = 5
```

exist only inside:

```python
addition()
```

They are local variables.

---

# Local Variable Example

```python
a = 10
b = 5

def addition():
    c = 10
    print(a + b + c)

def subtraction():
    print(a - b)

addition()
subtraction()
```

---

# Output

```text
25
5
```

Here:

* `a`, `b` → global
* `c` → local

---

# 13. Variable Naming Best Practices

Very important for:

* Interviews
* Production code
* Team collaboration

---

# Rule 1 — Use Lowercase

✅ Good

```python
name = "Abhishek"
```

❌ Bad

```python
NAME = "Abhishek"
```

---

# Rule 2 — Use Descriptive Names

✅ Good

```python
ec2_instance_name = "project-xyz-instance"
```

❌ Bad

```python
x = "project-xyz-instance"
```

---

# Rule 3 — Use Proper Naming Style

Two popular styles:

1. Snake Case
2. Camel Case

---

# 14. Snake Case vs Camel Case

---

# Snake Case

Words separated using underscores.

```python
ec2_instance_name = "project-xyz-instance"
```

---

# Diagram

```text
ec2_instance_name
   ↑       ↑
 underscores
```

---

# Camel Case

Each new word starts with capital letter.

```python
ec2InstanceName = "project-xyz-instance"
```

---

# Diagram

```text
ec2 Instance Name
    ↑        ↑
 Capital Letters
```

---

# Which One is Preferred in Python?

✅ Snake Case

Python officially recommends:

# PEP 8 Style Guide

Use:

```python
snake_case
```

for variable and function names.

---

# 15. Common Mistakes

---

## Mistake 1 — Hardcoding Values

❌

```python
print("server-1")
```

✅

```python
server_name = "server-1"
print(server_name)
```

---

## Mistake 2 — Using Non-Descriptive Variables

❌

```python
a = "server-1"
```

✅

```python
server_name = "server-1"
```

---

## Mistake 3 — Scope Confusion

❌

```python
def test():
    x = 5

print(x)
```

This gives error.

---

# 16. Interview Tips

---

# Frequently Asked Questions

## Q1. What is a variable?

A container used to store values.

---

## Q2. Difference between local and global variable?

| Global                    | Local                    |
| ------------------------- | ------------------------ |
| Declared outside function | Declared inside function |
| Accessible everywhere     | Accessible only locally  |

---

## Q3. Why Python is dynamically typed?

Because variable types are automatically detected.

---

## Q4. Why variables are important?

* Reusability
* Maintainability
* Reduces errors

---

# 17. Final Summary

---

# What We Learned

## Keywords

* Reserved words
* Used to build logic
* Essential in Python

---

## Variables

* Store values
* Reduce hardcoding
* Improve maintainability

---

## Dynamic Typing

Python automatically detects data types.

---

## Scope

### Global Variables

Accessible everywhere.

### Local Variables

Accessible only inside functions.

---

## Best Practices

✅ Use lowercase
✅ Use descriptive names
✅ Prefer snake_case
✅ Avoid hardcoding

---

# 18. Practice Exercises

---

# Exercise 1

Create variables for:

* EC2 instance name
* S3 bucket name
* AWS region

Print all values.

---

# Exercise 2

Write a program using global variables.

---

# Exercise 3

Create a function with local variables.

---

# Exercise 4

Convert these into snake_case:

```text
FullName
ServerName
EmployeeSalary
```

---

# Exercise 5

Identify keywords:

```python
for i in range(5):
    if i == 2:
        print(i)
```

---

# Quick Revision Sheet

| Concept         | Key Point                       |
| --------------- | ------------------------------- |
| Keywords        | Reserved words                  |
| Variables       | Store values                    |
| Dynamic Typing  | No type declaration needed      |
| Global Variable | Accessible everywhere           |
| Local Variable  | Accessible inside function only |
| Snake Case      | Recommended in Python           |
| Hardcoding      | Bad practice                    |

---

# End Notes

> Mastering keywords and variables builds the foundation for:
>
> * Functions
> * Loops
> * Automation
> * AWS scripting
> * Kubernetes scripting
> * DevOps tools
>
> These concepts are small but extremely powerful in real-world Python development.
