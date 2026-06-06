# Day 10 – Python for DevOps

# Real-Time Use Case with Lists, Loops, and Exception Handling

---

# 🎯 Learning Objectives

By the end of this session, you will understand:

* How DevOps engineers use **Lists** in real-world automation.
* How to take **runtime input** from users.
* How to iterate through lists using **For Loops**.
* How to use the **OS Module** to interact with the operating system.
* How to implement **Exception Handling** using `try` and `except`.
* How to build a practical Python script that lists files from multiple directories.

---

# 📚 Recap of Previous Learning

Before this session, we already learned:

## Lists and Tuples

| Concept  | Description                      |
| -------- | -------------------------------- |
| List     | Mutable collection of elements   |
| Tuple    | Immutable collection of elements |
| Indexing | Access elements using position   |
| append() | Add elements                     |
| remove() | Remove elements                  |
| len()    | Find length                      |
| range()  | Generate sequences               |

---

## Loops

We also learned:

```python
for item in sequence:
    print(item)
```

Loops allow us to process each element one by one.

Without loops, today's use case would not be possible.

---

# 🚀 Real-World DevOps Use Case

## Problem Statement

A user provides multiple folder paths.

The program should:

1. Read folder names.
2. Visit each folder.
3. List all files inside it.
4. Handle invalid folder names gracefully.

---

# 🏗️ Input and Output Design

## Input

```text
/tmp /opt /abc
```

User provides folder names separated by spaces.

---

## Expected Output

```text
Listing files for folder /tmp

file1.log
file2.txt
file3.conf

Listing files for folder /opt

python
containerd
k8s
```

---

# 🧠 Step 1: Designing the Algorithm

Before writing code, break the problem into smaller tasks.

---

## Algorithm Flow

```text
START
   |
   v
Read folder names from user
   |
   v
Convert input string into List
   |
   v
Loop through each folder
   |
   v
List files using OS module
   |
   v
Print files
   |
   v
Handle errors if folder doesn't exist
   |
   v
END
```

---

# 📥 Step 2: Taking Input from User

Python provides multiple ways to take input:

| Method                 | Usage      |
| ---------------------- | ---------- |
| Command Line Arguments | sys.argv   |
| Environment Variables  | os.environ |
| Runtime Input          | input()    |

Today we focus on:

```python
input()
```

---

## Example

```python
number = input("Please provide a number: ")

print(number)
```

---

### Execution

```text
Please provide a number:
5

5
```

---

# 🧩 Why Use input()?

The input is collected **during program execution**.

This is called:

## Runtime Input

```text
Program Starts
      |
      v
Ask User for Input
      |
      v
Wait for User Response
      |
      v
Continue Execution
```

---

# 🌍 Real DevOps Examples of Runtime Input

## Terraform

```bash
terraform apply
```

Output:

```text
Do you want to perform these actions?
Enter yes to continue:
```

---

## Apt Package Manager

```bash
apt install nginx
```

Output:

```text
Do you want to continue? [Y/n]
```

Both tools use runtime input internally.

---

# 📝 Step 3: Read Folder Names

---

## Code

```python
folders = input(
    "Please provide folder names with spaces: "
)
```

Example Input:

```text
/tmp /opt /abc
```

---

# 🔄 Converting String to List

User may not know Python lists.

Instead of asking:

```python
["/tmp", "/opt", "/abc"]
```

We ask:

```text
/tmp /opt /abc
```

Then convert it ourselves.

---

## Using split()

```python
folders = input(
    "Please provide folder names with spaces: "
).split()
```

---

### What Happens Internally?

```text
Input String

"/tmp /opt /abc"
       |
       v
split()
       |
       v

["/tmp", "/opt", "/abc"]
```

---

# 🔁 Step 4: Loop Through Folders

Once we have a list:

```python
["/tmp", "/opt", "/abc"]
```

We process each folder one by one.

---

## Code

```python
for folder in folders:
    print(folder)
```

---

## Visual Representation

```text
Folders List

["/tmp", "/opt", "/abc"]

        |
        v

Iteration 1 → /tmp
Iteration 2 → /opt
Iteration 3 → /abc
```

---

# 🖥️ Step 5: Use OS Module

Python provides an OS module for interacting with the operating system.

---

## Import OS Module

```python
import os
```

---

# 📂 Listing Files in a Directory

Python provides:

```python
os.listdir()
```

Equivalent to:

```bash
ls
```

in Linux.

---

## Example

```python
files = os.listdir("/opt")
```

Output:

```python
[
  "containerd",
  "python",
  "tmp"
]
```

---

# 🔍 Integrating It Into Our Loop

```python
for folder in folders:
    files = os.listdir(folder)

    print(files)
```

---

# 🧾 Better Output Formatting

Instead of printing raw lists:

```python
['a', 'b', 'c']
```

Print each file separately.

---

## Nested Loop

```python
for folder in folders:

    files = os.listdir(folder)

    for file in files:
        print(file)
```

---

# 📊 Complete Logic Flow

```text
Folders List
     |
     v

For Each Folder
     |
     v

Get Files
     |
     v

For Each File
     |
     v

Print File Name
```

