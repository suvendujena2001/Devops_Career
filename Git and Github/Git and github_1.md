# Day 9 Notes — Git & GitHub Fundamentals

## DevOps Zero to Hero — Complete Study Notes

---

# Table of Contents

1. Introduction
2. What is Version Control System (VCS)?
3. Problems Solved by Version Control
4. Real-World Example
5. Versioning Explained
6. Types of Version Control Systems
7. Centralized vs Distributed VCS
8. Why Git Became Popular
9. What is Fork?
10. Git vs GitHub
11. Git Architecture
12. Installing Git
13. Creating a Git Repository
14. Important Git Commands
15. Git Lifecycle
16. Understanding `.git` Folder
17. Tracking Files
18. Git Diff
19. Git Commits
20. Git Log
21. Git Reset
22. Sharing Code with GitHub
23. Creating GitHub Repository
24. Common Interview Questions
25. Key Takeaways
26. Cheat Sheet

---

# 1. Introduction

Git and GitHub are among the most important tools in DevOps, Software Engineering, and Cloud Engineering.

Before understanding Git, we must understand:

# ➜ Version Control System (VCS)

Git is fundamentally a **Version Control System**.

---

# 2. What is Version Control System (VCS)?

A **Version Control System** is a system that helps developers:

* Track code changes
* Share code with teammates
* Maintain different versions of code
* Restore previous versions if needed

---

# 3. Problems Solved by Version Control

Version Control solves **2 major problems**:

| Problem      | Description                              |
| ------------ | ---------------------------------------- |
| Code Sharing | Multiple developers working together     |
| Versioning   | Keeping track of historical code changes |

---

# 4. Real-World Example

Imagine two developers working on a Calculator Application.

```text
Developer 1 → Addition Function
Developer 2 → Subtraction Function
```

Eventually both changes must combine into one application.

---

## Without Version Control

Problems:

* Sharing files manually
* Emailing files
* Confusion over latest version
* File overwriting
* Dependency issues

---

## With Version Control

Advantages:

* Centralized tracking
* Easy collaboration
* Controlled modifications
* Change history available

---

# 5. Versioning Explained

Suppose requirements change over time:

```text
Version 1 → Addition of 2 numbers
Version 2 → Addition of 3 numbers
Version 3 → Addition of 4 numbers
```

Later management says:

> "Go back to Version 1"

Without version control:

❌ Difficult
❌ Time consuming
❌ Risky

With Git:

✅ Easy rollback
✅ Historical tracking
✅ Restore any version anytime

---

# 6. Types of Version Control Systems

There are mainly two types:

```text
1. Centralized Version Control System (CVCS)
2. Distributed Version Control System (DVCS)
```

---

# 7. Centralized vs Distributed VCS

# A. Centralized Version Control System

Examples:

* SVN
* CVS

---

## Architecture

```text
          Central Server (SVN/CVS)
                 /      \
                /        \
            Dev1        Dev2
```

All communication happens through ONE server.

---

## Problems in CVCS

### Single Point of Failure

If central server goes down:

```text
Dev1 ❌ cannot communicate ❌ Dev2
```

---

## Characteristics

| Feature               | CVCS      |
| --------------------- | --------- |
| Single central server | Yes       |
| Offline work          | Difficult |
| Backup copies         | No        |
| Failure risk          | High      |

---

# B. Distributed Version Control System

Example:

# ➜ Git

---

## Architecture

```text
          Main Git Repository
             /          \
            /            \
      Dev1 Copy       Dev2 Copy
```

Each developer has a FULL copy of repository.

---

## Advantages

| Feature            | DVCS |
| ------------------ | ---- |
| Multiple copies    | Yes  |
| Offline work       | Yes  |
| Better reliability | Yes  |
| Fast operations    | Yes  |
| No single failure  | Yes  |

---

# Why Git Became Popular

Git solved major issues of centralized systems:

✅ No single point of failure
✅ Faster collaboration
✅ Better branching
✅ Offline commits
✅ Distributed architecture

