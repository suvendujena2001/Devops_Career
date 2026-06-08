# Day 15 - Python for DevOps (Part-2)

# GitHub ↔ Jira Integration using Python Flask

---

# 🎯 Project Overview

In modern software development, developers often receive hundreds of GitHub issues reported by:

* QA Engineers
* Developers from other teams
* End users
* Internal stakeholders

Not every issue is valid. Developers first **triage** issues and decide whether they should be worked upon.

Once a developer accepts an issue, they usually need to:

1. Create a Jira ticket.
2. Track the work against that ticket.
3. Maintain visibility for managers and stakeholders.

Doing this manually becomes repetitive and time-consuming.

---

# 🚀 Problem Statement

Whenever a developer comments:

```text
/sljira
```

on a GitHub Issue,

the system should automatically:

1. Read the GitHub issue details.
2. Send the details to a Python application.
3. Create a Jira ticket automatically.
4. Add the ticket to the Jira backlog.

---

# 🏗️ High-Level Architecture

```text
+----------------+
|   Developer    |
+--------+-------+
         |
         | Comment "/sljira"
         v

+----------------+
|    GitHub      |
|    Issues      |
+--------+-------+
         |
         | Webhook
         v

+------------------------+
| Python Flask API       |
| (Hosted on EC2)        |
+-----------+------------+
            |
            | Jira REST API
            v

+----------------+
|      Jira      |
| Project Board  |
+----------------+
```

---

# 📚 Recap of Day 14

In Day 14, the following concepts were covered:

## ✅ Jira Setup

* Jira installation
* Jira project creation
* API token generation

---

## ✅ Jira API Understanding

Learned how to:

* Authenticate to Jira
* Use Jira REST APIs
* Create Jira tickets programmatically

---

## ✅ Python Script

Created a Python script that:

```text
Python Script
      ↓
Jira API
      ↓
Create Ticket
```

However, this script was not exposed as an API.

---

# ❓ Why Convert Python Script into an API?

GitHub cannot:

❌ Login into your EC2 instance

❌ Execute your Python script manually

GitHub can only communicate through:

1. APIs
2. CLI

Since CLI access isn't possible, we need:

```text
Python Script
      ↓
Convert to API
      ↓
GitHub can invoke it
```

---

# 🧠 Complete Workflow

```text
Developer
    |
    | Comment on Issue
    v

GitHub
    |
    | Sends JSON Payload
    v

Python Flask API
    |
    | Extract Information
    v

Jira REST API
    |
    | Create Ticket
    v

Jira Board
```

---

# 📋 Project Implementation Steps

## Step 1

Understand APIs

## Step 2

Convert Python Script to Flask API

## Step 3

Deploy Flask API

## Step 4

Configure GitHub Webhook

## Step 5

Add Conditional Logic

```python
if comment == "/sljira":
    create_jira_ticket()
```

---

# Understanding APIs

---

## What is an API?

API stands for:

```text
Application Programming Interface
```

An API allows applications to communicate with each other programmatically.

---

# User Interface vs API

## User Interface (UI)

Humans interact through:

```text
Browser
Mouse
Keyboard
```

Example:

```text
Book Movie Ticket
```

You manually:

* Enter Name
* Enter Age
* Select Movie
* Pay

---

## Application Interface (API)

Applications interact through APIs.

Example:

```text
Python Script
      ↓
GitHub API
```

No browser interaction needed.

Everything happens programmatically.

---

# Example

GitHub UI:

```text
https://github.com
```

GitHub API:

```text
https://api.github.com
```

---

# HTTP Request Methods

Almost every API uses HTTP.

The four major request types are:

---

## 1. GET

Used to retrieve data.

Example:

```text
Get Ticket Details
```

```http
GET /ticket
```

---

## 2. POST

Used to create data.

Example:

```text
Create Ticket
```

```http
POST /ticket
```

---

## 3. PUT

Used to update data.

Example:

```text
Modify Ticket
```

```http
PUT /ticket
```

---

## 4. DELETE

Used to remove data.

Example:

```text
Cancel Ticket
```

```http
DELETE /ticket
```

---

# Which Request Type Do We Need?

GitHub sends data to us.

Therefore:

```text
GitHub
   ↓
POST
   ↓
Flask API
```

We need:

```python
methods=["POST"]
```

---

# Flask Framework

Flask is a lightweight Python web framework used to create APIs.

---

# Installing Flask

```bash
pip install flask
```

or

```bash
pip3 install flask
```

---

# Creating First Flask Application

## hello_world.py

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World"

app.run(
    host="0.0.0.0",
    port=5000
)
```

---

# Understanding Each Line

---

## Import Flask

```python
from flask import Flask
```

Imports only the Flask module.

---

## Create Application Instance

```python
app = Flask(__name__)
```

Creates the Flask application object.

This line appears in almost every Flask application.

---

## Define Function

```python
def hello():
```

Function to execute when endpoint is called.

---

## Return Response

```python
return "Hello World"
```

Response sent to the client.

---

# Flask Decorators

```python
@app.route("/")
```

This is called a decorator.

---

## What is a Decorator?

A decorator performs an action before function execution.

---

### General Concept

```python
@decorator
def function():
    pass
```

Decorator executes first.

Function executes later.

---

## Real World Example

```text
User
   ↓
