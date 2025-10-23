# 🚀 DevOps (Beginner to Advanced)

## 📘 What is DevOps?

**DevOps** is a combination of **Development (Dev)** and **Operations (Ops)** — a culture and set of practices that aim to **bridge the gap** between software development and IT operations.  
The goal is to **deliver software faster, more reliably, and with continuous improvement**.

DevOps emphasizes:
- **Collaboration** between teams  
- **Automation** of repetitive tasks  
- **Continuous Integration & Continuous Deployment (CI/CD)**  
- **Monitoring & Feedback loops** for improvement  

---

## 🧭 DevOps Roadmap / Syllabus Overview

1. **Git & Version Control**
2. **Linux Fundamentals**
3. **Networking in DevOps**
4. **Build & CI/CD Tools (Jenkins, GitHub Actions, etc.)**
5. **Artifact Repository (Nexus, JFrog Artifactory)**
6. **Containerization (Docker)**
7. **Container Orchestration (Kubernetes)**
8. **Configuration Management (Ansible, Chef, Puppet)**
9. **Infrastructure as Code (Terraform, CloudFormation)**
10. **Cloud Computing (AWS, Azure, GCP)**
11. **Monitoring & Logging (Prometheus, Grafana, ELK Stack)**
12. **Security & DevSecOps**
13. **Scripting (Shell, Python)**
14. **Agile & ITIL Practices**
15. **Hands-on DevOps Project**

---

## 🧩 1. Git & Version Control

- Version Control concepts: Local, Centralized, Distributed  
- Git basics: init, clone, add, commit, push, pull  
- Branching & merging strategies  
- Pull requests and Git workflows (Gitflow, Trunk-Based)  
- Advanced concepts: stash, revert, reset, cherry-pick, tags  
- Integration with Jenkins, Ansible, Docker, Kubernetes, Terraform, GitHub Actions  

**Hands-on Example:**  
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin <repo_url>
git push -u origin main
````

---

## 🐧 2. Linux Fundamentals

* File system structure: `/`, `/home`, `/etc`, `/var`, `/usr`, `/opt`
* Basic commands: `ls`, `cd`, `cat`, `cp`, `mv`, `rm`, `touch`, `mkdir`
* File permissions: `chmod`, `chown`, `id`, `groups`
* Process management: `ps`, `top`, `htop`, `kill`, `systemctl`
* Disk management: `df -h`, `du -sh`, `lsblk`, `fdisk`, `mount`
* Package management: `apt`, `yum`, `rpm`
* Shell scripting basics: automation with bash
* Monitoring & logs: `uptime`, `free -h`, `journalctl`, `tail -f`
* SSH & remote access: `ssh`, `scp`, key-based authentication

**Hands-on Example:**

```bash
#!/bin/bash
echo "Deploying application..."
git pull origin main
docker-compose down
docker-compose up -d
echo "Deployment complete!"
```

---

## 🌐 3. Networking in DevOps

**Networking is critical** in DevOps for deployment, monitoring, and troubleshooting.

### Key Concepts:

* OSI & TCP/IP model basics
* IP addressing, Subnetting, NAT
* DNS: Domain names, Record types (A, CNAME, MX, TXT)
* HTTP/HTTPS protocols, SSL/TLS
* Load Balancers: ELB, Nginx, HAProxy
* Ports & protocols (80, 443, 22, 8080, etc.)
* Firewalls & Security Groups
* Network troubleshooting commands: `ping`, `traceroute`, `netstat`, `curl`, `nslookup`, `dig`

**Hands-on Example:**

```bash
ping google.com
curl -I https://myapp.com
netstat -tulnp | grep 80
nslookup example.com
```

---

## ⚙️ 4. Build & CI/CD Tools

* CI/CD concepts: Continuous Integration, Continuous Deployment
* Jenkins setup and configuration
* Jenkins pipelines: Freestyle vs Declarative (`Jenkinsfile`)
* GitHub Actions: workflows, jobs, runners, triggers
* Advanced CI/CD: Docker + Kubernetes integration, automated deployments
* Best practices: idempotent pipelines, secrets management, notifications

**Hands-on Example (Jenkinsfile):**

```groovy
pipeline {
  agent any
  stages {
    stage('Checkout') { steps { git 'https://github.com/DevOpsWithDheeraj/demo.git' } }
    stage('Build') { steps { sh 'mvn clean verify' } }
    stage('Docker Build') { steps { sh 'docker build -t dheeraj/demo-app:latest .' } }
    stage('Deploy') { steps { sh 'kubectl set image deployment/demo demo=dheeraj/demo-app:latest' } }
  }
}
```

---

## 📦 5. Artifact Repository

* Tools: Nexus, JFrog Artifactory
* Storing build artifacts (JAR, WAR, Docker images)
* Versioning and promotion between dev/staging/prod

---

## 🐳 6. Containerization

* Docker concepts: Images, Containers, Volumes, Networks
* Dockerfile, multi-stage builds, Docker Compose
* Pushing images to Docker Hub / private registry

---

## ☸️ 7. Container Orchestration

* Kubernetes architecture: Master & Worker nodes
* Pods, Deployments, ReplicaSets, Services, Ingress
* ConfigMaps, Secrets, Volumes
* Helm package manager for Kubernetes

---

## ⚒️ 8. Configuration Management

* Tools: Ansible, Puppet, Chef
* Concepts: Playbooks, Roles, Modules, Idempotency
* Ansible Vault for secrets management
* Integration with CI/CD pipelines

---

## 🌍 9. Infrastructure as Code (IaC)

* Tools: Terraform, CloudFormation
* Terraform concepts: Provider, Resource, State, Modules
* Automating cloud infrastructure provisioning
* IaC integration with CI/CD pipelines

---

## ☁️ 10. Cloud Computing

* AWS, Azure, GCP basics
* Key services for DevOps:

  * Compute: EC2, Lambda
  * Storage: S3, EBS, EFS
  * Networking: VPC, ELB, Route 53
  * Security: IAM, KMS
  * DevOps services: CodeCommit, CodeBuild, CodeDeploy, CodePipeline
  * Monitoring: CloudWatch

---

## 📊 11. Monitoring & Logging

* Tools: Prometheus, Grafana, ELK Stack
* Metrics collection, dashboards, alerts
* Log aggregation and analysis

---

## 🔐 12. Security & DevSecOps

* Secrets management: Vault, AWS KMS
* Security scanning: Trivy, SonarQube
* SSL/TLS, HTTPS, secure connections
* Shift-left security in DevOps

---

## 🧪 13. Scripting

* Bash scripting for automation
* Python scripting for APIs, cloud provisioning, IaC

---

## 🧠 14. Agile & ITIL Practices

* Agile methodology (Scrum, Kanban)
* ITIL processes: Incident, Problem, Change, Release Management
* Site Reliability Engineering (SRE) basics

---

## 🏗️ 15. Hands-on DevOps Project

**Objective:** Automate deployment of a web application end-to-end.

**Tech Stack Example:**

* Source Control: Git
* CI/CD: Jenkins / GitHub Actions
* Artifact: Nexus / Docker Registry
* Containerization: Docker
* Orchestration: Kubernetes
* IaC: Terraform
* Cloud: AWS
* Monitoring: Prometheus & Grafana

**Outcome:** Fully automated, monitored production-ready application pipeline.

---

## 🏁 Conclusion

DevOps is a **culture, mindset, and continuous learning journey**.
By mastering the tools, Linux fundamentals, networking, and automation practices above, you’ll be ready for any **DevOps Engineer** role in top organizations.

---
