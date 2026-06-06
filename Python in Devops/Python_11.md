# 🚀 Day 11 - Python for DevOps

# Dictionaries & Sets with Real-Time GitHub API Integration

---

# 📌 Learning Objectives

In this session, we learn:

1. **Dictionaries in Python**
2. **Sets in Python**
3. **Why Dictionaries are critical for DevOps Engineers**
4. **Working with JSON Data**
5. **GitHub API Integration using Python**
6. **Handling Lists of Dictionaries**
7. **Traversing Complex Data Structures**

---

# 🌟 Why Dictionaries Matter in DevOps

As DevOps Engineers, we constantly interact with tools such as:

* GitHub
* Jira
* AWS
* Kubernetes
* Jenkins
* Azure DevOps

Most of these tools communicate through APIs and return data in **JSON format**.

JSON data is typically converted into **Python Dictionaries** for further processing.

---

# Understanding the Need for Dictionaries

Before dictionaries, we learned:

| Data Type | Purpose               |
| --------- | --------------------- |
| String    | Store a single value  |
| Integer   | Store numbers         |
| List      | Store multiple values |
| Tuple     | Immutable collection  |

But what if we want to store **properties of an object**?

Example:

An EC2 instance has:

* Instance ID
* Instance Type
* Public IP
* Region
* Status

A Student has:

* Name
* Age
* Class
* Roll Number

This is where **Dictionaries** shine.

---

# ❌ Problem with Lists

Suppose we store student information in a list:

```python
student = ["Abhishek", 10, "11th"]
```

After a few days:

```python
student[0]
student[1]
student[2]
```

Question:

What does index `1` represent?

* Age?
* Class?
* Roll Number?

Not clear.

---

# ✅ Solution: Dictionary

Dictionary stores data using:

## Key → Value Pairs

```python
student = {
    "name": "Abhishek",
    "age": 10,
    "class": "11th"
}
```

---

# Dictionary Structure

```text
┌──────────┬────────────┐
│   Key    │   Value    │
├──────────┼────────────┤
│ name     │ Abhishek   │
│ age      │ 10         │
│ class    │ 11th       │
└──────────┴────────────┘
```

---

# Benefits of Dictionaries

✅ Self-descriptive

✅ Easy to read

✅ Easy to maintain

✅ Suitable for storing object properties

✅ Closely matches JSON structure

---

# Creating a Dictionary

```python
student_info = {
    "name": "Abhi",
    "age": 10,
    "class": "11th"
}
```

---

# Accessing Dictionary Values

```python
print(student_info["name"])
```

Output:

```text
Abhi
```

---

# Visual Representation

```text
student_info
│
├── name  → Abhi
├── age   → 10
└── class → 11th
```

---

# Lists vs Dictionaries

## List

```python
student = ["Abhi", 10, "11th"]
```

Access:

```python
student[1]
```

Not intuitive.

---

## Dictionary

```python
student = {
    "name": "Abhi",
    "age": 10,
    "class": "11th"
}
```

Access:

```python
student["age"]
```

Much clearer.

---

# 📚 List of Dictionaries

Real-world applications often contain multiple objects.

Example:

Multiple EC2 Instances.

```python
ec2_instances = [
    {
        "id": "instance-001",
        "type": "t2.micro"
    },
    {
        "id": "instance-002",
        "type": "t2.medium"
    },
    {
        "id": "instance-003",
        "type": "t2.xlarge"
    }
]
```

---

# Visual Structure

```text
ec2_instances
│
├── [0]
│     ├── id   → instance-001
│     └── type → t2.micro
│
├── [1]
│     ├── id   → instance-002
│     └── type → t2.medium
│
└── [2]
      ├── id   → instance-003
      └── type → t2.xlarge
```

---

# Accessing Nested Data

Get type of second instance:

```python
print(ec2_instances[1]["type"])
```

Output:

```text
t2.medium
```

---

# 🔥 Real-Time DevOps Use Case

## Task

Fetch Pull Requests from a GitHub Repository using Python.

---

# Architecture Diagram

```text
┌──────────────┐
│ Python Script│
└──────┬───────┘
       │
       │ HTTP Request
       ▼
┌──────────────┐
│ GitHub API   │
└──────┬───────┘
       │
       │ JSON Response
       ▼
┌──────────────┐
│ Dictionary   │
│ Conversion   │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│ Data Parsing │
└──────────────┘
```

---

# Algorithm

## Step 1

Install Requests Module

```bash
pip install requests
```

---

## Step 2

Import Module

```python
import requests
```

---

## Step 3

Call GitHub API

```python
url = "https://api.github.com/repos/kubernetes/kubernetes/pulls"

response = requests.get(url)
```

---

## Step 4

Convert JSON Response

```python
data = response.json()
```

---

## Step 5

Print Required Information

```python
print(data)
```

---

# Understanding Response Object

```python
response = requests.get(url)
```

Response is an object.

---

# Check API Status

```python
print(response.status_code)
```

Output:

```text
200
```

Meaning:

```text
200 → Success
404 → Not Found
403 → Forbidden
500 → Server Error
```

