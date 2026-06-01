# Complete YAML Course Notes (Beginner → Advanced for DevOps)

> **YAML is one of the most important technologies in DevOps.**
>
> It is extensively used in:
>
> * Docker Compose
> * Kubernetes
> * CI/CD Pipelines
> * Infrastructure as Code (IaC)
> * Cloud Configurations
> * Monitoring Tools
> * Logging Systems
>
> Understanding YAML is like learning the **ABCD of DevOps**.

---

# Table of Contents

1. Introduction to YAML
2. Why YAML Exists
3. Data Serialization
4. YAML vs JSON vs XML
5. Benefits of YAML
6. YAML Syntax
7. YAML Data Types
8. Collections (Lists & Maps)
9. Advanced YAML Structures
10. Anchors & Aliases
11. Real World YAML Examples
12. YAML in DevOps
13. Useful YAML Tools
14. Summary Cheat Sheet

---

# 1. Introduction to YAML

## What is YAML?

**YAML** stands for:

> **YAML Ain't Markup Language**

Originally:

> Yet Another Markup Language

But later renamed because YAML is **not actually a markup language**.

---

## What is YAML Used For?

YAML is a:

> **Human-readable data serialization language**

Used to:

* Store configuration data
* Exchange data between applications
* Represent objects in a text format

---

### YAML Can Store

✅ Data

* Strings
* Numbers
* Lists
* Maps
* Objects

❌ Commands

YAML is **not a programming language**.

You cannot write:

```python
if x > 5:
```

or

```java
for(...)
```

inside YAML.

---

# 2. Why YAML Exists

Imagine a Student Object:

```text
Student
│
├── Name
├── Roll Number
└── Marks
```

This object exists in memory.

What if we want to:

* Send it to a mobile app
* Store it in a database
* Send it to a web application
* Use it in an ML model

We need a universal format.

YAML provides that format.

---

# 3. Data Serialization

## What is Serialization?

Serialization is:

> Converting an object into a format that can be stored or transmitted.

---

### Serialization Flow

```text
Object in Memory
        │
        ▼
  Serialization
        │
        ▼
 YAML / JSON / XML
        │
        ▼
 File / Database / Network
```

---

## Deserialization

Reverse process.

```text
YAML File
    │
    ▼
Deserialization
    │
    ▼
Object in Memory
```

---

### Easy Analogy

```text
Object
  ↓
Packaging
  ↓
YAML File
  ↓
Unpackaging
  ↓
Object
```

---

## Popular Serialization Formats

| Format | Purpose        |
| ------ | -------------- |
| YAML   | Human-friendly |
| JSON   | Web APIs       |
| XML    | Legacy systems |

---

# 4. YAML vs JSON vs XML

---

## Example Data

```text
School
├── Name = DPS
├── Principal = Someone
└── Student
    ├── Roll = 23
    ├── Name = Kunal
    └── Marks = 94
```

---

# XML Representation

```xml
<school>
    <name>DPS</name>

    <principal>Someone</principal>

    <student>
        <roll>23</roll>
        <name>Kunal</name>
        <marks>94</marks>
    </student>
</school>
```

---

## JSON Representation

```json
{
  "school": {
    "name": "DPS",
    "principal": "Someone",
    "students": [
      {
        "roll": 23,
        "name": "Kunal",
        "marks": 94
      }
    ]
  }
}
```

---

## YAML Representation

```yaml
school:
  name: DPS
  principal: Someone

  students:
    - roll: 23
      name: Kunal
      marks: 94
```

---

## Comparison Chart

| Feature           | YAML      | JSON     | XML  |
| ----------------- | --------- | -------- | ---- |
| Human Readable    | ⭐⭐⭐⭐⭐     | ⭐⭐⭐      | ⭐⭐   |
| Easy to Write     | ⭐⭐⭐⭐⭐     | ⭐⭐⭐⭐     | ⭐⭐   |
| Verbosity         | Low       | Medium   | High |
| Supports Comments | Yes       | No       | No   |
| Used in DevOps    | Extremely | Moderate | Rare |