Authentication Check
   ↓
Function Execution
```

Authentication can be implemented using decorators.

---

## Flask Example

```python
@app.route("/")
```

Means:

```text
Execute hello()
ONLY when URL = /
```

---

# URL Routing

## Valid URL

```text
http://server:5000/
```

Output:

```text
Hello World
```

---

## Invalid URL

```text
http://server:5000/abc
```

Output:

```text
404 Not Found
```

Flask automatically handles this.

---

# Running Flask

```python
app.run(
    host="0.0.0.0",
    port=5000
)
```

---

## Meaning

### Host

```text
0.0.0.0
```

Accept connections from any interface.

---

### Port

```text
5000
```

Default Flask port.

---

# Converting Jira Script to Flask API

Previously:

```text
create_jira.py
```

was a normal Python script.

---

## New Structure

```python
from flask import Flask

app = Flask(__name__)

@app.route(
    "/createjira",
    methods=["POST"]
)
def create_jira():

    # Jira logic from Day 14

    return "Jira Ticket Created"

app.run(
    host="0.0.0.0",
    port=5000
)
```

---

# Flow After Conversion

```text
GitHub
   |
   | POST Request
   |
   v

/createjira

   |
   v

Python Function

   |
   v

Jira API

   |
   v

Create Ticket
```

---

# GitHub Webhooks

---

## What is a Webhook?

Webhook is an event-based notification system.

Whenever an event occurs:

```text
GitHub
   ↓
Sends HTTP Request
   ↓
Flask API
```

---

# Webhook Configuration

Navigate to:

```text
Repository
   ↓
Settings
   ↓
Webhooks
```

---

# Payload URL

Example:

```text
http://EC2-IP:5000/createjira
```

---

# Content Type

```text
application/json
```

---

# Select Event Type

Choose:

```text
Issue Comments
```

---

# Why Issue Comments?

Because we want:

```text
Developer Comment
      ↓
Trigger Automation
```

---

# GitHub Payload

Whenever a comment is added, GitHub sends:

```json
{
  "issue": {
      ...
  },
  "comment": {
      "body": "/sljira"
  },
  "user": {
      ...
  }
}
```

---

# Important Observation

GitHub sends ALL issue information.

Examples:

* Issue Title
* Issue Description
* User
* Labels
* Comment Body
* Repository Details

Everything arrives as JSON.

---

# JSON Flow

```text
GitHub UI
      ↓

JSON Payload
      ↓

Flask API
      ↓

Python Dictionary
      ↓

Extract Values
```

---

# Assignment (Important)

Current behavior:

```text
Comment anything
      ↓
Jira Ticket Created
```

Wrong behavior.

---

# Desired Behavior

Only create ticket when:

```text
/sljira
```

is commented.

---

# Solution

Read comment body from payload.

Example:

```python
comment = payload["comment"]["body"]

if comment == "/sljira":
    create_ticket()
else:
    print("Ignoring Comment")
```

---

# Final Production Architecture

```text
+-------------+
| Developer   |
+------+------+
       |
       | /sljira
       |
       v

+-------------+
| GitHub      |
+------+------+
       |
       | Webhook
       |
       v

+-------------------+
| Flask API         |
| EC2 Instance      |
+---------+---------+
          |
          | Jira REST API
          |
          v

+-------------------+
| Jira Project      |
| Backlog           |
+-------------------+
```

---

# Important Real-World Note

In this tutorial:

```text
Flask Development Server
```

was used.

---

In Production:

```text
Flask
   ↓
WSGI
   ↓
Apache / Nginx / Gunicorn
   ↓
Production Deployment
```

Organizations generally deploy Flask applications behind:

* Apache
* Nginx
* Gunicorn
* uWSGI

rather than using Flask's development server.

---

# Key Takeaways

✅ Understand API fundamentals

✅ Learn HTTP methods (GET, POST, PUT, DELETE)

✅ Build APIs using Flask

✅ Understand Flask decorators

✅ Convert Python scripts into APIs

✅ Configure GitHub Webhooks

✅ Receive GitHub JSON payloads

✅ Integrate GitHub with Jira

✅ Automate Jira ticket creation

✅ Implement event-driven DevOps workflows

---

# Interview Questions

### 1. What is a Webhook?

A webhook is an event-driven HTTP callback used to notify another application when an event occurs.

---

### 2. Why was Flask used?

Flask is a lightweight Python framework used for building REST APIs quickly.

---

### 3. Why use POST instead of GET?

GitHub sends data to our application, so POST is required.

---

### 4. What is a Flask Decorator?

A decorator performs an action before function execution. Flask uses decorators to map URLs to functions.

---

### 5. Why convert a Python script into an API?

External systems like GitHub can invoke APIs but cannot directly execute scripts on servers.

---

### 6. What does `@app.route()` do?

It maps a URL endpoint to a Python function.

---

### 7. What data does GitHub send through Webhooks?

GitHub sends event information as a JSON payload containing issue details, comments, repository details, and user information.

---

# End Result

A fully automated DevOps workflow where:

```text
GitHub Comment
       ↓
Webhook Trigger
       ↓
Flask API
       ↓
Jira REST API
       ↓
Automatic Jira Ticket Creation
```

This eliminates manual ticket creation and significantly improves developer productivity and traceability.
