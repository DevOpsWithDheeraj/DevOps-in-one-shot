# ⚙️ Build & CI/CD Tools

## 📘 Introduction

CI/CD (Continuous Integration / Continuous Deployment) is the **heart of DevOps**.  
It automates building, testing, and deploying applications to improve reliability, reduce manual errors, and accelerate delivery.

**Key Goals:**
- Automate repetitive tasks
- Ensure faster, reliable releases
- Detect issues early with automated testing
- Deploy to staging/production consistently

---

## 🧩 1. CI/CD Concepts

### 🔹 Continuous Integration (CI)
- Developers frequently **merge code** into a shared repository  
- Automated builds and tests run on each commit  
- Benefits: early bug detection, faster feedback  

### 🔹 Continuous Deployment / Delivery (CD)
- After CI passes, code is **automatically deployed**  
- **Continuous Delivery:** manual approval before deployment  
- **Continuous Deployment:** fully automated deployment to production  

### 🔹 Benefits for DevOps
- Faster time-to-market  
- Reduced manual errors  
- Improved collaboration between Dev & Ops  
- Easy rollback in case of failures  

---

## 🧱 2. Jenkins – CI/CD Server

### 🔹 What is Jenkins?
- Open-source automation server for building, testing, and deploying software  
- Integrates with **Git, Docker, Kubernetes, Ansible, Terraform**, etc.

### 🔹 Jenkins Installation (Linux Example)
```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
````

* Access Jenkins at: `http://<server-ip>:8080`
* Unlock Jenkins using the initial admin password

---

## 🔹 3. Basic Jenkins Job (Freestyle Project)

1. Create a **new Jenkins job → Freestyle Project**
2. Connect **GitHub repository** (URL)
3. Add **Build Steps** (for example, `npm install` / `docker build`)
4. Post-build: Archive artifacts or deploy to server
5. Run job → check console logs

**Example: Build Docker image**

```bash
docker build -t dheeraj/demo-app:latest .
docker run -d -p 8080:80 dheeraj/demo-app:latest
```

---

## 📜 4. Pipeline-as-Code (Jenkinsfile)

* Jenkins pipelines are **stored as code** in your repository
* Allows versioning, repeatability, and collaboration

### Example Jenkinsfile (Declarative)

```groovy
pipeline {
  agent any
  environment {
    IMAGE_NAME = "dheeraj/demo-app"
  }
  stages {
    stage('Checkout') {
      steps { git 'https://github.com/DevOpsWithDheeraj/demo.git' }
    }
    stage('Build Docker') {
      steps { sh 'docker build -t $IMAGE_NAME:latest .' }
    }
    stage('Push Docker') {
      steps { sh 'docker push $IMAGE_NAME:latest' }
    }
    stage('Deploy') {
      steps {
        sh 'ssh ubuntu@server "docker pull $IMAGE_NAME:latest && docker run -d -p 8080:80 $IMAGE_NAME:latest"'
      }
    }
  }
}
```

✅ Fully automates **build → push → deploy** workflow.

---

## ☁️ 5. GitHub Actions – CI/CD for GitHub Repos

* Workflow files in `.github/workflows/*.yml`
* Automates build, test, deployment on events (push, PR, merge)

### Example GitHub Actions Workflow

```yaml
name: Node CI/CD

on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
      - uses: actions/checkout@v4
      
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 18
          
      - name: Install Dependencies
        run: npm install
        
      - name: Run Tests
        run: npm test
        
      - name: Build Docker Image
        run: docker build -t dheeraj/node-app:latest .
        
      - name: Push Docker Image
        run: docker push dheeraj/node-app:latest
```

✅ Automatically builds and pushes Docker image on every push to `main`.

---

## 🧰 6. Advanced CI/CD Concepts

1. **Multi-branch pipelines** – separate pipelines for feature, dev, prod branches
2. **Parameterized builds** – allow dynamic input for builds
3. **Secrets management** – GitHub Secrets, Jenkins Credentials
4. **Notifications** – Slack, Email on build status
5. **Blue-Green / Canary deployments** – zero-downtime deployments
6. **Docker + Kubernetes integration** – CI/CD pipelines deploy containers directly to K8s

### Advanced Example: Deploy to Kubernetes

```groovy
stage('Deploy to K8s') {
  steps {
    sh 'kubectl set image deployment/myapp myapp=dheeraj/demo-app:latest'
    sh 'kubectl rollout status deployment/myapp'
  }
}
```

---

## 🔍 7. DevOps Ports in CI/CD

| Port | Service         | Usage                     |
| ---- | --------------- | ------------------------- |
| 22   | SSH             | Server access, git pushes |
| 80   | HTTP            | Web app access            |
| 443  | HTTPS           | Secure web/app endpoints  |
| 8080 | Jenkins         | CI/CD UI                  |
| 5000 | Docker Registry | Local image registry      |
| 6443 | Kubernetes API  | K8s cluster management    |

> 💡 Knowing ports helps configure **firewalls, security groups, and deployment pipelines**.

---

## 🧩 8. Real-World DevOps CI/CD Example

**Scenario:** Node.js app CI/CD with Jenkins + Docker + Kubernetes

**Steps:**

1. Code pushed to GitHub triggers Jenkins webhook
2. Jenkins builds Docker image (`docker build`)
3. Pushes image to Docker Hub
4. Updates Kubernetes Deployment (`kubectl set image`)
5. Monitors rollout status and triggers alerts if failed

✅ This represents a **complete DevOps CI/CD workflow in production**.

---

## 🏁 Summary

| Level           | Topics Covered                                                                                          |
| --------------- | ------------------------------------------------------------------------------------------------------- |
| 🟢 Beginner     | Jenkins setup, Freestyle jobs, simple builds                                                            |
| 🟡 Intermediate | Jenkins pipelines, GitHub Actions workflows, Docker builds                                              |
| 🔴 Advanced     | Multi-branch pipelines, K8s deployments, blue-green/canary deployment, notifications, secret management |

---

## 🧪 Hands-on Practice Tasks

### 🟢 Beginner

* Install Jenkins and create Freestyle job
* Build and run a Docker container

### 🟡 Intermediate

* Write Jenkinsfile / GitHub Actions workflow
* Push Docker image to Docker Hub

### 🔴 Advanced

* Deploy Docker container to Kubernetes automatically
* Implement multi-stage pipeline with notifications and secrets

---
