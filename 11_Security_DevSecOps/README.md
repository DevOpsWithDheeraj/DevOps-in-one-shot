# 🛡️ Security & DevSecOps 

## 📘 Introduction

**DevSecOps** integrates **security practices into DevOps processes** from the beginning, rather than as a final step.  
It ensures that **applications, infrastructure, and pipelines** are secure, compliant, and resilient.

**Key Principles:**
- Shift security **left** in development lifecycle  
- Automate security checks in CI/CD pipelines  
- Continuous monitoring & vulnerability management  
- Collaboration between Dev, Ops, and Security teams  

---

## 🧩 1. Security in DevOps

| Aspect | Description | Tools / Examples |
|--------|-------------|-----------------|
| **Code Security** | Detect vulnerabilities in code | SonarQube, Snyk |
| **Dependency Security** | Scan open-source libraries | OWASP Dependency-Check, Whitesource |
| **Infrastructure Security** | Secure servers, network, cloud resources | Terraform + AWS IAM policies, Security Groups |
| **Secrets Management** | Store API keys, passwords securely | HashiCorp Vault, AWS Secrets Manager |
| **Compliance & Auditing** | Maintain logs & audit trails | CloudTrail, ELK Stack |

---

## 🐧 2. DevSecOps Lifecycle

1. **Plan:** Security requirements & threat modeling  
2. **Code:** Secure coding practices, static code analysis  
3. **Build:** Integrate security scanning tools in CI/CD  
4. **Test:** Vulnerability assessment, penetration testing  
5. **Release:** Ensure signed artifacts, secure deployment  
6. **Deploy:** Hardened servers, encrypted communication  
7. **Operate:** Monitor, log, and alert on security events  
8. **Feedback:** Continuous improvement  

---

## 🧰 3. Basic Example – Integrating Security in CI/CD

### 🔹 Jenkins + SonarQube

**Jenkinsfile:**
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { git 'https://github.com/DevOpsWithDheeraj/node-app.git' }
        }
        stage('Build') {
            steps { sh 'npm install' }
        }
        stage('Static Code Analysis') {
            steps {
                sh 'sonar-scanner -Dsonar.projectKey=node-app -Dsonar.sources=.'
            }
        }
        stage('Test') {
            steps { sh 'npm test' }
        }
        stage('Deploy') {
            steps { sh './deploy.sh' }
        }
    }
}
````

✅ Automatically scans code for vulnerabilities before deployment.

---

## ⚡ 4. Advanced DevSecOps Concepts

### 🔹 Infrastructure Security

* **Terraform + AWS IAM Policies**

```hcl
resource "aws_iam_role" "devops_role" {
  name = "devops-role"
  assume_role_policy = jsonencode({
    Version = "2012-10-17",
    Statement = [{
      Action = "sts:AssumeRole",
      Effect = "Allow",
      Principal = { Service = "ec2.amazonaws.com" }
    }]
  })
}
```

### 🔹 Secrets Management

* **HashiCorp Vault Example**

```bash
vault kv put secret/devops username='admin' password='P@ssw0rd'
vault kv get secret/devops
```

### 🔹 Container Security

* Scan Docker images with **Trivy**

```bash
trivy image myapp:latest
```

### 🔹 Policy as Code

* **OPA (Open Policy Agent)**

```rego
package kubernetes.admission

deny[msg] {
    input.kind.kind == "Pod"
    container := input.spec.containers[_]
    container.image == "latest"
    msg = "Avoid using latest tag in production"
}
```

### 🔹 Runtime Security

* Monitor workloads using **Falco** or **Aqua Security**
* Detect abnormal behavior in containers & Kubernetes clusters

---

## 🧩 5. Real-World DevSecOps Example

**Scenario:** Node.js app deployed in Kubernetes

1. **Code Security:** SonarQube + ESLint
2. **Dependency Security:** Snyk scans `package.json`
3. **CI/CD Integration:** Jenkins pipeline includes security scans
4. **Infrastructure Security:** Terraform provisions VPC, Security Groups, IAM roles
5. **Secrets:** Vault stores DB passwords & API keys
6. **Container Security:** Trivy scans Docker images
7. **Runtime Security:** Falco monitors Kubernetes pods
8. **Monitoring & Logging:** CloudWatch + ELK Stack for audit logs

✅ Security is integrated **at every stage** of the DevOps lifecycle.

---

## 🏁 6. Hands-on Lab

**Task:** Implement DevSecOps pipeline

1. Setup **Jenkins pipeline** with code scan (SonarQube)
2. Integrate **dependency check** using Snyk
3. Deploy infrastructure with **Terraform + IAM policies**
4. Store secrets in **Vault**
5. Scan Docker images with **Trivy**
6. Deploy to Kubernetes with **policy as code** checks via OPA
7. Configure alerts for suspicious activity via **Falco**

---

## 🏁 7. Summary

| Level           | Topics Covered                                                                    |
| --------------- | --------------------------------------------------------------------------------- |
| 🟢 Beginner     | DevSecOps overview, code & dependency scanning, basic pipeline integration        |
| 🟡 Intermediate | Infrastructure security, secrets management, container scanning                   |
| 🔴 Advanced     | Policy as code, runtime security, Kubernetes security, CI/CD integrated DevSecOps |

> 💬 “DevSecOps embeds **security in every step of DevOps**, ensuring faster delivery without compromising safety, compliance, and reliability.”

---