---

# 8. What is Fork?

A **Fork** means:

> Creating a complete copy of another repository.

---

## Diagram

```text
Original Repository
        |
        |---- Fork by Abhishek
        |
        |---- Fork by Developer2
```

---

## Why Fork?

* Independent development
* Open-source contributions
* Safe experimentation
* Personal copies

---

# 9. Git vs GitHub

This is a VERY IMPORTANT interview question.

---

# What is Git?

Git is:

✅ Open-source
✅ Distributed Version Control System
✅ Tool for tracking code changes

---

# What is GitHub?

GitHub is:

> A platform built ON TOP OF Git.

---

## GitHub Provides

| Feature            | Available |
| ------------------ | --------- |
| UI Interface       | Yes       |
| Code Hosting       | Yes       |
| Pull Requests      | Yes       |
| Code Review        | Yes       |
| CI/CD              | Yes       |
| Issue Tracking     | Yes       |
| Project Management | Yes       |

---

# Simple Analogy

| Item   | Example                |
| ------ | ---------------------- |
| Git    | Engine                 |
| GitHub | Car built using engine |

---

# Other Platforms Similar to GitHub

* GitLab
* Bitbucket

---

# 10. Git Architecture

```text
Developer Laptop
       |
       | git push
       ↓
Remote Repository (GitHub/GitLab)
       |
       | git pull
       ↓
Other Developers
```

---

# 11. Installing Git

Official website:

