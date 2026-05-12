# Linux Folder Structure — Complete Revision Notes

## Free Linux Course | Day 2

---

# Table of Contents

1. Introduction
2. Linux Prompt Explained
3. Linux File System Hierarchy
4. Important Linux Directories
5. Linux Directory Structure Diagram
6. Difference Between `/bin` and `/sbin`
7. Home Directory vs Root Directory
8. Linux Boot Process Folders
9. Temporary & Runtime Directories
10. Mounting & Storage Directories
11. Configuration & Log Directories
12. PATH Variable Explained
13. Important Linux Commands
14. Summary Tables
15. Revision Cheat Sheet

---

# 1. Introduction

Linux follows a **hierarchical file system structure**.

Everything in Linux starts from:

```bash
/
```

This is called the:

* Root Directory
* Root of File System
* Parent Directory

All files, folders, applications, users, devices, and configurations exist under `/`.

---

# 2. Linux Prompt Explained

Example Prompt:

```bash
root@ubuntu:/#
```

---

## Breakdown

| Component | Meaning                   |
| --------- | ------------------------- |
| `root`    | Logged-in user            |
| `ubuntu`  | Hostname (machine name)   |
| `:`       | Separator                 |
| `/`       | Current working directory |
| `#`       | Root/admin user           |

---

## Normal User Prompt

```bash
ubuntu@ip-172-31-1-10:~$
```

| Symbol | Meaning        |
| ------ | -------------- |
| `~`    | Home directory |
| `$`    | Normal user    |

---

# Understanding `~`

```bash
~ = /home/username
```

Examples:

```bash
/home/ubuntu
/home/abhishek
```

---

# 3. Linux File System Hierarchy

---

# Master Diagram — Linux File System

```text
/
├── bin
├── sbin
├── boot
├── dev
├── etc
├── home
│   ├── ubuntu
│   └── abhishek
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── srv
├── sys
├── tmp
├── usr
│   ├── bin
│   ├── sbin
│   └── lib
└── var
    ├── log
    ├── cache
    └── spool
```

---

# 4. Important Linux Directories

---

# `/` → Root Directory

The top-most directory in Linux.

Everything begins from here.

---

## Example

```bash
cd /
```

---

# `/home` → User Home Directories

Stores personal files of users.

---

## Structure

```text
/home
├── ubuntu
├── john
└── devops
```

---

## Purpose

Used for:

* Personal files
* Scripts
* Downloads
* User-specific configurations

---

# `/root` → Root User Home

Home directory for the administrator (`root` user).

---

## Important Difference

| User Type   | Home Directory   |
| ----------- | ---------------- |
| Normal User | `/home/username` |
| Root User   | `/root`          |

---

# `/bin` → User Binaries

Contains commands accessible to all users.

---

## Examples

```bash
ls
cp
mv
cat
mkdir
date
```

---

## Actual Location

```bash
/bin/ls
```

---

# `/sbin` → System Binaries

Contains administrative/system commands.

Mostly used by root user.

---

## Examples

```bash
useradd
shutdown
reboot
fdisk
```

---

## Actual Location

```bash
/sbin/useradd
```

---

# Diagram — `/bin` vs `/sbin`

```text
           COMMANDS
               │
     ┌─────────┴─────────┐
     │                   │
 USER COMMANDS     ADMIN COMMANDS
     │                   │
   /bin               /sbin
```

---

# `/usr` → User System Resources

Contains:

* Applications
* Libraries
* User binaries
* Documentation

---

## Important Subdirectories

```text
/usr
├── bin
├── sbin
├── lib
└── share
```

---

# Key Point

`/bin`, `/sbin`, and `/lib` are often shortcuts/symlinks to `/usr`.

---

# `/lib` → Libraries

Stores shared libraries required by:

* Programs
* System binaries
* Kernel

---

## Windows Comparison

Linux Libraries ≈ Windows DLL files

---

# `/etc` → Configuration Files

One of the MOST IMPORTANT directories.

Stores Linux configuration files.

---

# Important Files Inside `/etc`

| File              | Purpose            |
| ----------------- | ------------------ |
| `/etc/passwd`     | User information   |
| `/etc/hosts`      | Local DNS mapping  |
| `/etc/os-release` | OS information     |
| `/etc/fstab`      | Disk mount configs |

---

## Example

```bash
cat /etc/os-release
```

---

# Diagram — `/etc`

```text
/etc
├── passwd
├── hosts
├── os-release
├── ssh
├── systemd
└── network
```

---

# `/var` → Variable Data

Stores changing data.

---

## Common Uses

* Logs
* Cache
* Mail queues
* Spool files

---

# Important Folder

```bash
/var/log
```

Stores:

* System logs
* Application logs
* Server logs

---

# Example

```text
/var/log
├── syslog
├── auth.log
├── nginx
└── apache2
```

---

# `/tmp` → Temporary Files

Stores temporary files.

Files may be auto-deleted after reboot or cleanup.

---

# Best Use Cases

* Temporary scripts
* Cache files
* Temporary logs

---

## Example

```bash
/tmp/test.log
```

---

# `/opt` → Optional Software

Used for third-party/custom software installations.

---

## Example

```text
/opt
├── java
├── maven
└── custom-tool
```

---

