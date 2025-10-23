# 🐍 Python Scripting in DevOps 

## 📘 Introduction

Python scripting in DevOps is used to **automate tasks, manage infrastructure, and integrate tools**.  
It is widely adopted for **CI/CD pipelines, cloud provisioning, monitoring, and system automation**.

**Key DevOps Use Cases for Python:**
- Automating server and deployment tasks  
- CI/CD pipeline scripting (Jenkins, GitHub Actions)  
- Cloud resource provisioning (AWS, Azure, GCP)  
- Log parsing, monitoring, and alerting  
- Infrastructure as Code automation  

---

## 🧩 1. Python Basics for DevOps

### 🔹 Hello World & Variables
```python
print("Hello DevOps Automation!")

server_name = "webserver01"
status = True
print(f"Server: {server_name}, Active: {status}")
````

### 🔹 Conditional Checks & Loops

```python
cpu_usage = 85

if cpu_usage > 80:
    print("Alert: CPU usage high!")
else:
    print("CPU usage normal.")

servers = ["web1", "web2", "db1"]
for server in servers:
    print(f"Checking status of {server}")
```

---

## 🐧 2. Functions & Modules

### 🔹 Functions

```python
def deploy_app(server):
    print(f"Deploying app on {server}")

deploy_app("web1")
```

### 🔹 Modules

```python
import os
# List running processes
print(os.popen("ps -ef").read())
```

---

## 🧰 3. File Handling & Logs

```python
# Write log
with open('devops.log', 'a') as f:
    f.write("Deployment started...\n")

# Read log
with open('devops.log', 'r') as f:
    print(f.read())
```

✅ Python is used to **track logs and automate reporting**.

---

## ⚡ 4. Intermediate DevOps Automation Examples

### 🔹 Automating Linux Commands

```python
import subprocess

# Check disk usage
output = subprocess.run(["df", "-h"], capture_output=True, text=True)
print(output.stdout)
```

### 🔹 Interacting with GitHub API

```python
import requests

repo = "DevOpsWithDheeraj/node-app"
response = requests.get(f"https://api.github.com/repos/{repo}")
print(response.json()['full_name'], response.json()['stargazers_count'])
```

### 🔹 Monitoring & Alerts

```python
import psutil

cpu = psutil.cpu_percent(interval=1)
if cpu > 80:
    print(f"CPU usage high: {cpu}% - Sending alert!")
```

---

## 🐦 5. Advanced DevOps Python Examples

### 🔹 AWS Automation with Boto3

```python
import boto3

ec2 = boto3.client('ec2', region_name='us-east-1')

# Launch EC2 instance
response = ec2.run_instances(
    ImageId='ami-0c02fb55956c7d316',
    InstanceType='t2.micro',
    MinCount=1,
    MaxCount=1
)
print("EC2 Instance launched:", response['Instances'][0]['InstanceId'])
```

### 🔹 CI/CD Pipeline Integration (Jenkins)

```python
import subprocess

# Trigger Jenkins job from Python
jenkins_job_url = "http://jenkins-server/job/deploy-app/build"
subprocess.run(["curl", "-X", "POST", jenkins_job_url])
print("Triggered Jenkins pipeline")
```

### 🔹 Docker Automation

```python
import docker

client = docker.from_env()

# Pull and run container
container = client.containers.run("nginx:latest", detach=True, ports={'80/tcp': 8080})
print(f"Container {container.short_id} running on port 8080")
```

### 🔹 Log Parsing & Reporting

```python
with open('app.log', 'r') as f:
    errors = [line for line in f if "ERROR" in line]

if errors:
    print("Errors found:", errors)
```

---

## 🧩 6. Real-World DevOps Python Scenario

**Scenario:** Automated Deployment & Monitoring Pipeline

1. **Provision Infrastructure:** Python + Boto3 to create EC2 instances
2. **Deploy Application:** Use Python + subprocess to install Docker & start containers
3. **Monitor System:** Python + psutil to check CPU/memory usage and generate alerts
4. **CI/CD Integration:** Trigger Jenkins pipeline automatically using Python script
5. **Logging & Notifications:** Parse logs, send alerts via Slack/email using Python `requests` or `smtplib`

✅ Complete DevOps automation from infrastructure → deployment → monitoring → alerting.

---

## 🏁 7. Hands-on Lab

**Task:** Python DevOps Automation Script

1. Launch EC2 instance using Python + Boto3
2. Install Docker on the instance via SSH + subprocess
3. Pull and run a Docker container (e.g., Nginx)
4. Monitor CPU/memory with `psutil` and log to a file
5. Trigger a Jenkins job for deployment automatically
6. Send Slack/email alert if CPU usage exceeds 80%

---

## 🏁 8. Summary

| Level           | Topics Covered                                                                           |
| --------------- | ---------------------------------------------------------------------------------------- |
| 🟢 Beginner     | Python syntax, variables, loops, functions                                               |
| 🟡 Intermediate | File handling, subprocess commands, API interaction, monitoring                          |
| 🔴 Advanced     | Cloud automation (AWS Boto3), Docker automation, CI/CD integration, log parsing & alerts |

> 💬 “Python scripting empowers DevOps engineers to **automate tasks, integrate tools, and manage infrastructure efficiently**, forming the backbone of modern DevOps pipelines.”

---