---

# ⚠️ Problem: Invalid Folder

Suppose user enters:

```text
/xyz /opt
```

Where:

```text
/xyz
```

does not exist.

---

## Without Exception Handling

Program crashes:

```text
FileNotFoundError:
No such file or directory
```

---

# 🛡️ Exception Handling

Exception Handling allows us to:

* Detect known errors.
* Show meaningful messages.
* Prevent abrupt program termination.

---

# Structure of Exception Handling

```text
TRY
 |
 |-- Execute Code
 |
 |-- Success?
 |       |
 |      YES ---> Continue
 |
 NO
 |
 v
EXCEPT
 |
 v
Handle Error Gracefully
```

---

# Syntax

```python
try:
    # risky code

except ErrorType:
    # handle error
```

---

# Handling Missing Folders

```python
try:
    files = os.listdir(folder)

except FileNotFoundError:
    print("Please provide valid folder")
```

---

# Enhanced Version

```python
try:
    files = os.listdir(folder)

except FileNotFoundError:
    print(
        f"Folder does not exist: {folder}"
    )
```

---

# Using break

If you want to stop execution:

```python
break
```

Example:

```python
except FileNotFoundError:
    print("Invalid folder")
    break
```

---

# Permission Errors

Another common DevOps scenario:

User has no access to a folder.

---

Example:

```text
/etc/shadow
```

may require elevated privileges.

---

Handle it as:

```python
except PermissionError:
    print(
        f"No access to folder: {folder}"
    )
```

---

# Multiple Exceptions

```python
try:
    files = os.listdir(folder)

except FileNotFoundError:
    print("Folder not found")

except PermissionError:
    print("Access denied")
```

---

# 📊 Exception Handling Diagram

```text
             TRY
               |
       ----------------
       |              |
       v              v

Folder Exists?     Folder Missing?
       |              |
      YES             |
       |              |
       v              |
List Files            |
       |              |
       v              |
Continue       FileNotFoundError
                      |
                      v
               Print Error
```

---

# 🧮 Additional Example: Division Calculator

Suppose user enters:

```text
5
0
```

And we perform:

```python
5 / 0
```

---

Result:

```text
ZeroDivisionError
```

---

# Handling It

```python
try:
    result = num1 / num2

except ZeroDivisionError:
    print(
        "Please do not provide zero"
    )
```

---

# Exception Handling Workflow

```text
User Input
    |
    v

Division
    |
    v

Is Divisor Zero?
    |
  ------
  |    |
 NO   YES
  |    |
  v    v

Result  Exception
          |
          v

   Show Friendly Message
```

---

# 🏆 Final Complete Program

```python
import os

folders = input(
    "Please provide folder names: "
).split()

for folder in folders:

    try:

        print(
            f"\nListing files for folder {folder}"
        )

        files = os.listdir(folder)

        for file in files:
            print(file)

    except FileNotFoundError:

        print(
            f"Folder does not exist: {folder}"
        )

    except PermissionError:

        print(
            f"No permission to access: {folder}"
        )
```

---

# 🔥 Production-Level Version Using Functions

Functions improve:

* Readability
* Reusability
* Maintainability

---

## Modular Design

```text
main()
   |
   v

list_files()
   |
   v

Exception Handling
```

---

## Example

```python
import os


def list_files(folder):

    try:

        files = os.listdir(folder)

        for file in files:
            print(file)

    except FileNotFoundError:
        print(
            f"Folder not found: {folder}"
        )

    except PermissionError:
        print(
            f"No access: {folder}"
        )


def main():

    folders = input(
        "Provide folders: "
    ).split()

    for folder in folders:

        print(
            f"\nListing files for {folder}"
        )

        list_files(folder)


main()
```

---

# 📌 Key Takeaways

## Lists

✅ Store multiple folder names.

```python
["/tmp", "/opt", "/abc"]
```

---

## split()

✅ Converts string into list.

```python
input().split()
```

---

## Loops

✅ Process each folder one by one.

```python
for folder in folders:
```

---

## OS Module

✅ Interacts with operating system.

```python
os.listdir()
```

---

## Exception Handling

✅ Prevents program crashes.

```python
try:
    ...
except:
    ...
```

---

# 💡 DevOps Relevance

This simple program demonstrates concepts used daily by DevOps engineers:

| DevOps Activity           | Python Concept Used |
| ------------------------- | ------------------- |
| Log Analysis              | Lists + Loops       |
| File Management           | OS Module           |
| Server Automation         | Functions           |
| Monitoring Scripts        | Exception Handling  |
| Deployment Tools          | Runtime Inputs      |
| Infrastructure Automation | Modular Programming |

---

# 🎯 Final Summary

This lesson combines multiple Python fundamentals into a practical DevOps automation script:

```text
Input
  ↓
List
  ↓
Loop
  ↓
OS Module
  ↓
List Files
  ↓
Exception Handling
  ↓
User-Friendly Output
```

This is one of the first examples of how Python transitions from a programming language into a powerful DevOps automation tool.
