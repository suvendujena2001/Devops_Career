# Day 8: Lists and Tuples in Python 🚀

## Python for DevOps – Complete Notes

---

# 📌 Learning Objectives

By the end of this lesson, you will understand:

* What Lists are and why they are used
* What Tuples are and when to use them
* Differences between Lists and Tuples
* Mutability vs Immutability
* Accessing elements using indexing
* Adding and removing elements
* Finding length of collections
* List slicing
* Sorting lists
* Heterogeneous lists
* Real-world DevOps use cases
* Frequently asked interview questions

---

# Why Do We Need Lists?

Imagine storing a single student's name:

```python
name = "Abhishek"
```

This is simple.

But what if you need to store 100 student names?

Without lists:

```python
student1 = "Abhishek"
student2 = "Ram"
student3 = "John"
student4 = "Tim"
...
student100 = "XYZ"
```

This approach is:

❌ Tedious
❌ Difficult to maintain
❌ Not scalable

---

# Solution: Lists

A list allows storing multiple values inside a single variable.

```python
students = ["Abhishek", "Ram", "John", "Tim"]
```

Now all data is stored in one place.

```python
print(students)
```

Output:

```python
['Abhishek', 'Ram', 'John', 'Tim']
```

---

# Visual Representation of a List

```text
students
│
├── "Abhishek"
├── "Ram"
├── "John"
└── "Tim"
```

Or

```text
┌───────────┬───────┬───────┬──────┐
│ Abhishek  │ Ram   │ John  │ Tim  │
└───────────┴───────┴───────┴──────┘
```

---

# DevOps Use Case of Lists

In DevOps, we frequently work with collections:

### Examples

* S3 Buckets
* EC2 Instances
* EBS Volumes
* EKS Clusters
* IAM Users
* Kubernetes Pods

Instead of:

```python
bucket1 = "logs-bucket"
bucket2 = "backup-bucket"
bucket3 = "prod-bucket"
```

Use:

```python
s3_buckets = [
    "logs-bucket",
    "backup-bucket",
    "prod-bucket"
]
```

This makes automation much easier.

---

# Creating a List

```python
s3_bucket_list = [
    "abhishek-demo-bucket",
    "ramu-demo-bucket",
    "tim-demo-bucket",
    "john-demo-bucket"
]
```

---

# What is a Tuple?

A Tuple is very similar to a List.

The key difference:

> A Tuple cannot be modified after creation.

---

# List vs Tuple Syntax

## List

Uses Square Brackets

```python
my_list = ["A", "B", "C"]
```

## Tuple

Uses Parentheses

```python
my_tuple = ("A", "B", "C")
```

---

# List vs Tuple Diagram

```text
LIST
────
["A", "B", "C"]

Can Add    ✔
Can Remove ✔
Mutable    ✔


TUPLE
─────
("A", "B", "C")

Can Add    ✘
Can Remove ✘
Mutable    ✘
Immutable  ✔
```

---

# Mutability vs Immutability

## Mutable

Can be changed after creation.

Example:

```python
my_list = ["A", "B"]

my_list.append("C")
```

Result:

```python
["A", "B", "C"]
```

---

## Immutable

Cannot be changed after creation.

Example:

```python
my_tuple = ("A", "B")

my_tuple.append("C")
```

Output:

```python
AttributeError
```

---

# Why Use Tuples?

Suppose an AWS account has fixed administrators.

```python
admins = (
    "Abhishek",
    "John"
)
```

Requirements:

* No new admins should be added accidentally.
* Existing admins should not be removed.
* Data must remain protected.

Tuple ensures this.

---

# Memory Perspective

```text
LIST
│
├─ Dynamic Memory Allocation
├─ Can Grow
├─ Can Shrink
└─ Uses More Memory


TUPLE
│
├─ Fixed Memory Allocation
├─ Cannot Grow
├─ Cannot Shrink
└─ Uses Less Memory
```

---

# Interview Answer

## Difference Between List and Tuple

| Feature         | List            | Tuple      |
| --------------- | --------------- | ---------- |
| Syntax          | []              | ()         |
| Mutable         | Yes             | No         |
| Add Elements    | Yes             | No         |
| Remove Elements | Yes             | No         |
| Memory Usage    | Higher          | Lower      |
| Performance     | Slightly Slower | Faster     |
| Use Case        | Dynamic Data    | Fixed Data |

---

# Checking Data Type

```python
s3_bucket_list = [
    "bucket1",
    "bucket2"
]

print(type(s3_bucket_list))
```

Output:

```python
<class 'list'>
```

Tuple:

```python
admins = (
    "Abhishek",
    "John"
)

print(type(admins))
```

Output:

```python
<class 'tuple'>
```

---

# Adding Elements to a List

Using `append()`

```python
s3_bucket_list.append("new-bucket")
```

Before:

```python
[
 "bucket1",
 "bucket2"
]
```

After:

```python
[
 "bucket1",
 "bucket2",
 "new-bucket"
]
```

---

# Append Diagram

```text
Before

┌─────────┬─────────┐
│ bucket1 │ bucket2 │
└─────────┴─────────┘


After append()

┌─────────┬─────────┬────────────┐
│ bucket1 │ bucket2 │ new-bucket │
└─────────┴─────────┴────────────┘
```

---

# Removing Elements

Using `remove()`

```python
s3_bucket_list.remove("bucket1")
```

Result:

```python
[
 "bucket2",
 "new-bucket"
]
```

---

# Remove Diagram

```text
Before

bucket1
bucket2
bucket3

After remove(bucket1)

bucket2
bucket3
```

