**GitHub** is a **cloud-based platform** used to **store, manage, share, and collaborate on code** using **Git**, a version control system.

In simple words:
👉 **Git** tracks code changes
👉 **GitHub** hosts Git repositories online and helps teams work together

---

## Why GitHub is used

GitHub helps developers and DevOps engineers to:

* Track code changes over time
* Collaborate with teams
* Review code before merging
* Manage releases and deployments
* Automate workflows (CI/CD)

---

## How GitHub works (basic flow)

1. You write code on your local system
2. Git tracks changes (`git add`, `git commit`)
3. You push code to GitHub (`git push`)
4. Others pull, review, and contribute (`git pull`, Pull Requests)

---

## Key features of GitHub

### 1. Repository (Repo)

A **repository** is a project folder that contains:

* Source code
* Configuration files
* Documentation
* Git history

Example:

```bash
git clone https://github.com/user/project.git
```

---

### 2. Version Control

GitHub uses **Git** to:

* Save every change as a commit
* Allow rollback to previous versions
* Track who changed what and when

---

### 3. Branching

Branches let you work on features independently.

Example:

```bash
git branch feature-login
git checkout feature-login
```

---

### 4. Pull Requests (PR)

A **Pull Request** is used to:

* Propose changes
* Review code
* Merge branches safely

Flow:

```
feature branch → Pull Request → Review → Merge to main
```

---

### 5. Collaboration

* Multiple developers can work on the same repo
* Code reviews, comments, and approvals
* Team access control

---

### 6. Issues & Projects

* **Issues** → track bugs, tasks, enhancements
* **Projects** → Kanban-style task management

---

### 7. GitHub Actions (CI/CD)

Automate tasks like:

* Build
* Test
* Deploy

Example:

```yaml
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
```

---

### 8. Security & Access Control

* Public & private repositories
* Role-based access (Read, Write, Admin)
* Secrets management

---

## GitHub vs Git (important)

| Git            | GitHub             |
| -------------- | ------------------ |
| Tool           | Platform           |
| Runs locally   | Cloud-based        |
| Tracks changes | Hosts repositories |
| CLI based      | Web + CLI          |

---

## Why GitHub is important for DevOps

As a **DevOps Engineer**, GitHub is central for:

* Source Code Management (SCM)
* CI/CD pipelines
* Infrastructure as Code (Terraform, Helm)
* Collaboration between Dev & Ops

---

## Real-life example

Imagine Google Docs for code:

* Git = “Track changes”
* GitHub = “Shared document online”

---
