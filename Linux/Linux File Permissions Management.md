# 🐧 Day 4 — Linux File Permissions Management

> **Linux File Permissions** are one of the most important security features in Linux.
> They protect files and folders from unauthorized access, accidental deletion, and unwanted modifications.

---

# 📚 Table of Contents

1. Introduction to File Permissions
2. Why File Permissions are Needed
3. Linux Multi-User System
4. Understanding Default Permissions
5. File Permission Structure
6. Users, Groups, and Others
7. Read, Write, Execute Permissions
8. Understanding `ls -ltr`
9. `chmod` Command
10. Numeric Permission System
11. File vs Folder Permissions
12. `chown` Command
13. Important Real-World Examples
14. Interview Concepts
15. Summary Cheat Sheet

---

# 1️⃣ Introduction to File Permissions

Linux is a **multi-user operating system**.

That means:

* Multiple users can access the same Linux server
* Every user can work independently
* Users can share server resources safely

But imagine this:

❌ Any user can delete system files
❌ Any user can modify another user's scripts
❌ Any user can corrupt applications

That would be dangerous.

---

# 🎯 Purpose of File Permissions

Linux introduced **File Permissions** to solve this problem.

File permissions help:

✅ Protect system files
✅ Prevent accidental deletion
✅ Restrict unauthorized access
✅ Control who can read/write/execute files
✅ Secure applications and scripts

---

# 2️⃣ Linux Multi-User System

Example organization:

```text
Organization Server
│
├── Developer Users
│   ├── dev1
│   ├── dev2
│
├── QE Users
│   ├── qe1
│   ├── qe2
│
└── DevOps Users
    ├── ops1
    ├── ops2
```

Every employee gets their own Linux account.

---

# 3️⃣ Why Permissions Are Important

Imagine:

```text
Developer creates:
    /tmp/automation.sh
```

Now if another user deletes it accidentally:

❌ Developer's workflow breaks

Or worse:

```text
User deletes:
/bin
/sbin
/etc
```

💥 Entire Linux system becomes unstable.

---

# 4️⃣ Linux Automatically Sets Permissions

Whenever a file/folder is created:

✅ Linux automatically assigns default permissions.

Example:

```bash
touch hello.sh
mkdir project
```

Linux immediately adds permissions to both.

---

# 5️⃣ Understanding `ls -ltr`

Command:

```bash
ls -ltr
```

Example output:

```text
-rw-rw-r-- 1 developer developer  45 May 17 hello.sh
drwxr-x--- 2 developer developer 4096 May 17 project
```

---

# 🔍 Breaking the Permission String

Example:

```text
-rw-rw-r--
```

Let's understand this beautifully.

---

# 🧩 Permission Structure Diagram

```text
-rw-rw-r--
││ ││ ││
││ ││ └└── Others
││ └└──── Group
└└──────── User
```

---

# 📌 First Character

| Symbol | Meaning   |
| ------ | --------- |
| `-`    | File      |
| `d`    | Directory |

Example:

```text
-rw-r--r--   → File
drwxr-xr-x   → Directory
```

---

# 📌 Remaining 9 Characters

They are divided into **3 groups**:

```text
rwx rwx rwx
│   │   │
│   │   └── Others
│   └────── Group
└────────── User
```

---

# 6️⃣ User, Group, Others

## 👤 User (Owner)

The person who created the file.

Example:

```text
developer created hello.sh
```

So:

```text
User = developer
```

---

## 👥 Group

A collection of users.

Example:

```text
Group = developers
```

All developers may share similar access.

---

## 🌍 Others

Everyone else on the Linux server.

Example:

```text
qe1
ops1
guest
```

---

# 7️⃣ Read, Write, Execute

Each permission set contains:

```text
rwx
```

| Permission | Meaning        |
| ---------- | -------------- |
| `r`        | Read           |
| `w`        | Write / Modify |
| `x`        | Execute        |

---

# 📖 Example

```text
rwxrw-r--
```

Breakdown:

| Section | Meaning              |
| ------- | -------------------- |
| `rwx`   | User has full access |
| `rw-`   | Group can read/write |
| `r--`   | Others can only read |

---

# 🎨 Permission Visualization

```text
User    → rwx → Read, Write, Execute
Group   → rw- → Read, Write
Others  → r-- → Read only
```

---

# 8️⃣ Practical Example

## Create Users

```bash
adduser developer
adduser qe
```

---

## Switch Users

```bash
su - developer
su - qe
```

---

## Create File as Developer

```bash
cd /tmp
vim hello.sh
```

Content:

```bash
#!/bin/bash
echo "Hello World"
```

---

## QE User Reads File

```bash
cat hello.sh
```

✅ Allowed

---

## QE User Modifies File

```bash
vim hello.sh
```

❌ Permission denied

---

## QE User Deletes File

```bash
rm hello.sh
```

❌ Operation not permitted

---

# 🔥 This Proves

Linux permissions protect files automatically.

---

# 9️⃣ chmod Command

`chmod` = **Change Mode (Permissions)**

Used to modify permissions.

---

# Syntax

```bash
chmod permissions filename
```

---

# 🔤 Symbolic Method

```bash
chmod u=rwx hello.sh
```

Meaning:

```text
u → user
g → group
o → others
```

---

# Example

```bash
chmod o=r hello.sh
```

Others can only read.

---

# Multiple Permissions

