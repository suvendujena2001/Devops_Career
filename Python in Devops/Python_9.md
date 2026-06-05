# Day 9: Loops in Python

## For Loop, While Loop, Loop Control Statements & DevOps Use Cases

---

# 📚 Learning Objectives

By the end of this lesson, you will understand:

* What loops are and why they are needed.
* Different types of loops in Python.
* How `for` loops work.
* How `while` loops work.
* The difference between `for` and `while`.
* Loop Control Statements:

  * `break`
  * `continue`
* Real-world DevOps use cases of loops.

---

# 1. What are Loops?

A **Loop** is a programming construct that allows us to execute a block of code repeatedly.

## Definition

> **Loops are used to perform repetitive execution of a block of code.**

Instead of writing the same code multiple times, we can use loops to automate repetition.

---

## Without Loops

```python
print("Abhishek")
print("Abhishek")
print("Abhishek")
print("Abhishek")
print("Abhishek")
```

### Problem

Imagine printing:

* 100 times ❌
* 10,000 times ❌
* 1,000,000 times ❌

This becomes impossible to manage manually.

---

## With Loops

```python
for i in range(5):
    print("Abhishek")
```

### Output

```text
Abhishek
Abhishek
Abhishek
Abhishek
Abhishek
```

---

# Why Loops Exist?

```text
Without Loop
─────────────

print()
print()
print()
print()
print()
print()
print()
...

Problem:
Repeated Code
More Maintenance
Poor Readability


With Loop
─────────

for i in range(1000):
    print()

Advantages:
✓ Cleaner Code
✓ Less Effort
✓ Better Readability
✓ Easy Maintenance
```

---

# 2. Types of Loops in Python

Python provides two primary loops:

```text
Loops
│
├── For Loop
│
└── While Loop
```

---

# 3. For Loop

A **For Loop** is used when we know beforehand how many times the code should execute.

---

## When to Use For Loop?

Use a `for` loop when:

✅ Number of iterations is known.

Examples:

* Print numbers 1 to 10.
* Iterate over a list.
* Iterate over servers.
* Iterate over databases.

---

# For Loop Syntax

```python
for variable in sequence:
    # code block
```

---

## Diagram

```text
            Sequence
        [1,2,3,4,5]

                │
                ▼

      for item in sequence

                │
                ▼

      item = 1 → Execute Block

      item = 2 → Execute Block

      item = 3 → Execute Block

      item = 4 → Execute Block

      item = 5 → Execute Block

                │
                ▼

              End
```

---

# Understanding the Syntax

```python
for i in range(10):
    print(i)
```

---

## Components

```text
for     i      in     range(10)
│       │      │          │
│       │      │          └── Sequence
│       │      │
│       │      └────────── Keyword
│       │
│       └──────────────── Variable
│
└──────────────────────── Keyword
```

---

# Understanding `range()`

`range()` generates a sequence of numbers.

```python
range(10)
```

Returns:

```text
0,1,2,3,4,5,6,7,8,9
```

---

## Visualization

```text
range(10)

┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ 0 │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ 7 │ 8 │ 9 │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

---

# Example: Printing Numbers

```python
for i in range(10):
    print(i)
```

### Output

```text
0
1
2
3
4
5
6
7
8
9
```

---

# How `i` Changes

```text
Iteration 1 → i = 0

Iteration 2 → i = 1

Iteration 3 → i = 2

Iteration 4 → i = 3

...

Iteration 10 → i = 9
```

---

# Example: Printing a Name

```python
for i in range(5):
    print("Abhishek")
```

Output:

```text
Abhishek
Abhishek
Abhishek
Abhishek
Abhishek
```

---

# For Loop with Lists

## Example

```python
colors = ["yellow", "green", "blue"]

for color in colors:
    print(color)
```

### Output

```text
yellow
green
blue
```

---

## Internal Working

```text
colors
│
├── yellow
├── green
└── blue

      ↓

for color in colors

Iteration 1:
color = yellow

Iteration 2:
color = green

Iteration 3:
color = blue
```

---

# For Loop with Tuple

```python
fruits = ("apple", "banana", "cherry")

for fruit in fruits:
    print(fruit)
```

Output:

```text
apple
banana
cherry
```

---

# Common Sequences Used in For Loops

| Sequence Type | Example         |
| ------------- | --------------- |
| Range         | `range(10)`     |
| List          | `["a","b","c"]` |
| Tuple         | `("a","b","c")` |

---

# 4. While Loop

A **While Loop** executes as long as a condition remains true.

---

## When to Use While Loop?

Use a while loop when:

✅ Number of executions is unknown.

Examples:

* Waiting for deployment.
* Waiting for pod readiness.
* Monitoring logs.
* Checking database synchronization.

---

# While Loop Syntax

```python
while condition:
    # code block
```

---

# While Loop Flow

```text
        Start
          │
          ▼
   Check Condition
          │
     ┌────┴────┐
     │         │
   True      False
     │         │
     ▼         ▼
 Execute      End
 Block
     │
     └─────────┘
```

---

# Example

```python
count = 1

while count <= 5:
    print(count)
    count += 1
