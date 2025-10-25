## 🔹 Example 21: Terraform Plan and Apply Automation

```python
#!/usr/bin/env python3
import subprocess

terraform_dir = "/home/devops/infra"

subprocess.run(["terraform", "-chdir", terraform_dir, "init"])
subprocess.run(["terraform", "-chdir", terraform_dir, "plan", "-out=tfplan"])
apply = input("Apply the Terraform plan? (yes/no): ")

if apply.lower() == "yes":
    subprocess.run(["terraform", "-chdir", terraform_dir, "apply", "tfplan"])
    print("✅ Infrastructure applied successfully.")
else:
    print("❌ Terraform apply skipped.")
```

✅ **Use case:** Automates Terraform provisioning from a single script — used in Jenkins or local pipelines.

---

## 🔹 Example 22: Git Branch Cleanup Script

```python
#!/usr/bin/env python3
import subprocess

branches = subprocess.run(["git", "branch", "--merged"], capture_output=True, text=True)
for branch in branches.stdout.splitlines():
    branch = branch.strip()
    if branch not in ["main", "master"]:
        print(f"Deleting merged branch: {branch}")
        subprocess.run(["git", "branch", "-d", branch])
```

✅ **Use case:** Keeps Git repositories clean by deleting merged local branches.

---

## 🔹 Example 23: Real-Time Log Tail and Alert

```python
#!/usr/bin/env python3
import time, re

log_file = "/var/log/myapp/error.log"

with open(log_file) as f:
    f.seek(0, 2)
    while True:
        line = f.readline()
        if not line:
            time.sleep(1)
            continue
        if re.search("ERROR|Exception|CRITICAL", line):
            print(f"⚠️ Alert: {line.strip()}")
```

✅ **Use case:** Real-time monitoring of logs during production incidents or deployments.

---

## 🔹 Example 24: Docker Image Cleanup Script

```python
#!/usr/bin/env python3
import subprocess

print("Cleaning up old Docker images...")
subprocess.run(["docker", "image", "prune", "-af"])
print("✅ Docker cleanup completed.")
```

✅ **Use case:** Regularly clear unused Docker images and free up build server space.

---

## 🔹 Example 25: AWS S3 Sync Automation

```python
#!/usr/bin/env python3
import subprocess

local_path = "/home/devops/artifacts"
s3_bucket = "s3://company-backups/artifacts"

subprocess.run(["aws", "s3", "sync", local_path, s3_bucket])
print("✅ Synced local artifacts to S3 successfully.")
```

✅ **Use case:** Upload build artifacts or logs from Jenkins to AWS S3 automatically.

---

## 🔹 Example 26: Kubernetes Deployment Rollout Status

```python
#!/usr/bin/env python3
import subprocess

deployment = "webapp"
namespace = "prod"

cmd = f"kubectl rollout status deployment/{deployment} -n {namespace}"
subprocess.run(cmd, shell=True)
```

✅ **Use case:** Verifies deployment progress and waits until rollout completes.

---

## 🔹 Example 27: CI/CD Artifact Versioning Script

```python
#!/usr/bin/env python3
import os
from datetime import datetime

artifact_dir = "/builds"
build_version = datetime.now().strftime("%Y%m%d%H%M")
artifact_name = f"myapp_{build_version}.tar.gz"
os.system(f"tar -czf {artifact_dir}/{artifact_name} /app/source")

print(f"✅ Artifact created: {artifact_name}")
```

✅ **Use case:** Automatically versions build artifacts with timestamps.

---

## 🔹 Example 28: AWS Lambda Deployment Script

```python
#!/usr/bin/env python3
import boto3, zipfile, os

lambda_client = boto3.client('lambda')
zip_file = '/tmp/lambda.zip'

with zipfile.ZipFile(zip_file, 'w') as zf:
    zf.write('lambda_function.py')

response = lambda_client.update_function_code(
    FunctionName='myLambdaFunction',
    ZipFile=open(zip_file, 'rb').read()
)
print(f"✅ Lambda deployed, version: {response['Version']}")
```

✅ **Use case:** Automate AWS Lambda function updates during deployment.

---

## 🔹 Example 29: Security Audit – Find World Writable Files

```python
#!/usr/bin/env python3
import subprocess

print("Scanning for world-writable files...")
cmd = "find / -type f -perm -0002 2>/dev/null"
result = subprocess.run(cmd, shell=True, capture_output=True, text=True)

for line in result.stdout.splitlines():
    print(line)
```

✅ **Use case:** Security compliance audit in Linux production servers.

---

## 🔹 Example 30: Automatic Restart for Failed Jenkins Builds

```python
#!/usr/bin/env python3
import requests
from requests.auth import HTTPBasicAuth

JENKINS_URL = "http://jenkins.company.com/job/build-app/lastBuild/api/json"
USERNAME = "jenkins_user"
TOKEN = "jenkins_token"

resp = requests.get(JENKINS_URL, auth=HTTPBasicAuth(USERNAME, TOKEN)).json()

if resp["result"] == "FAILURE":
    print("Build failed. Restarting...")
    restart_url = "http://jenkins.company.com/job/build-app/build"
    requests.post(restart_url, auth=HTTPBasicAuth(USERNAME, TOKEN))
else:
    print("✅ Build succeeded.")
```

✅ **Use case:** Self-healing Jenkins job that restarts automatically on failure.

---

## 🧩 Bonus: Example 31 – Integrated “Deploy & Verify” Workflow

```python
#!/usr/bin/env python3
import subprocess, requests, time

print("🚀 Starting deployment...")
subprocess.run(["ansible-playbook", "-i", "inventory.ini", "deploy.yml"])

print("⏳ Waiting for app to start...")
time.sleep(15)

url = "https://myapp.company.com/health"
resp = requests.get(url)
if resp.status_code == 200:
    print("✅ Deployment verified successfully!")
else:
    print(f"❌ Deployment failed. Status code: {resp.status_code}")
```

✅ **Use case:** Automates deploy + post-deploy validation — a must-have CI/CD step.

---

## 🧠 What You’ve Mastered So Far (21–30)

| Category          | Focus Area                    | Python Libraries / Tools    |
| ----------------- | ----------------------------- | --------------------------- |
| Infra Automation  | Terraform, Ansible, AWS       | `subprocess`, `boto3`       |
| CI/CD Integration | Jenkins, Git, S3, Lambda      | `requests`, `os`, `zipfile` |
| Monitoring        | Log parsing, Alerts, Rollouts | `re`, `time`, `subprocess`  |
| Security          | File permissions & audits     | `find`, `os`                |
| Reliability       | Auto restart / self-healing   | `requests`, `HTTPBasicAuth` |

---
