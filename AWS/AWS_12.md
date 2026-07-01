````markdown
# 📘 Day 12 – AWS CI/CD | AWS CodeCommit
### Complete Notes 

---

# 🎯 Learning Objectives

After completing this lesson, you should be able to:

- Understand what CI/CD is on AWS.
- Learn AWS Developer Tools used for CI/CD.
- Understand AWS CodeCommit.
- Compare CodeCommit with GitHub/GitLab.
- Learn why AWS introduced CodeCommit.
- Understand its advantages and disadvantages.
- Learn the practical workflow of creating repositories and pushing code.
- Understand why many organizations still prefer GitHub or GitLab.

---

# What is CI/CD?

CI/CD stands for:

| CI | CD |
|----|----|
| Continuous Integration | Continuous Delivery / Continuous Deployment |

CI/CD is an automated software delivery pipeline that allows developers to:

- Continuously integrate code changes
- Automatically build applications
- Test applications
- Deploy applications

without manual intervention.

---

# Traditional CI/CD Pipeline

Most organizations traditionally use multiple tools.

```text
Developer
    │
    ▼
GitHub / GitLab
    │
(Webhook Trigger)
    │
    ▼
Jenkins
    │
    ▼
Build + Test
(Maven, Gradle, Docker, SonarQube...)
    │
    ▼
Deployment
(ArgoCD / Ansible / Shell Script / Kubernetes / EC2)
```

---

# AWS Managed CI/CD

AWS provides fully managed services for each stage of the pipeline.

```text
Developer
      │
      ▼
AWS CodeCommit
(Source Code Repository)
      │
      ▼
AWS CodePipeline
(Pipeline Orchestrator)
      │
      ▼
AWS CodeBuild
(Build & Test)
      │
      ▼
AWS CodeDeploy
(Application Deployment)
      │
      ▼
EC2 / ECS / EKS / Lambda
```

---

# Mapping Traditional Tools to AWS Services

| Traditional Tool | AWS Equivalent | Purpose |
|------------------|---------------|----------|
| GitHub | AWS CodeCommit | Source Code Repository |
| Jenkins | AWS CodePipeline | Pipeline Orchestration |
| Maven / Docker Build | AWS CodeBuild | Build & Testing |
| ArgoCD / Ansible | AWS CodeDeploy | Deployment |

> **Note:** This is a conceptual mapping. In real-world implementations, AWS services can integrate with GitHub, GitLab, Bitbucket, Jenkins, and many other tools.

---

# AWS Developer Tools

AWS provides four major services for CI/CD.

```text
                    AWS Developer Tools

          +----------------------------+
          |      CodeCommit            |
          |  Source Code Repository    |
          +-------------+--------------+
                        |
                        ▼
          +----------------------------+
          |      CodePipeline          |
          | Pipeline Automation        |
          +-------------+--------------+
                        |
                        ▼
          +----------------------------+
          |      CodeBuild             |
          | Build + Test               |
          +-------------+--------------+
                        |
                        ▼
          +----------------------------+
          |      CodeDeploy            |
          | Application Deployment     |
          +----------------------------+
```

---

# AWS CodeCommit

AWS CodeCommit is:

> A fully managed Git-based source control service provided by AWS.

It allows developers to:

- Store source code
- Manage branches
- Perform commits
- Review pull requests
- Maintain version history

Just like GitHub or GitLab.

---

# Why Did AWS Create CodeCommit?

Before CodeCommit, organizations usually had two options:

## Option 1

Use GitHub Enterprise.

## Option 2

Install GitLab on self-managed servers.

Example:

```text
EC2 Server
      │
      ▼
GitLab Installed
      │
      ▼
Developers connect
```

Problems:

- Server maintenance
- OS patching
- Security updates
- Storage management
- High availability
- Scaling
- Backups

AWS solved this by introducing CodeCommit.

---

# What Problem Does CodeCommit Solve?

Instead of:

```text
Manage Git Server
        │
Install GitLab
        │
Patch Servers
        │
Scale Servers
        │
Take Backups
```

AWS provides:

```text
AWS CodeCommit

Managed by AWS

✔ No servers
✔ No patching
✔ No scaling
✔ No maintenance
✔ Highly available
```

---

# CodeCommit Architecture

```text
            Developers

        ┌────────┴────────┐
        │                 │
        ▼                 ▼

   Visual Studio      AWS Console

        │                 │
        └────────┬────────┘
                 ▼

          AWS CodeCommit

                 │

       Git Repository Storage
```

---

# Features of AWS CodeCommit

- Git-based repositories
- Branch management
- Commit history
- Pull Requests
- Repository permissions
- IAM integration
- HTTPS access
- SSH access
- Encryption
- AWS integrations

---

# Important Difference

## GitHub

Supports:

- Public repositories
- Private repositories

Example:

```text
GitHub

Public Repo
Private Repo
```

---

## AWS CodeCommit

Supports:

```text
Private Repositories Only
```

No public repositories exist.

This makes sense because CodeCommit is designed for enterprise organizations.

---

# Advantages of AWS CodeCommit

## 1. Fully Managed

AWS manages everything.

You don't need to:

- Install Git
- Maintain servers
- Patch servers
- Upgrade GitLab

---

## 2. Automatic Scalability

Suppose today you have:

```text
100 Developers
20 Repositories
```

Tomorrow:

```text
2000 Developers
500 Repositories
```

AWS automatically handles scaling.