```bash
chmod u=rwx,g=rw,o=r hello.sh
```

---

# 📊 Symbolic Permission Chart

| Symbol | Meaning |
| ------ | ------- |
| `u`    | User    |
| `g`    | Group   |
| `o`    | Others  |

---

# 🔢 Numeric Permission System

Linux also supports numbers.

---

# Permission Values

| Permission | Value |
| ---------- | ----- |
| `r`        | 4     |
| `w`        | 2     |
| `x`        | 1     |

---

# 🎯 Calculation Examples

## `rwx`

```text
4 + 2 + 1 = 7
```

---

## `rw-`

```text
4 + 2 = 6
```

---

## `r--`

```text
4 = 4
```

---

# 📌 Common Permission Values

| Permission | Numeric |
| ---------- | ------- |
| `rwx`      | 7       |
| `rw-`      | 6       |
| `r-x`      | 5       |
| `r--`      | 4       |
| `---`      | 0       |

---

# 🔥 Important Examples

---

## `777`

```bash
chmod 777 file.sh
```

```text
rwx rwx rwx
```

✅ Everyone has full access

⚠️ Very risky

---

## `644`

```bash
chmod 644 file.txt
```

```text
rw- r-- r--
```

✅ Common for normal files

---

## `755`

```bash
chmod 755 script.sh
```

```text
rwx r-x r-x
```

✅ Common for executable scripts

---

## `400`

```bash
chmod 400 secret.txt
```

```text
r-- --- ---
```

✅ Only owner can read

---

# 📊 Numeric Permission Diagram

```text
chmod 764 file.sh

7 → rwx → User
6 → rw- → Group
4 → r-- → Others
```

---

# 🔟 File Execution Example

Suppose:

```text
-rw-rw-r--
```

No execute permission.

---

## Try Running Script

```bash
./hello.sh
```

❌ Permission denied

---

## Add Execute Permission

```bash
chmod u=rwx hello.sh
```

Now:

```bash
./hello.sh
```

✅ Script executes successfully

---

# 1️⃣1️⃣ Folder Permissions

Permissions apply to directories too.

Example:

```text
drwxr-x---
```

---

# 🔥 Important Concept

To access a file:

✅ You must first access the folder.

---

# 🏦 Bank & Locker Analogy

```text
Folder = Bank
File   = Locker
```

To access locker:

1. Enter bank first
2. Then access locker

---

# 🚨 Interview Question Concept

Suppose:

```text
/tmp/demo.txt → 777
/tmp          → 700
```

Can another user access `demo.txt`?

❌ NO

Because:

```text
No access to /tmp
```

Folder permission blocks access first.

---

# 📌 Priority Rule

```text
Folder Permission > File Permission
```

---

# 1️⃣2️⃣ chown Command

`chown` = **Change Ownership**

Used to transfer file ownership.

---

# Example

Current owner:

```text
developer developer test.sh
```

---

## Change Owner

```bash
chown qe:qe test.sh
```

Now:

```text
qe qe test.sh
```

Ownership transferred successfully.

---

# 📊 chown Diagram

```text
Before:
developer → owns → test.sh

After:
qe → owns → test.sh
```

---

# 1️⃣3️⃣ Real-World Linux Permission Defaults

| Resource         | Common Permission |
| ---------------- | ----------------- |
| Files            | 644               |
| Scripts          | 755               |
| Home Directories | 750               |
| Sensitive Files  | 400               |

---

# 1️⃣4️⃣ Most Important Commands

| Command   | Purpose            |
| --------- | ------------------ |
| `ls -ltr` | View permissions   |
| `chmod`   | Change permissions |
| `chown`   | Change ownership   |
| `whoami`  | Current user       |
| `cat`     | Read file          |
| `rm`      | Delete file        |

---

# 🧠 Ultimate Permission Flow

```text
User tries to access file
        │
        ▼
Check Folder Permission
        │
        ▼
Allowed?
 ├── No → Access Denied
 └── Yes
        │
        ▼
Check File Permission
        │
        ▼
Allowed?
 ├── No → Access Denied
 └── Yes → Access Granted
```

---

# 🎯 Key Takeaways

✅ Linux is a multi-user OS
✅ File permissions secure the system
✅ Permissions apply to files and folders
✅ Three entities:

* User
* Group
* Others

✅ Three permissions:

* Read
* Write
* Execute

✅ `chmod` changes permissions
✅ `chown` changes ownership
✅ Folder permission is checked first

---

# 🚀 Quick Cheat Sheet

```bash
# View permissions
ls -ltr

# Give full access
chmod 777 file

# Read-only
chmod 444 file

# Owner full access only
chmod 700 file

# Execute permission
chmod +x script.sh

# Change owner
chown user:group file
```

---

# 🎓 Final Conclusion

Linux File Permissions are the foundation of:

* Linux Security
* User Isolation
* Server Protection
* Secure Collaboration

Understanding permissions deeply is essential for:

✅ DevOps Engineers
✅ System Administrators
✅ Cloud Engineers
✅ Security Engineers
✅ Developers

---

# 💡 Golden Rule to Remember

> **“First Folder Permission, then File Permission.”**

🏦 Bank first → 🔐 Locker next

---

# 🏁 End of Notes

> Practice `chmod`, `chown`, and `ls -ltr` repeatedly.
> File permissions become very easy once you experiment hands-on.