# Best Practice

Install manually downloaded software here.

---

# `/boot` → Boot Files

Contains files required during Linux startup.

---

## Includes

* Kernel files
* GRUB bootloader
* Initramfs

---

# Boot Flow Diagram

```text
Power ON
   │
   ▼
BIOS / UEFI
   │
   ▼
BOOTLOADER (/boot)
   │
   ▼
LINUX KERNEL
   │
   ▼
OPERATING SYSTEM
```

---

# `/mnt` → Mount Point

Used to temporarily mount disks or storage.

---

## Examples

```bash
/mnt/data
/mnt/backup
```

---

# Common Usage

* Extra hard disks
* Cloud volumes
* USB devices

---

# `/srv` → Server Data

Stores files related to server applications.

---

## Examples

* Web server files
* FTP files
* Shared content

---

# `/proc` → Process Information

Virtual file system containing process and system info.

---

# Examples

```bash
/proc/cpuinfo
/proc/meminfo
```

---

# Key Point

These files are generated dynamically by the kernel.

---

# `/dev` → Device Files

Linux treats devices as files.

---

# Examples

```bash
/dev/sda
/dev/null
/dev/tty
```

---

# Device File Diagram

```text
HARDWARE DEVICE
       │
       ▼
   /dev entry
       │
       ▼
Linux Kernel Access
```

---

# `/run` → Runtime Data

Stores runtime process data while system is running.

---

# `/media`

Used for removable media devices like:

* USB drives
* DVDs
* External storage

---

# 5. Linux Directory Structure Summary Chart

| Directory | Purpose                  |
| --------- | ------------------------ |
| `/`       | Root filesystem          |
| `/home`   | User home directories    |
| `/root`   | Root user home           |
| `/bin`    | User commands            |
| `/sbin`   | Admin commands           |
| `/usr`    | Applications & resources |
| `/lib`    | Libraries                |
| `/etc`    | Configuration files      |
| `/var`    | Logs & changing data     |
| `/tmp`    | Temporary files          |
| `/opt`    | Third-party software     |
| `/boot`   | Boot files               |
| `/mnt`    | Mount points             |
| `/srv`    | Server files             |
| `/proc`   | Process info             |
| `/dev`    | Device files             |
| `/run`    | Runtime data             |

---

# 6. PATH Variable Explained

When you type:

```bash
ls
```

Linux automatically searches directories listed in the `PATH` variable.

---

# Check PATH

```bash
echo $PATH
```

Example:

```bash
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

---

# PATH Search Flow Diagram

```text
Command Typed
      │
      ▼
Search PATH Directories
      │
      ▼
Command Found?
   ├── YES → Execute
   └── NO  → command not found
```

---

# Find Command Location

```bash
which ls
```

Example Output:

```bash
/usr/bin/ls
```

---

# 7. Important Linux Commands

---

# List Files

```bash
ls
```

Detailed Listing:

```bash
ls -ltr
```

---

# Change Directory

```bash
cd /etc
```

Go to root:

```bash
cd /
```

Go to home:

```bash
cd ~
```

---

# Print Current Directory

```bash
pwd
```

---

# Create Directory

```bash
mkdir test
```

---

# Show File Content

```bash
cat file.txt
```

---

# Switch User

```bash
su username
```

---

# Become Root User

```bash
sudo su -
```

---

# 8. Real-World Linux Understanding

---

# Linux Folder Philosophy

Linux follows:

> "Everything has a dedicated place."

---

## Examples

| Task                  | Recommended Directory |
| --------------------- | --------------------- |
| Install Java manually | `/opt/java`           |
| Store logs            | `/var/log`            |
| User files            | `/home/user`          |
| Temporary scripts     | `/tmp`                |
| Configuration files   | `/etc`                |

---

# Good Linux Practice

✅ Use proper directories
✅ Keep system organized
✅ Follow Linux standards
✅ Avoid random folder creation

---

# Bad Practice Example

❌ Installing software in random directories:

```bash
/random/java
/test/apache
```

---

# Correct Practice

✅

```bash
/opt/java
/opt/apache
```

---

# 9. Revision Cheat Sheet

---

# MOST IMPORTANT DIRECTORIES

```text
/etc    → Configuration
/home   → User files
/bin    → User commands
/sbin   → Admin commands
/var    → Logs
/tmp    → Temporary files
/usr    → System resources
/opt    → Third-party software
```

---

# MOST IMPORTANT COMMANDS

```bash
ls
cd
pwd
mkdir
cat
which
echo $PATH
```

---

# QUICK MEMORY TRICKS

| Folder | Meaning               |
| ------ | --------------------- |
| bin    | binaries              |
| sbin   | system binaries       |
| lib    | libraries             |
| etc    | editable text configs |
| var    | variable data         |
| tmp    | temporary             |
| opt    | optional software     |
| srv    | server                |
| mnt    | mount                 |

---

# Final Learning Summary

Linux filesystem is:

* Structured
* Hierarchical
* Organized
* Permission-driven

Understanding Linux directories is the foundation for:

* DevOps
* Cloud
* Linux Administration
* Kubernetes
* Docker
* CI/CD
* Server Management

---

# Golden Rule

> “In Linux, everything starts from `/` and everything is treated like a file.”
