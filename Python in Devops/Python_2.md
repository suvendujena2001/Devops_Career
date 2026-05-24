# Day 2 Notes — Python for DevOps

## Data Types | Strings | String Handling Functions | Numeric Types | Regular Expressions

> Based on the session by Abhishek Veeramalla
> Topic Focus: Python Data Types, String Operations, Numeric Types, and Introduction to Regular Expressions.

---

# 📚 Table of Contents

1. Introduction to Data Types
2. Why Data Types Matter
3. Categories of Python Data Types
4. Dynamically Typed Languages
5. String Data Type
6. String Handling Functions
7. String Concatenation
8. Real-Time DevOps Example (AWS ARN Parsing)
9. Numeric Data Types
10. Integer Functions
11. Float Functions
12. Regular Expressions (Regex)
13. Regex in DevOps
14. Important Interview Questions
15. Practical Exercises
16. Summary Cheat Sheet

---

# 1️⃣ Introduction to Data Types

## What is Data?

Anything written in a Python program is treated as **data**.

Examples:

```python
"Abhishek"
6
6.3
True
```

Humans understand meanings naturally.

But for Python Interpreter:

* `"Abhishek"` → Text Data
* `6` → Integer Data
* `6.3` → Decimal Data

Python needs a way to classify data.

That classification is called:

# ✅ Data Type

---

# 2️⃣ Why Data Types Matter

Data types help Python understand:

| Purpose           | Example                  |
| ----------------- | ------------------------ |
| Storage           | Save data correctly      |
| Operations        | Add, split, compare      |
| Memory Allocation | Efficient processing     |
| Error Prevention  | Avoid invalid operations |

---

# 3️⃣ Categories of Python Data Types

```text
Python Data Types
│
├── String
│
├── Numeric
│   ├── Integer
│   └── Float
│
├── Sequence
│   ├── List
│   └── Tuple
│
├── Mapping
│   └── Dictionary
│
├── Set
│
└── Boolean
```

---

# 📊 Python Data Type Hierarchy Diagram

```text
                 PYTHON DATA TYPES
                         │
 ┌──────────────┬────────┴─────────┬──────────────┐
 │              │                  │              │
String       Numeric           Sequence       Boolean
                │                  │
        ┌───────┴───────┐      ┌───┴────┐
        │               │      │        │
      Integer         Float   List    Tuple
```

---

# 4️⃣ Dynamically Typed Languages

Python is a:

# ✅ Dynamically Typed Language

Meaning:

You DO NOT explicitly define data types.

Example:

```python
name = "Abhishek"
age = 25
salary = 99.99
```

Python automatically detects:

| Value        | Data Type |
| ------------ | --------- |
| `"Abhishek"` | String    |
| `25`         | Integer   |
| `99.99`      | Float     |

---

# 🔥 Interview Question

## Is Python statically typed or dynamically typed?

✅ Answer:

Python is a dynamically typed programming language.

---

# 5️⃣ String Data Type

## What is a String?

A string is a sequence/list of characters.

Example:

```python
"Abhishek"
```

Characters:

```text
A b h i s h e k
```

---

# Declaring Strings

## Using Double Quotes

```python
name = "Abhishek"
```

## Using Single Quotes

```python
name = 'Abhishek'
```

Both are valid.

---

# 🧠 Important Concept

Python identifies strings because they are enclosed inside:

* `" "`
* `' '`

Without quotes:

```python
Abhishek
```

Python assumes it is a variable name.

---

# 6️⃣ String Handling Functions

Python provides many built-in string functions.

---

# Why Built-In Functions Matter

Advantages:

✅ Less code
✅ Faster development
✅ Reliable
✅ Already tested
✅ Used in interviews

---

# 📊 Common String Functions

| Function    | Purpose              |
| ----------- | -------------------- |
| `split()`   | Split string         |
| `upper()`   | Convert to uppercase |
| `lower()`   | Convert to lowercase |
| `strip()`   | Remove spaces        |
| `replace()` | Replace text         |
| `len()`     | Find length          |
| `find()`    | Find substring       |