No server upgrades required.

---

## 3. High Availability

AWS ensures:

- Data replication
- Availability
- Durability
- Reliability

---

## 4. Security

Integrated with:

- IAM
- CloudTrail
- KMS Encryption

---

## 5. Easy AWS Integration

Works seamlessly with:

- CodePipeline
- CodeBuild
- CodeDeploy
- CloudWatch
- SNS

---

# Practical Demo Summary

## Step 1

Search:

```text
AWS Console
      ↓
CodeCommit
```

---

## Step 2

Create Repository

Example:

```text
Repository Name:

demo-repo-cc
```

---

## Step 3

Repository Created

Repository contains:

```text
Repository

├── Files
├── Branches
├── Commits
├── Pull Requests
├── Settings
```

---

## Step 4

Upload Files

Options:

### Using Browser

```
Add File
     ↓
Upload File
```

Suitable for:

- Small changes
- Single file upload

---

### Using Git

Preferred for:

- Multiple files
- Development work
- Visual Studio Code
- CI/CD

---

# Why Use Git Instead of Browser?

Browser allows:

- Upload one file
- Edit one file

Git allows:

- Multiple commits
- Branches
- Merge
- Push hundreds of files
- Team collaboration

---

# IAM Requirement

AWS recommends **not using the root account**.

Instead:

Create an IAM user.

Example:

```text
IAM

↓

Users

↓

Create User

↓

Attach Policy
```

---

# Required IAM Policy

Attach:

```text
AWSCodeCommitPowerUser
```

This provides permissions for:

- Clone
- Push
- Pull
- Branches
- CloudWatch Integration
- SNS Integration

---

# Clone Repository

After installing Git:

```bash
git clone https://repository-url
```

---

# Git Workflow

```text
Clone Repository

        │

        ▼

Modify Files

        │

        ▼

git add .

        │

        ▼

git commit -m "message"

        │

        ▼

git push
```

---

# Complete Workflow Diagram

```text
Developer

     │

git clone

     │

     ▼

Local Repository

     │

Edit Files

     │

git add

     │

git commit

     │

git push

     │

     ▼

AWS CodeCommit Repository
```

---

# Git Configuration

Configure username:

```bash
git config --global user.name "Your Name"
```

Configure email:

```bash
git config --global user.email "you@example.com"
```

---

# Assignment

Create a repository.

Clone it locally.

Create a file.

Then execute:

```bash
git add .
git commit -m "Initial Commit"
git push
```

Verify the file appears inside CodeCommit.

---

# Disadvantages of AWS CodeCommit

Although CodeCommit is fully managed, it has several drawbacks.

---

## 1. Limited Features

Compared to GitHub or GitLab, CodeCommit provides fewer advanced capabilities.

---

## 2. Weaker Collaboration

GitHub offers:

- Discussions
- Rich Pull Requests
- Project Boards
- Issues
- Wikis
- Actions

CodeCommit is relatively basic.

---

## 3. Limited Integrations

GitHub integrates with thousands of tools.

CodeCommit primarily integrates with AWS services.

---

## 4. Inferior Developer Experience

GitHub provides:

- VS Code Web
- GitHub Codespaces
- Copilot
- Rich UI

CodeCommit lacks many of these developer-friendly features.

---

## 5. Smaller Ecosystem

GitHub has:

- Millions of repositories
- Large open-source community
- Extensive documentation
- Marketplace

CodeCommit is primarily designed for private enterprise repositories.

---

# Comparison Table

| Feature | GitHub | GitLab | AWS CodeCommit |
|----------|---------|---------|----------------|
| Public Repository | ✅ | ✅ | ❌ |
| Private Repository | ✅ | ✅ | ✅ |
| Open Source Community | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ❌ |
| AWS Integration | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Rich UI | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| AI Features | Copilot | GitLab Duo | Limited |
| Managed by AWS | ❌ | ❌ | ✅ |
| Enterprise Ready | ✅ | ✅ | ✅ |

---

# Why Most Organizations Still Prefer GitHub or GitLab

Reasons include:

- Better collaboration
- Rich developer ecosystem
- Excellent integrations
- Superior user interface
- AI-powered development tools
- Larger community support

Even organizations using AWS often:

```text
GitHub

↓

AWS CodePipeline

↓

AWS CodeBuild

↓

AWS CodeDeploy
```

instead of

```text
CodeCommit

↓

CodePipeline

↓

CodeBuild

↓

CodeDeploy
```

---

# Interview Question

**Why is AWS CodeCommit less popular?**

**Answer:**

Although CodeCommit is fully managed and integrates well with AWS, it offers fewer collaboration features, limited third-party integrations, and a smaller ecosystem compared to GitHub and GitLab. Most organizations therefore prefer GitHub or GitLab as their source control platform while integrating them with AWS CI/CD services.

---

# Key Takeaways

- AWS provides managed CI/CD services.
- CodeCommit is AWS's Git repository service.
- It replaces self-hosted Git servers.
- Supports only private repositories.
- Integrates seamlessly with AWS Developer Tools.
- IAM users should be used instead of the root account.
- Git commands remain the same (`clone`, `add`, `commit`, `push`).
- GitHub and GitLab remain more widely adopted due to their richer features and larger ecosystems.
- In practice, many organizations use **GitHub + AWS CodePipeline + AWS CodeBuild + AWS CodeDeploy** rather than relying solely on CodeCommit.

---

# End of Notes
````
