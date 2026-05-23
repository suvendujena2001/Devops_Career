# Git Interview Q&A and Commands for DevOps — Complete Notes

> Based on a real-world DevOps workflow using Git and GitHub.
> These notes cover:
>
> * Git fundamentals
> * Real-world workflows
> * Interview questions
> * Git commands
> * Branching strategies
> * Merge vs Rebase
> * Clone vs Fork
> * SSH authentication
> * Conflict resolution
> * Practical DevOps usage

---

# Table of Contents

1. Introduction to Git
2. Why Git is Important in DevOps
3. Git Architecture
4. Git Repository Initialization
5. Understanding `.git`
6. Git Workflow Lifecycle
7. Important Git Commands
8. Git Status & Tracking
9. Git Add
10. Git Commit
11. Git Push
12. Local vs Remote Repository
13. Git Clone
14. HTTPS vs SSH Authentication
15. SSH Key Generation
16. Git Fork vs Git Clone
17. Branching in Git
18. Git Checkout
19. Git Merge
20. Git Rebase
21. Git Cherry-Pick
22. Merge Conflicts
23. Real-World DevOps Workflow
24. Important Interview Questions
25. Command Cheat Sheet
26. Best Practices

---

# 1. Introduction to Git

## What is Git?

Git is a:

* **Distributed Version Control System (DVCS)**

It helps developers:

* Track code changes
* Collaborate with teams
* Maintain history
* Roll back changes
* Manage multiple versions

---

# 2. Why Git is Important in DevOps

In DevOps:

* Developers continuously modify code
* Multiple engineers work simultaneously
* CI/CD pipelines require source tracking
* Rollbacks are critical

Git enables:

| Feature         | Benefit                           |
| --------------- | --------------------------------- |
| Version Control | Track every change                |
| Collaboration   | Multiple developers work together |
| Branching       | Isolate features                  |
| Rollback        | Restore previous stable code      |
| Auditability    | Know who changed what             |
| Automation      | CI/CD integration                 |

---

# 3. Git Architecture

```text
                ┌─────────────────────┐
                │   Remote Repository │
                │ (GitHub/GitLab/etc) │
                └─────────┬───────────┘
                          │
                     git push/pull
                          │
                ┌─────────▼───────────┐
                │   Local Repository  │
                │      (.git)         │
                └─────────┬───────────┘
                          │
                    git add/commit
                          │
                ┌─────────▼───────────┐
                │ Working Directory   │
                │ calculator.sh etc   │
                └─────────────────────┘
```

---

# 4. Git Repository Initialization

## Command

```bash
git init
```

## Purpose

Creates a new Git repository in the current directory.

---

## What Happens Internally?

Git creates a hidden directory:

```text
.git/
```

This directory contains:

* commit history
* logs
* branches
* hooks
* configurations
* object database

---

## Example

```bash
mkdir git-demo
cd git-demo

git init
```

Output:

```text
Initialized empty Git repository
```

---

# 5. Understanding `.git`

## What is `.git`?

`.git` is the heart of Git.

It stores:

| Component | Purpose             |
| --------- | ------------------- |
| objects   | commit data         |
| refs      | branch references   |
| logs      | activity history    |
| hooks     | automation scripts  |
| config    | repository settings |

---

## View Hidden Files

```bash
ls -a
```

Output:

```text
.git
calculator.sh
```

---

# 6. Git Workflow Lifecycle

---

## Standard Git Workflow

```text
Create/Modify File
        ↓
     git add
        ↓
    git commit
        ↓
     git push
        ↓
Remote Repository
```

---

## Real-World Workflow

```text
Developer writes code
        ↓
Stages changes using git add
        ↓
Creates checkpoint using git commit
        ↓
Uploads code using git push
        ↓
CI/CD pipeline triggers
        ↓
Deployment happens
```

---

# 7. Important Git Commands