---

# JSON Response Structure

GitHub returns:

```text
[
  {
    pull_request_1
  },
  {
    pull_request_2
  },
  {
    pull_request_3
  }
]
```

This is:

## List of Dictionaries

---

# Access First Pull Request

```python
data = response.json()

print(data[0])
```

---

# Access Pull Request Creator

```python
print(data[0]["user"])
```

---

# Access Username

```python
print(data[0]["user"]["login"])
```

Output:

```text
neo123
```

---

# Nested Dictionary Diagram

```text
data
│
└── [0]
      │
      └── user
            │
            └── login
                  │
                  └── neo123
```

---

# Printing All Usernames

Using For Loop:

```python
for i in range(len(data)):
    print(data[i]["user"]["login"])
```

---

# Better Approach

```python
for pr in data:
    print(pr["user"]["login"])
```

---

# Expected Output

```text
neo123
lins
ravent
xyz
...
```

---

# Real DevOps Learning

This example teaches:

* APIs
* JSON
* Dictionaries
* Loops
* Automation
* GitHub Integration

These are core DevOps skills.

---

# Dictionary Operations

## Add Key

```python
student["city"] = "Bangalore"
```

---

## Update Value

```python
student["age"] = 12
```

---

## Delete Key

```python
del student["class"]
```

---

# ⚡ Python Sets

---

# What is a Set?

A Set is a collection of:

✅ Unique Elements

❌ No Duplicates

---

# Creating a Set

```python
students = {
    "Abhi",
    "Ram",
    "John"
}
```

---

# Duplicate Example

```python
students = {
    "Abhi",
    "Ram",
    "Abhi"
}
```

Output:

```text
{'Abhi', 'Ram'}
```

Duplicate removed automatically.

---

# Why Use Sets?

Useful when uniqueness matters.

Example:

* S3 Bucket Names
* User IDs
* Email IDs
* Unique Resources

---

# Data Type Comparison

| Feature            | List | Tuple | Set | Dictionary |
| ------------------ | ---- | ----- | --- | ---------- |
| Ordered            | Yes  | Yes   | No  | Yes        |
| Mutable            | Yes  | No    | Yes | Yes        |
| Duplicates Allowed | Yes  | Yes   | No  | Keys No    |
| Key-Value Pair     | No   | No    | No  | Yes        |

---

# Set Operations

Consider:

```python
A = {"Abhi", "Ram", "Tim"}
B = {"Ram", "John", "Tim"}
```

---

# 1. Union

All elements.

```python
A | B
```

Diagram:

```text
      A ∪ B

    _________
   /         \
  / A       B \
 |             |
 | Abhi Ram Tim|
 |      John   |
  \           /
   \_________/
```

Output:

```python
{
 'Abhi',
 'Ram',
 'Tim',
 'John'
}
```

---

# 2. Intersection

Common elements.

```python
A & B
```

Diagram:

```text
        A ∩ B

      _______
     /   |   \
    / A  | B  \
   |     |     |
   | Ram | Tim |
    \    |    /
     \___|___/
```

Output:

```python
{'Ram', 'Tim'}
```

---

# 3. Difference

Elements only in A.

```python
A - B
```

Output:

```python
{'Abhi'}
```

---

# Quick Revision

```text
String
  ↓
Single Value

List
  ↓
Multiple Values

Dictionary
  ↓
Properties of an Object
(Key → Value)

List of Dictionaries
  ↓
Multiple Objects

JSON
  ↓
Converted to Dictionary

Set
  ↓
Unique Values Only
```

---

# 🎯 Key Takeaways

### Dictionaries

* Most important Python data structure for DevOps.
* Stores data as Key-Value pairs.
* Used heavily while working with APIs.
* JSON data is commonly converted into dictionaries.
* Supports nested structures.

### GitHub API Example

* Install Requests
* Call API
* Receive JSON
* Convert to Dictionary
* Parse Data
* Automate Operations

### Sets

* Store unique elements.
* Automatically remove duplicates.
* Useful for mathematical operations.
* Supports Union, Intersection, and Difference.

---

# DevOps Interview Questions

### Q1. Why are dictionaries important in DevOps?

Because API responses from tools like AWS, GitHub, Jira, and Kubernetes are usually JSON objects, which map naturally to Python dictionaries.

---

### Q2. Difference between List and Dictionary?

List uses indexes.

Dictionary uses keys.

---

### Q3. What does `response.json()` do?

Converts API JSON response into Python-native data structures (typically dictionaries/lists).

---

### Q4. What is a Set?

A collection of unique elements.

---

### Q5. Name three Set operations.

1. Union
2. Intersection
3. Difference

---

# 🏆 Final Conclusion

Dictionaries are among the most important Python concepts for DevOps Engineers because almost every automation workflow involves processing API responses. Understanding dictionaries, nested dictionaries, and lists of dictionaries is foundational for working with AWS, Kubernetes, GitHub, Jenkins, and countless other tools. Sets, while used less frequently, provide an elegant way to handle unique collections and perform mathematical set operations efficiently.