---

# 7️⃣ String Concatenation

## Concatenation = Joining Strings

Example:

```python
str1 = "Hello"
str2 = "World"

print(str1 + " " + str2)
```

## Output

```text
Hello World
```

---

# 📌 Diagram — String Concatenation

```text
"Hello" + " " + "World"

        ↓

"Hello World"
```

---

# 8️⃣ Real-Time DevOps Example — AWS ARN Parsing

## What is an ARN?

ARN = Amazon Resource Name

Example:

```text
arn:aws:iam::123456789012:user/Abhishek
```

---

# 🎯 Goal

Extract username:

```text
Abhishek
```

---

# Solution Using `split()`

```python
arn = "arn:aws:iam::123456789012:user/Abhishek"

print(arn.split("/")[1])
```

---

# 🔍 Step-by-Step Breakdown

## Step 1

Split using `/`

```python
arn.split("/")
```

Output:

```python
['arn:aws:iam::123456789012:user', 'Abhishek']
```

---

## Step 2

Access index `[1]`

```python
['arn:aws:iam::123456789012:user', 'Abhishek']
```

| Index | Value      |
| ----- | ---------- |
| 0     | ARN Prefix |
| 1     | Username   |

---

# 📊 Visualization

```text
Original String
│
├── arn:aws:iam::123456789012:user
└── Abhishek
      ↑
   index [1]
```

---

# 9️⃣ Important String Functions

# 🔹 `upper()`

Converts to uppercase.

```python
name = "abhishek"

print(name.upper())
```

Output:

```text
ABHISHEK
```

---

# 🔹 `lower()`

```python
name = "ABHISHEK"

print(name.lower())
```

Output:

```text
abhishek
```

---

# 🔹 `strip()`

Removes extra spaces.

```python
text = "  hello  "

print(text.strip())
```

Output:

```text
hello
```

---

# 🔹 `len()`

Returns total characters.

```python
text = "Hello World"

print(len(text))
```

Output:

```text
11
```

---

# 📊 String Function Summary Table

| Function  | Example          | Output      |
| --------- | ---------------- | ----------- |
| `upper()` | `"abc".upper()`  | `"ABC"`     |
| `lower()` | `"ABC".lower()`  | `"abc"`     |
| `strip()` | `" hi ".strip()` | `"hi"`      |
| `split()` | `"a b".split()`  | `['a','b']` |
| `len()`   | `len("abc")`     | `3`         |

---

# 1️⃣0️⃣ Numeric Data Types

Python numeric types are mainly:

```text
Numeric
│
├── Integer (int)
└── Float
```

---

# Integer (`int`)

Whole numbers.

Examples:

```python
10
-5
1000
```

---

# Float

Decimal numbers.

Examples:

```python
5.0
3.14
9.99
```

---

# 📊 Integer vs Float

| Integer     | Float          |
| ----------- | -------------- |
| `5`         | `5.0`          |
| `10`        | `10.5`         |
| No decimals | Decimal values |

---

# 1️⃣1️⃣ Integer Functions

# 🔹 `abs()`

Returns absolute value.

```python
print(abs(-10))
```

Output:

```text
10
```

---

# 📌 Diagram

```text
-10  ── abs() ──►  10
```

---

# 1️⃣2️⃣ Float Functions

# 🔹 `round()`

Rounds decimal numbers.

```python
pi = 3.141592

print(round(pi, 2))
```

Output:

```text
3.14
```

---

# 📊 Rounding Visualization

```text
3.141592
     ↓
round(..., 2)

3.14
```

---

# Arithmetic Operations

```python
a = 5.5
b = 2.0

print(a + b)
print(a - b)
print(a * b)
print(a / b)
```

---

# 1️⃣3️⃣ Regular Expressions (Regex)

## What is Regex?

Regex helps identify patterns in text.

---

# Example

Sentence:

```text
The quick brown fox
```

Find:

```text
quick
```

---

# Regex Code