| Command         | Purpose                  |
| --------------- | ------------------------ |
| git init        | Initialize repository    |
| git status      | Check repository state   |
| git add         | Stage files              |
| git commit      | Save snapshot            |
| git push        | Upload code              |
| git pull        | Download latest changes  |
| git clone       | Copy repository          |
| git branch      | Create/list branches     |
| git checkout    | Switch branches          |
| git merge       | Combine branches         |
| git rebase      | Reapply commits linearly |
| git cherry-pick | Pick specific commits    |
| git log         | Show history             |
| git diff        | Show changes             |

---

# 8. Git Status & Tracking

## Command

```bash
git status
```

---

## Purpose

Shows:

* tracked files
* untracked files
* staged changes
* modified files

---

## Example

```text
Untracked files:
calculator.sh
```

Meaning:

Git knows the file exists, but is not tracking it yet.

---

# 9. Git Add

## Command

```bash
git add calculator.sh
```

OR

```bash
git add .
```

---

## Purpose

Moves changes to the **staging area**.

Git now starts tracking changes.

---

# Git Staging Architecture

```text
Working Directory
       │
       │ git add
       ▼
Staging Area
       │
       │ git commit
       ▼
Git Repository
```

---

# Why `git add` is Important

Without `git add`:

* Git ignores changes
* No tracking happens
* Cannot commit

---

# 10. Git Commit

## Command

```bash
git commit -m "Added addition functionality"
```

---

## Purpose

Creates a permanent snapshot.

---

# Why Commits Matter

Commits help:

* rollback code
* track bugs
* identify contributors
* audit history

---

# Git Commit History

```text
Commit 1 → Commit 2 → Commit 3
```

Each commit contains:

* author
* timestamp
* changes
* message

---

## View Commit History

```bash
git log
```

Compact view:

```bash
git log --oneline
```

---

# 11. Git Push

## Command

```bash
git push
```

---

## Purpose

Uploads local commits to remote repository.

---

# Local vs Remote

```text
LOCAL MACHINE
    │
git push
    ▼
GITHUB/GITLAB
```

---

# Why `git push` May Fail

If remote repository isn't configured.

---

# Check Remote

```bash
git remote -v
```

---

# Add Remote Repository

```bash
git remote add origin <repository-url>
```

Example:

```bash
git remote add origin https://github.com/user/repo.git
```

---

# 12. Local vs Remote Repository

| Type              | Description             |
| ----------------- | ----------------------- |
| Local Repository  | Exists on your laptop   |
| Remote Repository | Exists on GitHub/GitLab |

---

# 13. Git Clone

## Command

```bash
git clone <repository-url>
```

---

## Purpose

Downloads repository from GitHub/GitLab.

---

# Clone Workflow

```text
GitHub Repository
        │
    git clone
        ▼
Local Machine
```

---

# Example

```bash
git clone https://github.com/argoproj/argo-cd.git
```

---

# 14. HTTPS vs SSH Authentication

| HTTPS                   | SSH              |
| ----------------------- | ---------------- |
| Uses password/token     | Uses SSH keys    |
| Easier initially        | More secure      |
| Frequent authentication | Passwordless     |
| Slower for automation   | Ideal for DevOps |

---

# HTTPS Clone

```bash
git clone https://github.com/user/repo.git
```

---

# SSH Clone

```bash
git clone git@github.com:user/repo.git
```

---

# 15. SSH Key Generation

## Generate SSH Key

```bash
ssh-keygen -t rsa
```

---

# SSH Key Structure

```text
~/.ssh/
    │
    ├── id_rsa        (Private Key)
    ├── id_rsa.pub    (Public Key)
```

---

# Important Rule

| Key         | Share? |
| ----------- | ------ |
| Public Key  | YES    |
| Private Key | NEVER  |

---

# Add Public Key to GitHub

1. Open GitHub Settings
2. SSH and GPG Keys
3. Add New SSH Key
4. Paste `id_rsa.pub`

---

