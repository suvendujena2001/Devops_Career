# Linux Zero to Hero — Day 3 Notes

## User Management | Group Management | File Management | Vim Editor Shortcuts

> Instructor: Abhishek Virala
> Topics Covered:
>
> * User Management
> * Group Management
> * SSH Basics
> * File & Directory Management
> * Vim/Vi Editor Basics
> * File Viewing Commands

---

# Table of Contents

1. Introduction to User Management
2. Why Linux Needs Users & Groups
3. Creating Users in Linux
4. Password Management
5. Important Linux User Files
6. User Switching & Permissions
7. Group Management
8. SSH Login to Linux Servers
9. File & Directory Management
10. Vim / Vi Editor
11. File Reading Commands
12. Important Interview Questions
13. Command Cheat Sheet
14. Summary Diagram

---

# 1. Introduction to User Management

## Why User Management Exists

On personal laptops:

* Usually only **one admin user**
* Accountability is obvious

On Linux servers:

* Multiple developers
* Multiple teams
* Shared infrastructure

Without users:

* Everyone uses `root`
* No accountability
* Security risk

---

# 2. Why Linux Needs Users & Groups

## Problem Scenario

```text
100 Developers
        ↓
Everyone uses ROOT
        ↓
One user deletes /sbin
        ↓
Server corruption
        ↓
No accountability
```

---

## Linux Solution

```text
Linux Administrator
        ↓
Creates Individual Users
        ↓
Assigns Permissions
        ↓
Tracks Actions Securely
```

---

# 3. Creating Users in Linux

## Method 1 — `useradd`

Creates user quickly.

```bash
useradd avirala
```

### Characteristics

| Feature                | useradd |
| ---------------------- | ------- |
| Creates User           | ✅       |
| Creates Home Directory | ❌       |
| Interactive            | ❌       |
| Used in Scripts        | ✅       |

---

## Verify User Creation

Linux stores user information in:

```bash
/etc/passwd
```

View it:

```bash
cat /etc/passwd
```

---

## Method 2 — `adduser`

Recommended for manual user creation.

```bash
adduser avirala
```

### Characteristics

| Feature                | adduser |
| ---------------------- | ------- |
| Creates User           | ✅       |
| Creates Home Directory | ✅       |
| Interactive Inputs     | ✅       |
| Human Friendly         | ✅       |

---

## Difference Between `useradd` vs `adduser`

| Feature                | useradd | adduser |
| ---------------------- | ------- | ------- |
| Script Friendly        | ✅       | ❌       |
| Creates Home Directory | ❌       | ✅       |
| Asks User Details      | ❌       | ✅       |
| Interactive            | ❌       | ✅       |

---

# 4. Password Management

## Set Password

```bash
passwd avirala
```

---

## Password Storage

Encrypted passwords are stored in:

```bash
/etc/shadow
```

View:

```bash
cat /etc/shadow
```

---

## Important Security Concept

### Passwords Cannot Be Decrypted

Linux uses **one-way hashing**.

```text
Plain Password
      ↓
Hashing Algorithm (SHA)
      ↓
Encrypted Hash
```

### Key Point

* You can RESET passwords
* You CANNOT restore original passwords

---

# 5. Important Linux User Files

| File          | Purpose                    |
| ------------- | -------------------------- |
| `/etc/passwd` | Stores user information    |
| `/etc/shadow` | Stores encrypted passwords |
| `/etc/group`  | Stores group information   |

---

# 6. User Switching & Permissions

## Switch User

```bash
su - avirala
```

---

## Check Current User

```bash
whoami
```

---

## Root vs Normal User

### Example

Attempt to delete system directory:

```bash
rm -rf /sbin
```

### Result

| User Type   | Result            |
| ----------- | ----------------- |
| Root        | Allowed           |
| Normal User | Permission Denied |

---

## Permission Flow

```text
ROOT USER
   ↓
Full Access

NORMAL USER
   ↓
Restricted Access
```

---

# 7. Group Management

## Why Groups?

Managing permissions individually for:

* 500 developers
* 200 testers
* 50 DevOps engineers

is difficult.

---

## Solution

```text
Users
   ↓
Groups
   ↓
Permissions Applied Once
```

---

## Example Structure

```text
dev group
├── user1
├── user2
└── user3

qa group
├── user4
└── user5
```

---

## Create Group

```bash
groupadd devops
```

---

## View Groups

```bash
cat /etc/group
```

---

## Add User to Group

```bash
usermod -aG devops avirala
```

### Breakdown

| Option | Meaning |
| ------ | ------- |
| `-a`   | Append  |
| `-G`   | Group   |

---

# 8. SSH Login to Linux Servers

## What is SSH?

SSH = Secure Shell

Used to remotely connect to Linux servers.

---

# SSH Architecture

```text
Your Laptop
(SSH Client)
      ↓
Internet
      ↓
Linux Server
(SSH Daemon/sshd)
```

---

## SSH Command

```bash
ssh username@ip-address
```

Example:

```bash
ssh avirala@172.31.x.x
```

---

## SSH Configuration File

```bash
/etc/ssh/sshd_config.d/
```

---

## Password Authentication