```

Output:

```text
1
2
3
4
5
```

---

# For Loop vs While Loop

| Feature                      | For Loop | While Loop |
| ---------------------------- | -------- | ---------- |
| Iterations Known?            | Yes      | No         |
| Uses Sequence?               | Yes      | No         |
| Condition-Based?             | No       | Yes        |
| Preferred For Fixed Count?   | Yes      | No         |
| Preferred For Dynamic Count? | No       | Yes        |

---

# Visual Comparison

```text
FOR LOOP

Known Iterations

1 → 2 → 3 → 4 → 5

──────────────────

WHILE LOOP

Condition Based

While Condition=True
       ↓
Execute
       ↓
Check Again
```

---

# 5. DevOps Use Cases

---

# For Loop Use Cases

Used when resources are known beforehand.

---

## Example 1: Deploying to Multiple Environments

```python
environments = ["dev", "staging", "prod"]

for env in environments:
    deploy(env)
```

---

### Visualization

```text
environments

┌───────┐
│ dev   │
├───────┤
│stage  │
├───────┤
│ prod  │
└───────┘

      ↓

Deploy to each environment
```

---

## Example 2: Backup Multiple Databases

```python
databases = ["db1", "db2", "db3"]

for db in databases:
    backup(db)
```

---

## Example 3: Restart Multiple Servers

```python
servers = ["server1", "server2", "server3"]

for server in servers:
    restart(server)
```

---

# While Loop Use Cases

Used when resources or conditions are dynamic.

---

## Example 1: Wait for Kubernetes Deployment

```python
while deployment_not_ready:
    print("Waiting...")
```

---

### Flow

```text
Deployment Ready?

      NO
       │
       ▼

Waiting...

       │
       ▼

Check Again

       │
       ▼

      YES
       │
       ▼

Proceed
```

---

## Example 2: Monitor Pods

```python
while pods_not_ready:
    print("Waiting for pods")
```

---

## Example 3: Log Monitoring

```python
while errors_exist:
    analyze_logs()
```

---

## Example 4: Database Replication

```python
while replication_not_complete:
    wait()
```

---

## Example 5: File Cleanup

```python
while files_exist:
    delete_file()
```

---

# 6. Loop Control Statements

Loop execution can be controlled using:

```text
Loop Controls
│
├── break
│
└── continue
```

---

# Break Statement

Used to terminate the loop immediately.

---

## Syntax

```python
break
```

---

## Example

```python
for number in range(1, 6):

    if number == 3:
        break

    print(number)
```

---

### Output

```text
1
2
```

---

## Flow Diagram

```text
1 → Print

2 → Print

3 → Break

STOP
```

---

# Continue Statement

Used to skip current iteration and continue with the next one.

---

## Syntax

```python
continue
```

---

## Example

```python
for number in range(1, 6):

    if number == 3:
        continue

    print(number)
```

---

### Output

```text
1
2
4
5
```

---

## Flow Diagram

```text
1 → Print

2 → Print

3 → Skip

4 → Print

5 → Print
```

---

# Break vs Continue

| Feature              | Break     | Continue                  |
| -------------------- | --------- | ------------------------- |
| Stops Loop?          | Yes       | No                        |
| Skips Iteration?     | No        | Yes                       |
| Continues Execution? | No        | Yes                       |
| Use Case             | Exit Loop | Ignore Specific Iteration |

---

## Visual Comparison

```text
BREAK

1 → Print
2 → Print
3 → STOP

Output:
1 2


CONTINUE

1 → Print
2 → Print
3 → Skip
4 → Print
5 → Print

Output:
1 2 4 5
```

---

# Interview Questions

### Q1. What is a loop?

A loop is used to repeatedly execute a block of code.

---

### Q2. What are the types of loops in Python?

1. For Loop
2. While Loop

---

### Q3. When should we use a For Loop?

When the number of iterations is known beforehand.

---

### Q4. When should we use a While Loop?

When execution depends on a condition and the number of iterations is unknown.

---

### Q5. What does `range(10)` return?

A sequence of numbers from:

```text
0 to 9
```

---

### Q6. Difference between Break and Continue?

| Break                 | Continue                |
| --------------------- | ----------------------- |
| Stops loop completely | Skips current iteration |
| Exits loop            | Continues loop          |

---

# Quick Revision Sheet

```text
LOOPS
│
├── For Loop
│   │
│   ├── Known Iterations
│   ├── Uses Sequence
│   ├── range()
│   ├── list
│   └── tuple
│
├── While Loop
│   │
│   ├── Unknown Iterations
│   ├── Condition Based
│   └── Dynamic Execution
│
└── Loop Controls
    │
    ├── break
    │   └── Stop Loop
    │
    └── continue
        └── Skip Iteration
```

---

# Key Takeaways

✅ Loops eliminate repetitive code.

✅ Python provides two loops:

* `for`
* `while`

✅ Use **for loop** when iteration count is known.

✅ Use **while loop** when execution depends on a condition.

✅ `range()` generates sequences of numbers.

✅ `break` stops a loop immediately.

✅ `continue` skips the current iteration.

✅ DevOps engineers frequently use loops for:

* Server management
* Deployments
* Monitoring
* Log analysis
* Kubernetes automation
* Database operations

---

> **Golden Rule:**
> **If you know how many times to execute → Use `for` loop.**
> **If execution depends on a changing condition → Use `while` loop.** 🚀