# 16. Git Fork vs Git Clone

---

# Git Clone

## Meaning

Downloads repository locally.

```text
GitHub Repo → Your Laptop
```

---

# Git Fork

## Meaning

Creates your own copy on GitHub.

```text
Original Repo → Your GitHub Account
```

---

# Diagram

```text
Original Repo
      │
      ├── Clone → Local Machine
      │
      └── Fork → Your GitHub Account
```

---

# Real-World Example

You fork open-source projects when:

* contributing externally
* modifying independently
* experimenting safely

---

# Interview Question

## Difference Between Clone and Fork

| Clone                | Fork                  |
| -------------------- | --------------------- |
| Local copy           | Remote copy           |
| Exists on laptop     | Exists on GitHub      |
| Used for development | Used for contribution |
| No ownership         | Your own repository   |

---

# 17. Branching in Git

## What is a Branch?

A separate line of development.

---

# Why Branching?

Allows teams to:

* work independently
* avoid breaking production
* develop large features safely

---

# Real-World Example

Amazon wants new feature:

```text
"Home Services"
```

Instead of modifying production code directly:

* create feature branch
* develop separately
* merge after testing

---

# Branching Diagram

```text
main
 ├── feature-login
 ├── feature-payment
 ├── bugfix-ui
 └── release-v2
```

---

# Create Branch

```bash
git checkout -b division
```

---

# View Branches

```bash
git branch
```

---

# Switch Branches

```bash
git checkout main
```

---

# 18. Git Checkout

## Purpose

Switch between branches.

---

# Example

```bash
git checkout division
```

---

# Branch Isolation Example

```text
main branch:
    addition
    subtraction

division branch:
    addition
    subtraction
    division
```

Changes remain isolated until merged.

---

# 19. Git Merge

## Command

```bash
git merge branch-name
```

---

# Purpose

Combines branches.

---

# Merge Diagram

```text
main
  ├── commit A
  ├── commit B
  │
  └──── feature
          ├── commit C
          └── commit D

After Merge:

main
  ├── A
  ├── B
  ├── C
  └── D
```

---

# Characteristics

| Merge              | Description |
| ------------------ | ----------- |
| Preserves history  | YES         |
| Non-linear history | YES         |
| Safe               | YES         |

---

# 20. Git Rebase

## Command

```bash
git rebase branch-name
```

---

# Purpose

Moves commits to create clean linear history.

---

# Rebase Diagram

```text
Before:

A --- B --- C (main)
       \
        D --- E (feature)

After Rebase:

A --- B --- C --- D --- E
```

---

# Advantages

| Benefit               | Description            |
| --------------------- | ---------------------- |
| Linear history        | Easier tracking        |
| Cleaner logs          | Better readability     |
| Professional workflow | Preferred in many orgs |

---

# Merge vs Rebase

| Merge                | Rebase                  |
| -------------------- | ----------------------- |
| Non-linear           | Linear                  |
| Easier               | Cleaner                 |
| Keeps branch history | Rewrites history        |
| Safer for teams      | Cleaner for maintenance |

---

# Key Interview Answer

## Difference Between Merge and Rebase

### Merge

* Combines histories
* Creates merge commit
* Non-linear history

### Rebase

* Reapplies commits
* Creates linear history
* Cleaner commit structure

---

# 21. Git Cherry-Pick

## Command

```bash
git cherry-pick <commit-id>
```

---

# Purpose

Copies specific commit from another branch.

---

# Example

```text
Branch A:
    commit1
    commit2

Branch B:
    commit3

Cherry-pick commit2 into Branch B
```

---

# When Useful

* small fixes
* hotfixes
* specific features

---

# Limitation

Not practical for:

* thousands of commits
* large feature branches

---

# 22. Merge Conflicts

---

# Why Conflicts Happen

When multiple developers modify same lines.

---

# Conflict Example

```text
<<<<<<< HEAD
percentage functionality
=======
multiplication functionality
>>>>>>> branch
```