---

# 5. Benefits of YAML

## 1. Human Readable

```yaml
name: Kunal
age: 25
```

Very easy to understand.

---

## 2. Less Verbose

No unnecessary symbols.

```yaml
name: Kunal
```

instead of

```json
{
  "name": "Kunal"
}
```

---

## 3. Easy Conversion

Can be converted to:

* JSON
* XML

easily.

---

## 4. Widely Supported

Supported by:

* Python
* Java
* JavaScript
* Go
* Kubernetes
* Docker

---

## 5. Powerful for Complex Data

Supports:

* Nested objects
* Arrays
* Maps
* Reusable structures

---

# 6. YAML Syntax

---

## File Extensions

```text
.yaml
.yml
```

Both are valid.

---

## Key Value Pair

```yaml
fruit: Apple
```

Diagram:

```text
fruit ───► Apple
```

---

## Multiple Key Values

```yaml
name: Kunal
age: 25
city: Delhi
```

---

# Comments

```yaml
# This is a comment
name: Kunal
```

---

### Multi-line Comments

❌ Not supported

Instead:

```yaml
# line 1
# line 2
# line 3
```

---

## Case Sensitive

```yaml
Apple
```

and

```yaml
apple
```

are different.

---

# 7. YAML Data Types

---

# Strings

### Method 1

```yaml
name: Kunal
```

### Method 2

```yaml
name: "Kunal"
```

### Method 3

```yaml
name: 'Kunal'
```

---

## Multi-line String (Preserve Lines)

```yaml
bio: |
  Hello
  My name is Kunal
  I teach DevOps
```

Output:

```text
Hello
My name is Kunal
I teach DevOps
```

---

## Multi-line String (Single Line Output)

```yaml
message: >
  This is line 1
  This is line 2
```

Output:

```text
This is line 1 This is line 2
```

---

## Integers

```yaml
age: 25
```

---

## Float

```yaml
marks: 89.5
```

---

## Boolean

```yaml
isStudent: true
isTeacher: false
```

Also accepted:

```yaml
yes
no
TRUE
FALSE
```

---

## Null

```yaml
surname: null
```

or

```yaml
surname: ~
```

---

## Date

```yaml
date: 2025-01-15
```

---

## Timestamp

```yaml
timestamp: 2025-01-15T14:30:00Z
```

---

# 8. Collections

---

# Lists (Sequences)

```yaml
fruits:
  - Apple
  - Mango
  - Banana
```

Diagram:

```text
fruits
 │
 ├── Apple
 ├── Mango
 └── Banana
```

---

## Flow Style List

```yaml
fruits: [Apple, Mango, Banana]
```

---

# Maps (Dictionaries)

```yaml
student:
  name: Kunal
  age: 25
```

Diagram:

```text
student
 │
 ├── name → Kunal
 └── age  → 25
```

---

## Nested Map

```yaml
employee:
  name: Kunal

  details:
    age: 25
    role: Engineer
```

Diagram:

```text
employee
 │
 ├── name
 │
 └── details
      ├── age
      └── role
```

---

# 9. Advanced YAML Structures

---

## Nested Lists

```yaml
students:
  -
    - Kunal
    - Rahul

  -
    - Priya
    - Neha
```

---

## Sparse Sequence

```yaml
students:
  - Kunal
  -
  - Rahul
```

---

## Sets

Unique values only.

```yaml
names: !!set
  ? Kunal
  ? Rahul
  ? Priya
```

---

## Ordered Map

```yaml
people:
  - Kunal:
      age: 25

  - Rahul:
      age: 30
```

---

# 10. Anchors & Aliases

One of YAML's most powerful features.

---

## Problem

Repeated configuration:

```yaml
fruit: Mango
dislikes: Grapes
```

used hundreds of times.

---

