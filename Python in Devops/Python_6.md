# 🐍 Day 6: Operators in Python for DevOps Engineers

# 📌 What are Operators?

An **Operator** is a symbol or keyword used to perform operations on variables and values.

Think of operators as the "action performers" in Python.

### Example

```python
a = 10
b = 5

c = a + b
```

Here:

| Symbol | Meaning             |
| ------ | ------------------- |
| `=`    | Assignment Operator |
| `+`    | Arithmetic Operator |

Without operators, Python cannot perform calculations, comparisons, or logical decisions.

---

# 🗺️ Operator Categories in Python

```text
Python Operators
│
├── Arithmetic Operators
├── Assignment Operators
├── Relational (Comparison) Operators
├── Logical Operators
├── Identity Operators
├── Membership Operators   (Covered Later)
├── Bitwise Operators      (Rarely Used in DevOps)
└── Operator Precedence    (Covered Later)
```

---

# 1️⃣ Arithmetic Operators

Arithmetic operators perform mathematical calculations.

---

## Arithmetic Operator Chart

| Operator | Meaning             | Example  | Result |
| -------- | ------------------- | -------- | ------ |
| `+`      | Addition            | `5 + 2`  | `7`    |
| `-`      | Subtraction         | `5 - 2`  | `3`    |
| `*`      | Multiplication      | `5 * 2`  | `10`   |
| `/`      | Division            | `5 / 2`  | `2.5`  |
| `//`     | Floor Division      | `5 // 2` | `2`    |
| `%`      | Modulus (Remainder) | `5 % 2`  | `1`    |
| `**`     | Exponent            | `5 ** 2` | `25`   |

---

## Arithmetic Operators Diagram

```text
           16 / 13

       1.2307692307
              │
              ▼
       Normal Division (/)
```

---

### Floor Division (`//`)

Returns only the integer portion.

```python
16 // 13
```

Output:

```python
1
```

---

### Visualization

```text
16 ÷ 13 = 1.2307

Normal Division
---------------
16 / 13 = 1.2307

Floor Division
--------------
16 // 13 = 1
```

---

## DevOps Use Case

Suppose:

```python
total_ram = 9
server_count = 4

ram_per_server = total_ram // server_count
```

Output:

```python
2
```

Useful when calculating resource allocations.

---

### Modulus Operator (`%`)

Returns the remainder after division.

```python
16 % 13
```

Output:

```python
3
```

---

### Visualization

```text
       13 ) 16
           13
         ----
            3  ← Remainder
```

---

## Quick Terminal Practice

```bash
python
```

```python
16 / 13
16 // 13
16 % 13
```

Exit Python:

```python
exit()
```

or

```bash
Ctrl + D
```

---

# 2️⃣ Assignment Operators

Assignment operators assign values to variables.

---

## Basic Assignment

```python
a = 5
```

Meaning:

```text
Store value 5 inside variable a
```

---

## Assignment Operator Chart

| Operator | Equivalent To       |
| -------- | ------------------- |
| `=`      | Assignment          |
| `+=`     | Add and Assign      |
| `-=`     | Subtract and Assign |
| `*=`     | Multiply and Assign |
| `/=`     | Divide and Assign   |

---

## Example: Addition Assignment

### Traditional Way

```python
a = 1
a = a + 2
```

Output:

```python
3
```

---

### Shortcut Way

```python
a = 1
a += 2
```

Output:

```python
3
```

---

## Visualization

```text
a = 1

a += 2

Python converts it to:

a = a + 2

a = 3
```

---

## Example

```python
a = 3

a += 2
print(a)
```

Output:

```python
5
```

---

```python
a -= 2
print(a)
```

Output:

```python
3
```

---

# 3️⃣ Identity Operators

Identity operators compare whether two objects are the same.

---

## Identity Operator Chart

| Operator | Meaning           |
| -------- | ----------------- |
| `is`     | Same Object?      |
| `is not` | Different Object? |

---

## Example

```python
a = 5
b = 5

a is b
```

Output:

```python
True
```

---

### Example

```python
a = 2
b = 3

a is b
```

Output:

```python
False
```

---

### Using `is not`

```python
a = 2
b = 3

a is not b
```

Output:

```python
True
```

---

## Identity Operator Flowchart

```text
       a is b ?
           │
     ┌─────┴─────┐
     │           │
   Yes          No
     │           │
   True       False
```

---

## DevOps Use Case

Suppose your automation script fetches:

```text
EC2 Instance A
CPU = 4
RAM = 4
```

```text
EC2 Instance B
CPU = 4
RAM = 4
```