---

# How to Resolve

1. Open conflicting file
2. Discuss with developers
3. Keep correct code
4. Remove markers
5. Add & commit again

---

# Conflict Resolution Workflow

```text
Conflict Occurs
      ↓
Open File
      ↓
Choose Correct Changes
      ↓
git add
      ↓
git commit
```

---

# 23. Real-World DevOps Workflow

---

# Enterprise Git Flow

```text
Developer
    ↓
Feature Branch
    ↓
git add
    ↓
git commit
    ↓
git push
    ↓
Pull Request
    ↓
Code Review
    ↓
Merge/Rebase
    ↓
CI/CD Pipeline
    ↓
Production Deployment
```

---

# 24. Important Interview Questions

---

# Q1. What is Git?

Git is a distributed version control system used for tracking changes in source code.

---

# Q2. What happens after `git init`?

A hidden `.git` directory gets created that stores repository metadata and version history.

---

# Q3. Difference Between Git Clone and Git Fork?

| Clone                | Fork                   |
| -------------------- | ---------------------- |
| Local copy           | Remote copy            |
| Used for development | Used for collaboration |

---

# Q4. Difference Between Merge and Rebase?

| Merge              | Rebase         |
| ------------------ | -------------- |
| Non-linear history | Linear history |
| Safer              | Cleaner        |

---

# Q5. What is Git Cherry-Pick?

Copies a specific commit from one branch to another.

---

# Q6. What is Git Workflow?

```bash
git add
git commit
git push
```

---

# Q7. What is Git Branching?

Creating isolated environments for feature development.

---

# Q8. Why Use SSH Instead of HTTPS?

SSH provides:

* secure authentication
* passwordless access
* automation-friendly workflows

---

# 25. Command Cheat Sheet

---

## Repository Setup

```bash
git init
git clone <url>
git remote -v
git remote add origin <url>
```

---

## Tracking Changes

```bash
git status
git diff
git add .
git commit -m "message"
```

---

## Branching

```bash
git branch
git checkout branch-name
git checkout -b new-branch
```

---

## Merging

```bash
git merge branch-name
git rebase branch-name
git cherry-pick <commit-id>
```

---

## Remote Operations

```bash
git push
git pull
git fetch
```

---

# 26. Best Practices

---

# Commit Best Practices

✅ Use meaningful commit messages

```text
Added payment API integration
```

❌ Avoid:

```text
changes
update
fixed stuff
```

---

# Branch Naming Standards

| Good Examples  |
| -------------- |
| feature/login  |
| bugfix/payment |
| hotfix/crash   |

---

# Avoid Direct Push to Main

Always:

```text
Feature Branch → Pull Request → Main
```

---

# Use Rebase Carefully

Avoid rebasing:

* shared public branches
* production branches

---

# Never Commit Secrets

❌ Do NOT commit:

* passwords
* API keys
* certificates

Use:

* `.gitignore`
* secret managers
* hooks

---

# Final Summary

---

# Core Git Workflow

```text
git init
    ↓
git add
    ↓
git commit
    ↓
git push
```

---

# Key Concepts Learned

| Topic                     | Covered |
| ------------------------- | ------- |
| Git Basics                | ✅       |
| Repository Initialization | ✅       |
| `.git` Internals          | ✅       |
| Tracking Files            | ✅       |
| Commits                   | ✅       |
| Push/Pull                 | ✅       |
| Clone vs Fork             | ✅       |
| SSH Authentication        | ✅       |
| Branching                 | ✅       |
| Merge vs Rebase           | ✅       |
| Cherry-Pick               | ✅       |
| Conflict Resolution       | ✅       |
| DevOps Workflow           | ✅       |
| Interview Questions       | ✅       |

---

# Golden Rule for DevOps Engineers

```text
Commit often.
Push safely.
Branch wisely.
Rebase carefully.
Never break production.
```

---