---

# Accessing Elements (Indexing)

Python assigns an index to every element.

```python
s3_bucket_list = [
    "abhishek",
    "ramu",
    "tim",
    "john"
]
```

---

# Index Diagram

```text
Index

  0          1        2       3
  ↓          ↓        ↓       ↓

┌────────┬────────┬────────┬────────┐
│abhishek│ ramu   │ tim    │ john   │
└────────┴────────┴────────┴────────┘
```

---

# Access First Element

```python
print(s3_bucket_list[0])
```

Output:

```python
abhishek
```

---

# Access Third Element

```python
print(s3_bucket_list[2])
```

Output:

```python
tim
```

---

# Important Rule

Python indexing starts from:

```text
0
```

NOT

```text
1
```

---

# Finding Length

Use `len()`

```python
print(len(s3_bucket_list))
```

Output:

```python
4
```

---

# Length Visualization

```text
┌────────┬────────┬────────┬────────┐
│ item1  │ item2  │ item3  │ item4  │
└────────┴────────┴────────┴────────┘

Length = 4
```

---

# List Slicing

Slicing creates a new list from an existing list.

Syntax:

```python
list[start:end]
```

---

# Example

```python
buckets = [
    "bucket1",
    "bucket2",
    "bucket3",
    "bucket4"
]

new_list = buckets[0:3]
```

Output:

```python
[
 "bucket1",
 "bucket2",
 "bucket3"
]
```

---

# Slicing Diagram

```text
Indexes

0       1       2       3
│       │       │       │

bucket1 bucket2 bucket3 bucket4

[0:3]

Take:
✔ 0
✔ 1
✔ 2

Do NOT take:
✘ 3
```

---

# Important Slicing Rule

```text
Start Index  → Included
End Index    → Excluded
```

Remember:

```python
[0:3]
```

means:

```python
0, 1, 2
```

NOT

```python
0, 1, 2, 3
```

---

# Sorting Lists

```python
numbers = [10, 20, 15]
```

Before:

```python
[10, 20, 15]
```

Sort:

```python
numbers.sort()
```

After:

```python
[10, 15, 20]
```

---

# Sorting Diagram

```text
Before

10 → 20 → 15

After

10 → 15 → 20
```

---

# String Concatenation Using List Elements

```python
print(
    s3_bucket_list[0]
    + "--"
    + s3_bucket_list[1]
)
```

Output:

```python
abhishek--ramu
```

---

# Heterogeneous Lists

Lists can store multiple data types.

```python
random_list = [
    1,
    2,
    3,
    "Ram",
    "Abhi",
    7.5
]
```

---

# Data Type Diagram

```text
random_list

├── 1         (int)
├── 2         (int)
├── 3         (int)
├── Ram       (str)
├── Abhi      (str)
└── 7.5       (float)
```

Python allows this flexibility.

---

# Commonly Used List Operations

| Operation      | Function        |
| -------------- | --------------- |
| Add Item       | append()        |
| Remove Item    | remove()        |
| Length         | len()           |
| Sort           | sort()          |
| Access Element | list[index]     |
| Slice List     | list[start:end] |

---

# DevOps Examples

## S3 Buckets

```python
s3_buckets = [
    "logs",
    "backup",
    "production"
]
```

## EC2 Instances

```python
instances = [
    "i-123",
    "i-456",
    "i-789"
]
```

## Kubernetes Pods

```python
pods = [
    "frontend",
    "backend",
    "database"
]
```

## Fixed Admin Users (Tuple)

```python
admins = (
    "admin1",
    "admin2"
)
```

---

# Frequently Asked Interview Questions

## Q1. What is a List?

A List is an ordered, mutable collection that can store multiple elements.

---

## Q2. What is a Tuple?

A Tuple is an ordered, immutable collection that cannot be modified after creation.

---

## Q3. Difference Between List and Tuple?

* List → Mutable
* Tuple → Immutable

---

## Q4. Is List Mutable?

Yes.

Elements can be added or removed.

---

## Q5. Is Tuple Mutable?

No.

Elements cannot be added or removed.

---

## Q6. How Do You Add Elements to a List?

Using:

```python
append()
```

---

## Q7. How Do You Remove Elements?

Using:

```python
remove()
```

---

## Q8. How Do You Find Length?

Using:

```python
len()
```

---

## Q9. Can a List Store Multiple Data Types?

Yes.

Example:

```python
[
    1,
    "Abhishek",
    7.5,
    True
]
```

---

## Q10. Give a Real DevOps Example of Tuple

Store fixed administrator accounts:

```python
admins = (
    "admin1",
    "admin2"
)
```

Since the list of admins should not change frequently, Tuple is preferred.

---

# Quick Revision Sheet

```text
LIST
====
[] brackets
Mutable
append()
remove()
sort()
len()

Example:
["A", "B", "C"]


TUPLE
=====
() brackets
Immutable
Cannot append
Cannot remove

Example:
("A", "B", "C")
```

---

# Key Takeaways 🎯

✅ Lists store multiple values in one variable

✅ Lists use square brackets `[]`

✅ Tuples use parentheses `()`

✅ Lists are mutable

✅ Tuples are immutable

✅ Use `append()` to add elements

✅ Use `remove()` to delete elements

✅ Use `len()` to find size

✅ Use indexing to access elements

✅ Python indexing starts from 0

✅ Lists can contain different data types

✅ Tuples are ideal for fixed, unchangeable data

✅ Lists are heavily used in DevOps automation for handling resources like EC2, S3, Pods, Volumes, and Clusters

---