You can compare configurations.

```python
instance_a == instance_b
```

If:

```python
True
```

Display:

```text
Instances are identical.
```

Otherwise:

```text
Instances differ.
```

---

# 4️⃣ Logical Operators

Logical operators combine multiple conditions.

These operators work primarily with **Boolean values**.

---

# Boolean Data Type

Boolean has only two values:

```python
True
False
```

---

## Important

```python
a = True
```

Boolean Value

---

```python
a = "True"
```

String Value

---

## Logical Operator Chart

| Operator | Meaning                             |
| -------- | ----------------------------------- |
| `and`    | Both conditions must be True        |
| `or`     | At least one condition must be True |
| `not`    | Reverse the result                  |

---

# Truth Table for AND

| Condition A | Condition B | Result |
| ----------- | ----------- | ------ |
| True        | True        | True   |
| True        | False       | False  |
| False       | True        | False  |
| False       | False       | False  |

---

## AND Visualization

```text
Condition 1 ----\
                 AND ----> Result
Condition 2 ----/
```

Both must be True.

---

## Example

```python
x = True
y = False

x and y
```

Output:

```python
False
```

---

## DevOps Example

Condition 1:

```text
EC2 instance exists
```

Condition 2:

```text
EC2 instance is running
```

Automation should proceed only when:

```python
instance_exists and instance_running
```

Output:

```python
True
```

Only if both conditions are satisfied.

---

# 5️⃣ Relational (Comparison) Operators

Relational operators compare values.

---

## Comparison Operator Chart

| Operator | Meaning               |
| -------- | --------------------- |
| `>`      | Greater Than          |
| `<`      | Less Than             |
| `>=`     | Greater Than or Equal |
| `<=`     | Less Than or Equal    |
| `==`     | Equal To              |
| `!=`     | Not Equal To          |

---

## Example

```python
a = 2
b = 3

a > b
```

Output:

```python
False
```

---

```python
a < b
```

Output:

```python
True
```

---

## Important Interview Question

### Assignment vs Comparison

---

### Assignment Operator

```python
a = b
```

Meaning:

```text
Copy value of b into a
```

---

### Comparison Operator

```python
a == b
```

Meaning:

```text
Check whether a and b are equal
```

---

## Visual Representation

```text
a = b

Assignment
──────────
Copies value


a == b

Comparison
──────────
Checks equality
```

---

## DevOps Example

Check CPU utilization:

```python
cpu_usage = 80

cpu_usage > 75
```

Output:

```python
True
```

Automation can trigger scaling actions.

---

# 🔥 Quick Summary Table

| Operator Type | Purpose                   |
| ------------- | ------------------------- |
| Arithmetic    | Mathematical calculations |
| Assignment    | Assign values             |
| Relational    | Compare values            |
| Logical       | Combine conditions        |
| Identity      | Check object identity     |

---

# 🧠 DevOps Relevance

| Scenario                        | Operator Used |
| ------------------------------- | ------------- |
| Resource Calculation            | Arithmetic    |
| Variable Updates                | Assignment    |
| CPU/RAM Threshold Checks        | Relational    |
| Conditional Automation          | Logical       |
| Object/Configuration Validation | Identity      |

---

# 🎯 Key Takeaways

✅ Operators are fundamental building blocks of Python.

✅ Every calculation, comparison, and decision uses operators.

✅ Arithmetic operators help in resource calculations.

✅ Assignment operators simplify variable updates.

✅ Relational operators compare values.

✅ Logical operators combine multiple conditions.

✅ Identity operators help validate objects and configurations.

✅ These concepts become extremely important while automating cloud infrastructure, CI/CD pipelines, monitoring scripts, and Kubernetes operations.

---

# 📚 Practice Tasks

### Complete

* Arithmetic Operator Exercises
* Assignment Operator Exercises
* Identity Operator Exercises
* Logical Operator Exercises
* Relational Operator Exercises

### Skip for Now

❌ Task 5
❌ Task 6

These require understanding of sequence data types, which will be covered later in the course.

---

## Final Revision Mind Map

```text
OPERATORS
│
├── Arithmetic
│   ├── +
│   ├── -
│   ├── *
│   ├── /
│   ├── //
│   └── %
│
├── Assignment
│   ├── =
│   ├── +=
│   └── -=
│
├── Relational
│   ├── >
│   ├── <
│   ├── >=
│   ├── <=
│   └── ==
│
├── Logical
│   ├── and
│   ├── or
│   └── not
│
└── Identity
    ├── is
    └── is not
```

**Master these five operator categories thoroughly before moving to Python conditionals and loops.**