| Setting                      | Meaning                 |
| ---------------------------- | ----------------------- |
| `PasswordAuthentication yes` | Password login enabled  |
| `PasswordAuthentication no`  | Password login disabled |

---

## Restart SSH Service

```bash
systemctl restart ssh
```

---

# 9. File & Directory Management

---

# Basic Navigation Commands

| Command | Purpose                   |
| ------- | ------------------------- |
| `ls`    | List files                |
| `cd`    | Change directory          |
| `pwd`   | Present working directory |

---

## Example

```bash
cd /tmp
pwd
ls
```

---

# Directory Management

## Create Directory

```bash
mkdir avirala
```

---

## Remove Empty Directory

```bash
rmdir avirala
```

---

## Remove Directory Forcefully

```bash
rm -rf avirala
```

⚠️ Dangerous command.

---

# File Management

## Create File

```bash
touch demo.txt
```

---

## Delete File

```bash
rm demo.txt
```

---

## Copy File

```bash
cp source.txt copy.txt
```

---

## Rename/Move File

```bash
mv old.txt new.txt
```

---

# Linux File System Navigation

```text
/
├── home
├── tmp
├── etc
├── var
└── sbin
```

---

# 10. Vim / Vi Editor

## Open File

```bash
vim demo.txt
```

---

# Vim Modes

```text
NORMAL MODE
    ↓ Press i
INSERT MODE
    ↓ Press ESC
COMMAND MODE
```

---

## Mode Explanation

| Mode    | Purpose    |
| ------- | ---------- |
| Normal  | Navigation |
| Insert  | Writing    |
| Command | Save/Quit  |

---

# Common Vim Commands

## Enter Insert Mode

```bash
i
```

---

## Save and Quit

```bash
:wq!
```

---

## Quit Without Saving

```bash
:q!
```

---

## Navigation Shortcuts

| Shortcut        | Purpose        |
| --------------- | -------------- |
| `ESC + :0`      | First line     |
| `ESC + Shift+G` | Last line      |
| `ESC + :500`    | Go to line 500 |

---

# Vim Workflow Diagram

```text
Open File
   ↓
NORMAL MODE
   ↓ Press i
INSERT MODE
   ↓ Write Content
ESC
   ↓
COMMAND MODE
   ↓
:wq!
```

---

# 11. File Reading Commands

---

## Print Entire File

```bash
cat demo.txt
```

---

## Interactive File Reading

```bash
less demo.txt
```

### Exit `less`

```bash
q
```

---

## View First Lines

```bash
head -10 demo.txt
```

---

## View Last Lines

```bash
tail -20 demo.txt
```

---

## Reverse Output

```bash
tac demo.txt
```

---

# 12. Echo Command & Redirection

## Overwrite File

```bash
echo "hello" > demo.txt
```

⚠️ Deletes old content.

---

## Append to File

```bash
echo "world" >> demo.txt
```

Adds new content safely.

---

# Redirection Diagram

```text
echo "hello"
       ↓
    > file
       ↓
Overwrite

echo "hello"
       ↓
   >> file
       ↓
Append
```

---

# 13. Important Interview Questions

---

## Q1. Difference Between `useradd` and `adduser`

### Answer

| useradd            | adduser          |
| ------------------ | ---------------- |
| Non-interactive    | Interactive      |
| No home dir        | Creates home dir |
| Used in automation | Used manually    |

---

## Q2. Can Linux Passwords Be Restored?

### Answer

❌ No.

Passwords are hashed using one-way encryption.

Only reset is possible.

---

## Q3. Why Are Groups Needed?

### Answer

Groups simplify permission management for large numbers of users.

---

## Q4. Difference Between `>` and `>>`

| Symbol | Purpose   |
| ------ | --------- |
| `>`    | Overwrite |
| `>>`   | Append    |

---

# 14. Complete Command Cheat Sheet

## User Management

```bash
useradd username
adduser username
passwd username
userdel username
su - username
whoami
```

---

## Group Management

```bash
groupadd groupname
usermod -aG groupname username
cat /etc/group
```

---

## SSH

```bash
ssh user@ip
systemctl restart ssh
```

---

## File Management

```bash
ls
cd
pwd
mkdir
rmdir
touch
rm
cp
mv
```

---

## File Viewing

```bash
cat
less
head
tail
tac
```

---

## Vim

```bash
vim filename
i
ESC
:wq!
:q!
```

---

# Final Summary

```text
Linux Day 3
│
├── User Management
│   ├── useradd
│   ├── adduser
│   ├── passwd
│   └── permissions
│
├── Group Management
│   ├── groupadd
│   └── usermod
│
├── SSH
│   ├── ssh client
│   └── sshd server
│
├── File Management
│   ├── ls
│   ├── cd
│   ├── mkdir
│   └── rm
│
└── Vim Editor
    ├── Insert Mode
    ├── Command Mode
    └── Navigation
```

---

# Key Takeaways

✅ Linux uses users and groups for accountability
✅ Root user has full permissions
✅ SSH is used to access remote Linux servers
✅ Vim has multiple modes
✅ `>` overwrites while `>>` appends
✅ Groups simplify permission management
✅ Passwords in Linux cannot be decrypted

---