```python
import re

text = "The quick brown fox"

match = re.search("quick", text)

if match:
    print("Pattern Found")
```

---

# 📊 Regex Working Diagram

```text
"The quick brown fox"
        │
        ▼
 Search "quick"
        │
        ▼
 Pattern Found ✅
```

---

# 1️⃣4️⃣ Regex in DevOps

## Real-Time Use Cases

### 🔹 Log Analysis

Find all errors:

```text
ERROR: Database failed
ERROR: Timeout occurred
```

Regex can search:

```python
"ERROR"
```

---

# DevOps Use Cases

| Use Case         | Example                  |
| ---------------- | ------------------------ |
| Log Monitoring   | Find ERROR logs          |
| Security Audits  | Detect failed logins     |
| CI/CD Validation | Parse build logs         |
| Kubernetes Logs  | Extract pod errors       |
| AWS Logs         | Filter CloudWatch events |

---

# 📊 DevOps Regex Workflow

```text
Application Logs
       │
       ▼
Regular Expression
       │
       ▼
Find ERROR / WARNING / FAILED
       │
       ▼
Alert DevOps Engineer
```

---

# 1️⃣5️⃣ Important Interview Questions

# 🔥 Questions

### Q1. What is a data type?

A classification that tells Python what type of value is stored.

---

### Q2. Difference between int and float?

| int           | float           |
| ------------- | --------------- |
| Whole numbers | Decimal numbers |

---

### Q3. What is string concatenation?

Combining multiple strings.

---

### Q4. What is `split()` used for?

To divide strings into smaller parts.

---

### Q5. What are regular expressions?

Patterns used to search and match text.

---

# 1️⃣6️⃣ Practice Exercises

# ✅ Exercise 1

Convert string to uppercase.

```python
name = "devops"
```

Expected:

```text
DEVOPS
```

---

# ✅ Exercise 2

Extract username from ARN.

```text
arn:aws:iam::123456789012:user/Rahul
```

Expected:

```text
Rahul
```

---

# ✅ Exercise 3

Find length of string.

```python
text = "Python"
```

Expected:

```text
6
```

---

# ✅ Exercise 4

Round number to 3 decimals.

```python
3.14159265
```

Expected:

```text
3.142
```

---

# 🚀 Final Summary Cheat Sheet

# 📌 Strings

| Operation     | Syntax         |
| ------------- | -------------- |
| Uppercase     | `text.upper()` |
| Lowercase     | `text.lower()` |
| Split         | `text.split()` |
| Length        | `len(text)`    |
| Remove spaces | `text.strip()` |

---

# 📌 Integers

| Function | Purpose        |
| -------- | -------------- |
| `abs()`  | Absolute value |

---

# 📌 Floats

| Function  | Purpose        |
| --------- | -------------- |
| `round()` | Round decimals |

---

# 📌 Regex

| Function      | Purpose         |
| ------------- | --------------- |
| `re.search()` | Search patterns |

---

# 🎯 Key Takeaways

✅ Python supports multiple data types
✅ Python is dynamically typed
✅ Strings are heavily used in DevOps
✅ Built-in functions save time
✅ `split()` is extremely important in DevOps automation
✅ Regex is essential for log analysis
✅ Practice is the key to mastering Python

---

# 🔥 Mini Mind Map

```text
                 PYTHON DAY 2
                        │
 ┌─────────────┬────────┴─────────┬─────────────┐
 │             │                  │             │
Data Types   Strings          Numeric       Regex
 │             │                  │             │
 │      ┌──────┼──────┐      ┌────┴────┐       │
 │      │      │      │      │         │       │
String split upper len      int      float   Patterns
```

---

# ✅ Recommended Practice

Practice ALL:

* String functions
* ARN parsing
* Numeric operations
* Regex searches
* Mini DevOps automation scripts

---

# 🎉 End of Notes

These notes are designed to be:

* Interview-ready
* Beginner-friendly
* DevOps-focused
* Revision-friendly
* Practical-oriented