[Git Official Website](https://git-scm.com?utm_source=chatgpt.com)

---

## Linux Installation

Example (Ubuntu):

```bash
sudo apt install git
```

---

## Verify Installation

```bash
git
```

If installed successfully, Git commands will appear.

---

# 12. Creating a Git Repository

---

## Step 1 — Create Project Folder

```bash
mkdir example.com
cd example.com
```

---

## Step 2 — Create File

```bash
vim calculator.sh
```

Example:

```bash
x = a + b
```

---

## Step 3 — Initialize Git Repository

```bash
git init
```

---

## Output

```text
Initialized empty Git repository
```

---

# 13. Important Git Commands

The 3 MOST IMPORTANT commands:

```bash
git add
git commit
git push
```

---

# 14. Git Lifecycle

This is extremely important.

---

## Git Workflow Diagram

```text
Working Directory
       |
       | git add
       ↓
Staging Area
       |
       | git commit
       ↓
Local Repository
       |
       | git push
       ↓
Remote Repository (GitHub)
```

---

# 15. Understanding `.git` Folder

After running:

```bash
git init
```

Hidden folder created:

```bash
.git
```

Check using:

```bash
ls -la
```

---

# Important Components Inside `.git`

| Component | Purpose                    |
| --------- | -------------------------- |
| objects   | Stores file versions       |
| refs      | Stores commit references   |
| hooks     | Automation/security checks |
| config    | Git configuration          |
| HEAD      | Current branch pointer     |

---

# 16. Tracking Files

---

## Check Git Status

```bash
git status
```

---

## Untracked File Example

```text
calculator.sh
```

Git says:

> "I see the file, but should I track it?"

---

## Start Tracking

```bash
git add calculator.sh
```

Now Git tracks the file.

---

# 17. Modifying Files

Suppose we update:

```bash
x = a + b + c
```

---

## Check Changes

```bash
git status
```

Git shows:

```text
modified: calculator.sh
```

---

# 18. Git Diff

To see exact changes:

```bash
git diff
```

---

## Example Output

```diff
- x = a + b
+ x = a + b + c
```

---

# 19. Git Commit

A commit is like:

# ➜ Saving a checkpoint/version

---

## Commit Command

```bash
git commit -m "This is my first version"
```

---

## Important Concept

```text
Commit = Snapshot of project at a point in time
```

---

# Example Workflow

```bash
git add calculator.sh
git commit -m "Added addition function"
```

---

# 20. Git Log

Shows commit history.

---

## Command

```bash
git log
```

---

## Example

```text
Commit 1 → Initial version
Commit 2 → Added subtraction
Commit 3 → Bug fix
```

---

# 21. Git Reset

Used to go back to older version.

---

## Command

```bash
git reset --hard <commit-id>
```

---

## Diagram

```text
Commit1 ---- Commit2 ---- Commit3
    ↑
Reset Here
```

---

## Important

⚠️ `--hard` removes current changes permanently.

---

# 22. Sharing Code with GitHub

Local repository exists only on your laptop.

To collaborate:

You need remote hosting like:

* GitHub
* GitLab
* Bitbucket

---

# Flow

```text
Local Git Repo
      |
      | git push
      ↓
GitHub Repository
      |
      ↓
Accessible to Team
```

---

# 23. Creating GitHub Repository

Official Website:

[GitHub Official Website](https://github.com?utm_source=chatgpt.com)

---

## Steps

### 1. Create Account

* Sign Up
* Verify Email

---

### 2. Create Repository

Click:

```text
New Repository
```

---

### 3. Enter Details

| Field           | Example        |
| --------------- | -------------- |
| Repository Name | calculator     |
| Visibility      | Public/Private |
| README          | Optional       |

---

### 4. Create Repository

Done ✅

---

# Public vs Private Repository

| Type    | Visibility        |
| ------- | ----------------- |
| Public  | Anyone can see    |
| Private | Restricted access |

---

# 24. Common Interview Questions

---

## Q1. What is Version Control System?

A system used to track code changes and maintain version history.

---

## Q2. Difference Between Git and GitHub?

| Git             | GitHub        |
| --------------- | ------------- |
| Tool            | Platform      |
| Version Control | Code Hosting  |
| CLI Based       | Web Interface |

---

## Q3. Difference Between Centralized and Distributed VCS?

| CVCS              | DVCS            |
| ----------------- | --------------- |
| Single server     | Multiple copies |
| High failure risk | Reliable        |
| Example: SVN      | Example: Git    |

---

## Q4. What is Fork?

A complete copy of another repository.

---

## Q5. What is Git Commit?

A snapshot/version of project changes.

---

## Q6. What is `.git` Folder?

Internal metadata folder Git uses for tracking repository information.

---

# 25. Key Takeaways

---

## Core Learning

✅ Git is a Distributed Version Control System
✅ GitHub is a platform built on Git
✅ Git solves sharing + versioning problems
✅ Commits help track versions
✅ GitHub enables collaboration
✅ Fork creates full repository copy
✅ `.git` stores tracking information

---

# 26. Git Cheat Sheet

---

# Initialize Repository

```bash
git init
```

---

# Check Status

```bash
git status
```

---

# Add File

```bash
git add filename
```

---

# Add All Files

```bash
git add .
```

---

# View Changes

```bash
git diff
```

---

# Commit Changes

```bash
git commit -m "message"
```

---

# View History

```bash
git log
```

---

# Reset to Previous Commit

```bash
git reset --hard <commit-id>
```

---

# Push Code to GitHub

```bash
git push
```

---

# Clone Repository

```bash
git clone <repo-url>
```

---

# Pull Latest Changes

```bash
git pull
```

---

# Final Summary Diagram

```text
                VERSION CONTROL SYSTEM
                           |
        -----------------------------------------
        |                                       |
   Centralized VCS                       Distributed VCS
     (SVN/CVS)                                 (Git)
        |                                       |
 Single Central Server               Multiple Repository Copies
        |                                       |
 Single Point Failure                     Highly Reliable
        |
      Problems
```

---

# End Notes

This lecture mainly focused on:

* Version Control Basics
* Git Fundamentals
* Git vs GitHub
* Git Workflow
* Core Git Commands
* Distributed Systems Concept

The next learning topics generally include:

* GitHub Deep Dive
* Branches
* Pull Requests
* Merge
* CI/CD with GitHub
* GitHub Actions

---