## Solution: Anchors

---

### Create Anchor

```yaml
likes: &likes

  favouriteFruit: Mango
  dislikes: Grapes
```

---

### Reuse Anchor

```yaml
person1:
  <<: *likes

person2:
  <<: *likes
```

---

## Visual Representation

```text
Anchor
  │
  ▼

likes
 ├── Mango
 └── Grapes

      │
      │ reused
      ▼

person1
person2
person3
```

---

## Override Values

```yaml
person2:
  <<: *likes

  dislikes: Berries
```

Result:

```yaml
person2:
  favouriteFruit: Mango
  dislikes: Berries
```

---

# 11. Real World YAML Example

## Kubernetes Deployment Style

```yaml
apiVersion: apps/v1

kind: Deployment

metadata:
  name: nginx

spec:
  replicas: 3

  template:
    spec:
      containers:
      - name: nginx
        image: nginx
```

This structure is exactly why YAML is heavily used in DevOps.

---

# 12. YAML in DevOps

YAML appears almost everywhere.

---

## Docker Compose

```yaml
services:
  web:
    image: nginx
```

---

## Kubernetes

```yaml
kind: Pod
```

---

## GitHub Actions

```yaml
name: CI Pipeline
```

---

## GitLab CI/CD

```yaml
stages:
  - build
  - test
  - deploy
```

---

## Ansible

```yaml
tasks:
  - name: Install nginx
```

---

### DevOps YAML Ecosystem

```text
            YAML
              │
    ┌─────────┼─────────┐
    │         │         │
 Docker   Kubernetes  CI/CD
    │         │         │
Compose   Deployments  Pipelines
```

---

# 13. Useful YAML Tools

---

## 1. YAML Validators

Used to:

* Check syntax
* Detect indentation errors
* Validate schemas

Example:

```yaml
name:
 age: 25
```

❌ Invalid indentation

---

## 2. Datree

Purpose:

* Kubernetes YAML validation
* Best practice checks
* Schema validation

Benefits:

* Detects configuration mistakes
* Prevents deployment failures

---

## 3. Monokle

Purpose:

* Visual Kubernetes YAML management
* Understand large manifests

Useful for:

* Large Kubernetes projects
* Multi-file deployments

---

## 4. Lens IDE

Purpose:

* GUI for Kubernetes

Benefits:

* Creates YAML automatically
* Visual cluster management
* Monitor CPU/Memory
* Manage resources without manually writing YAML

---

### Lens Workflow

```text
GUI
 │
 ▼
Lens
 │
 ▼
Generates YAML
 │
 ▼
Deploys to Kubernetes
```

---

# 14. YAML Cheat Sheet

## Key Value

```yaml
name: Kunal
```

---

## List

```yaml
fruits:
  - Apple
  - Mango
```

---

## Map

```yaml
person:
  age: 25
```

---

## Nested Map

```yaml
person:
  details:
    age: 25
```

---

## String

```yaml
name: "Kunal"
```

---

## Integer

```yaml
age: 25
```

---

## Float

```yaml
marks: 89.5
```

---

## Boolean

```yaml
active: true
```

---

## Null

```yaml
value: null
```

---

## Anchor

```yaml
defaults: &defaults
  role: developer

user:
  <<: *defaults
```

---

# Final Takeaways

### YAML is:

* Human-readable
* Lightweight
* Data serialization language
* Indentation-sensitive
* Extensively used in DevOps

### Most Important Concepts

✅ Key-Value Pairs
✅ Lists (Sequences)
✅ Maps (Dictionaries)
✅ Nested Structures
✅ Anchors & Aliases
✅ Serialization & Deserialization
✅ Kubernetes YAML Files

### Remember

> **If Kubernetes is the heart of modern DevOps, YAML is its language.**
>
> Mastering YAML early makes Docker, Kubernetes, CI/CD, Ansible, and Cloud-Native technologies significantly easier to learn. 🚀
