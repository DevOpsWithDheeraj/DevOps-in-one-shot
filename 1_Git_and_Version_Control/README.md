# 🧩 Git & Version Control 

## 📘 What is Version Control?

Version Control is a **system that tracks changes** made to files over time, allowing developers to:
- Revert to previous versions
- Compare code changes
- Collaborate effectively on the same project

### 🔹 Types of Version Control
1. **Local VCS** – Only tracks changes locally (e.g., RCS)
2. **Centralized VCS** – One central server, everyone pulls/pushes (e.g., SVN)
3. **Distributed VCS** – Each user has a full copy of repo (e.g., **Git**)

---

## 🧠 What is Git?

**Git** is an open-source **Distributed Version Control System** developed by *Linus Torvalds* (creator of Linux).  
It allows multiple developers to work on the same project **simultaneously**, **without overwriting each other's work**.

### 🔹 Why Git?
- Speed and efficiency  
- Offline commits  
- Branching and merging  
- Secure (SHA-1 checksum-based)  
- Integration with CI/CD tools like Jenkins, GitHub Actions

---

## 🏗️ Git Architecture

```

Working Directory  →  Staging Area  →  Local Repository  →  Remote Repository

````

- **Working Directory:** Your local files
- **Staging Area:** Files marked for commit
- **Local Repository:** Your version history
- **Remote Repository:** Shared repo on GitHub/GitLab/Bitbucket

---

## ⚙️ Git Installation & Setup

### 🔧 Install Git
```bash
sudo apt install git -y     # Ubuntu/Debian
git --version
````

### 🔧 Setup Identity

```bash
git config --global user.name "Dheeraj Singh"
git config --global user.email "dheeraj@example.com"
```

---

## 🧱 Git Basic Commands (Step-by-Step)

| Command                   | Description                    |
| ------------------------- | ------------------------------ |
| `git init`                | Initialize a repository        |
| `git clone <repo_url>`    | Clone an existing repository   |
| `git status`              | Check status of changes        |
| `git add <file>`          | Stage file for commit          |
| `git commit -m "Message"` | Commit changes                 |
| `git log`                 | View commit history            |
| `git push`                | Push commits to remote repo    |
| `git pull`                | Fetch & merge from remote repo |

---

## 🧩 Example 1 – Local Git Project

### Step 1: Initialize a repo

```bash
mkdir devops-demo
cd devops-demo
git init
```

### Step 2: Add files

```bash
echo "Hello DevOps World!" > app.txt
git add app.txt
```

### Step 3: Commit changes

```bash
git commit -m "Initial commit with app.txt"
```

### Step 4: Create GitHub repo and push

```bash
git remote add origin https://github.com/DevOpsWithDheeraj/devops-demo.git
git branch -M main
git push -u origin main
```

✅ You’ve now uploaded your local code to GitHub.

---

## 🌿 Branching & Merging

### 🔹 Create and Switch Branch

```bash
git checkout -b feature/login
```

### 🔹 Make changes and Commit

```bash
echo "Login feature added" >> app.txt
git add .
git commit -m "Added login feature"
```

### 🔹 Merge Branch into Main

```bash
git checkout main
git merge feature/login
```

### 🔹 Delete Branch

```bash
git branch -d feature/login
```

---

## 💡 Example 2 – Real DevOps Scenario

### 🎯 Scenario:

You’re part of a DevOps team managing a web application in production.
You need to create a new Jenkins pipeline, but without affecting the running code.

### ✅ Steps:

1. Create a new branch `ci-pipeline-update`
2. Modify Jenkinsfile
3. Test locally
4. Create Pull Request for review
5. Merge after approval

### Example Commands:

```bash
git checkout -b ci-pipeline-update
vi Jenkinsfile  # Edit your pipeline code
git add Jenkinsfile
git commit -m "Updated Jenkins pipeline for Docker build"
git push origin ci-pipeline-update
```

Then open a **Pull Request (PR)** on GitHub for review.

---

## 🧠 Git Advanced Concepts

### 🔸 Git Stash

Temporarily save uncommitted changes.

```bash
git stash
git pull origin main
git stash pop
```

### 🔸 Git Revert

Undo a commit safely (creates new commit).

```bash
git revert <commit_id>
```

### 🔸 Git Reset

Completely remove commits (use carefully).

```bash
git reset --hard <commit_id>
```

### 🔸 Git Tagging (for releases)

```bash
git tag -a v1.0 -m "First release"
git push origin v1.0
```

### 🔸 Git Cherry-pick

Apply a specific commit from another branch.

```bash
git cherry-pick <commit_id>
```

---

## ⚡ Git Workflow Strategies

### 1. **Git Flow**

* Master → Production code
* Develop → Integration branch
* Feature branches for new development

### 2. **GitHub Flow**

* `main` → deployable branch
* Short-lived feature branches
* Continuous deployment model

### 3. **Trunk-Based Development**

* Single main branch
* Small, frequent commits
* Preferred for CI/CD pipelines

---

## 🌍 Git Integration in DevOps Tools

| Tool               | Integration Purpose                          |
| ------------------ | -------------------------------------------- |
| **Jenkins**        | Triggers pipeline on `git push`              |
| **Ansible**        | Fetches playbooks from Git                   |
| **Docker**         | Uses Git repos for Dockerfile builds         |
| **Kubernetes**     | Uses GitOps (ArgoCD, FluxCD) for deployments |
| **Terraform**      | Source control for `.tf` IaC code            |
| **GitHub Actions** | Runs workflows directly on Git commits       |

---

## 🧪 Example 3 – End-to-End CI/CD Flow

1. Developer commits code to `feature/login`
2. Jenkins detects commit → builds using Maven
3. Docker builds image → pushes to Docker Hub
4. Kubernetes deploys image to staging
5. Git tag created for release → `v2.0`

### Sample Jenkinsfile

```groovy
pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps { git 'https://github.com/DevOpsWithDheeraj/devops-demo.git' }
    }
    stage('Build') {
      steps { sh 'mvn clean package' }
    }
    stage('Docker Build & Push') {
      steps {
        sh 'docker build -t dheeraj/devops-demo:v2.0 .'
        sh 'docker push dheeraj/devops-demo:v2.0'
      }
    }
  }
}
```

---

## 🧾 Summary

| Level               | Concepts Covered                         |
| ------------------- | ---------------------------------------- |
| 🟢 **Beginner**     | Git basics, commits, push/pull, branches |
| 🟡 **Intermediate** | Merge, PR, stash, revert, tag            |
| 🔴 **Advanced**     | Git workflows, CI/CD integration, GitOps |

---

## 🏁 Final Takeaway

Git is the **foundation of DevOps** — every automation pipeline, deployment, or configuration starts from **version-controlled code**.

> 💬 “If it’s not in Git, it doesn’t exist.”

---
