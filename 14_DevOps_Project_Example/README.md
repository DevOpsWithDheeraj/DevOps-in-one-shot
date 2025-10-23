# 🚀 Hands-on DevOps Project 

## 📘 Introduction

A hands-on DevOps project demonstrates **practical application of DevOps principles, tools, and workflows**.  
It integrates **CI/CD, Infrastructure as Code, Configuration Management, Monitoring, Cloud, and DevSecOps** in one pipeline.

**Key Goals of a DevOps Project:**
- Automate deployment and scaling  
- Ensure high availability and reliability  
- Implement monitoring and logging  
- Integrate security and compliance  
- Demonstrate end-to-end DevOps lifecycle  

---

## 🧩 1. Project Overview

**Example Project:** Deploy a **Node.js Web Application** on AWS EC2 with Docker, using Jenkins CI/CD, Ansible for configuration management, Terraform for provisioning, and monitoring with Prometheus/Grafana.  

**Components:**
1. **Source Code:** Node.js app stored in GitHub  
2. **CI/CD:** Jenkins pipeline automates build, test, deploy  
3. **Infrastructure:** Terraform provisions EC2, VPC, Security Groups  
4. **Configuration Management:** Ansible installs Docker, configures app  
5. **Monitoring & Alerts:** Prometheus + Grafana for metrics  
6. **Security:** DevSecOps integration with vulnerability scanning and secrets management  

---

## 🐧 2. Basic Project Steps

### 🔹 Step 1: Code Repository
- Create GitHub repository for Node.js app  
- Organize code with `src/`, `package.json`, `Dockerfile`, `Jenkinsfile`  

### 🔹 Step 2: Dockerize Application
```dockerfile
# Dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
````

### 🔹 Step 3: Terraform for Infrastructure

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"
  tags = { Name = "DevOps-WebApp" }
}
```

### 🔹 Step 4: Ansible Playbook for Configuration

```yaml
- name: Configure Web Server
  hosts: web
  become: yes
  tasks:
    - name: Install Docker
      apt:
        name: docker.io
        state: present
    - name: Run Node.js Docker Container
      docker_container:
        name: nodeapp
        image: node-app:latest
        state: started
        ports:
          - "3000:3000"
```

---

## ⚡ 3. CI/CD Integration

### 🔹 Jenkins Pipeline Example

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { git 'https://github.com/DevOpsWithDheeraj/node-app.git' }
        }
        stage('Build') {
            steps { sh 'docker build -t node-app:latest .' }
        }
        stage('Test') {
            steps { sh 'npm test' }
        }
        stage('Deploy') {
            steps { sh 'ansible-playbook -i hosts.ini playbook.yml' }
        }
    }
}
```

✅ Fully automates build → test → deploy.

---

## 🐦 4. Monitoring & Alerts

### 🔹 Prometheus Configuration

```yaml
scrape_configs:
  - job_name: 'nodeapp'
    static_configs:
      - targets: ['<EC2_PUBLIC_IP>:3000']
```

### 🔹 Grafana Dashboard

* Visualize metrics: CPU, Memory, App Response Time
* Set alerts for high CPU/memory usage

---

## 🧩 5. DevSecOps Integration

* **Code Security:** SonarQube scans Node.js app
* **Dependency Security:** Snyk scans `package.json`
* **Secrets Management:** Store DB passwords in HashiCorp Vault
* **Container Security:** Scan Docker image with Trivy

✅ Security embedded in the CI/CD pipeline.

---

## 🧩 6. Advanced Project Features

* **Blue/Green Deployment:** Reduce downtime by switching environments
* **Rollback:** Automated rollback on failed deployment
* **Infrastructure Scaling:** Auto-scale EC2 instances using AWS Auto Scaling
* **Logging:** Centralized logs using ELK stack
* **Notifications:** Slack/Email alerts on pipeline success/failure

---

## 🏁 7. Real-World DevOps Project Flow

1. **Developer Commit:** Push feature to GitHub (Agile sprint)
2. **CI Pipeline:** Jenkins builds Docker image, runs tests
3. **Code Scanning:** SonarQube & Snyk for vulnerabilities
4. **Infrastructure Provision:** Terraform provisions EC2 & networking
5. **Configuration:** Ansible installs Docker & deploys app
6. **Monitoring & Alerts:** Prometheus & Grafana track metrics; alert via Slack
7. **DevSecOps:** Secrets from Vault, container scan, compliance checks
8. **Feedback:** Logs and metrics used for continuous improvement

---

## 🏁 8. Hands-on Lab

**Task:** Implement End-to-End DevOps Project

1. Setup **GitHub repo** for Node.js app
2. Dockerize the app and push to local/remote registry
3. Write **Terraform scripts** for EC2 and VPC
4. Write **Ansible playbook** to configure servers & run containers
5. Configure **Jenkins pipeline** for CI/CD
6. Integrate **SonarQube & Snyk** for security
7. Setup **Prometheus & Grafana** for monitoring
8. Add **Slack alerts** and logs for pipeline results
9. Test **rollback & blue/green deployment**

✅ Complete DevOps lifecycle with automation, monitoring, security, and CI/CD.

---

## 🏁 9. Summary

| Level           | Topics Covered                                                     |
| --------------- | ------------------------------------------------------------------ |
| 🟢 Beginner     | GitHub repo, Dockerization, basic Terraform & Ansible              |
| 🟡 Intermediate | Jenkins CI/CD, monitoring, security integration                    |
| 🔴 Advanced     | Blue/Green deployment, rollback, scaling, DevSecOps, notifications |

> 💬 “Hands-on DevOps projects combine all tools and practices to create **fully automated, monitored, and secure software delivery pipelines**, demonstrating end-to-end DevOps mastery.”

---

