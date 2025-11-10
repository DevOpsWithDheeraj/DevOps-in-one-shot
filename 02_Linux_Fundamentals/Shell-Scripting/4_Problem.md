## 🔹 Example 31: Cross-Cloud Backup (AWS → Azure)

```python
#!/usr/bin/env python3
import subprocess

aws_s3 = "s3://company-backups/db-dumps/"
azure_blob = "https://company.blob.core.windows.net/backups/"

# Step 1: Sync AWS to local
subprocess.run(["aws", "s3", "sync", aws_s3, "/tmp/backups"])

# Step 2: Upload to Azure Blob
subprocess.run(["azcopy", "sync", "/tmp/backups", azure_blob, "--recursive"])

print("✅ Cross-cloud backup completed successfully (AWS → Azure)")
```

✅ **Use case:** Multi-cloud resilience — ensures your data is backed up on two providers.

---

## 🔹 Example 32: Kubernetes Secret Rotation Script

```python
#!/usr/bin/env python3
import subprocess, secrets

namespace = "production"
secret_name = "db-credentials"
new_password = secrets.token_urlsafe(16)

subprocess.run([
    "kubectl", "create", "secret", "generic", secret_name,
    f"--from-literal=password={new_password}", "-n", namespace, "--dry-run=client", "-o", "yaml"
], stdout=open("/tmp/new_secret.yaml", "w"))

subprocess.run(["kubectl", "apply", "-f", "/tmp/new_secret.yaml"])
print(f"✅ Rotated secret '{secret_name}' in {namespace} namespace.")
```

✅ **Use case:** Regular password/key rotation for security compliance.

---

## 🔹 Example 33: Jenkins Job Audit Logging

```python
#!/usr/bin/env python3
import requests
from datetime import datetime
from requests.auth import HTTPBasicAuth

jenkins_url = "http://jenkins.company.com/api/json"
user, token = "jenkins_user", "jenkins_token"

resp = requests.get(jenkins_url, auth=HTTPBasicAuth(user, token)).json()

with open("/var/log/jenkins_audit.log", "a") as log:
    log.write(f"\n\n=== Jenkins Audit @ {datetime.now()} ===\n")
    for job in resp["jobs"]:
        log.write(f"Job: {job['name']} | URL: {job['url']}\n")
print("✅ Jenkins job audit logged.")
```

✅ **Use case:** Maintain compliance history for Jenkins job configurations.

---

## 🔹 Example 34: AWS Auto Scaling Trigger via CloudWatch

```python
#!/usr/bin/env python3
import boto3

cw = boto3.client("cloudwatch")
asg = boto3.client("autoscaling")

alarms = cw.describe_alarms(AlarmNamePrefix="HighCPU")["MetricAlarms"]

for alarm in alarms:
    if alarm["StateValue"] == "ALARM":
        print(f"Triggering scaling action for {alarm['AlarmName']}")
        asg.set_desired_capacity(AutoScalingGroupName="webapp-asg", DesiredCapacity=5)
```

✅ **Use case:** Python-driven auto-scaling based on custom CloudWatch metrics.

---

## 🔹 Example 35: Ansible + Python Orchestration

```python
#!/usr/bin/env python3
import subprocess

servers = ["web01", "db01", "cache01"]
playbook = "deploy_app.yml"

for host in servers:
    print(f"Deploying to {host}...")
    subprocess.run(["ansible-playbook", playbook, "-l", host])
print("✅ All servers deployed successfully.")
```

✅ **Use case:** Partial, targeted orchestration using Python logic with Ansible.

---

## 🔹 Example 36: Automated CI/CD Log Archiving to S3

```python
#!/usr/bin/env python3
import os, subprocess, datetime

jenkins_logs = "/var/log/jenkins"
s3_bucket = "s3://company-ci-logs"
date = datetime.datetime.now().strftime("%Y-%m-%d")

archive = f"/tmp/jenkins_logs_{date}.tar.gz"
os.system(f"tar -czf {archive} {jenkins_logs}")
subprocess.run(["aws", "s3", "cp", archive, s3_bucket])

print("✅ Jenkins logs archived to S3 successfully.")
```

✅ **Use case:** Log retention and compliance automation for CI/CD systems.

---

## 🔹 Example 37: Kubernetes Cluster Node Health Reporter

```python
#!/usr/bin/env python3
import subprocess, json

cmd = "kubectl get nodes -o json"
nodes = json.loads(subprocess.run(cmd, shell=True, capture_output=True, text=True).stdout)

for node in nodes["items"]:
    name = node["metadata"]["name"]
    status = node["status"]["conditions"][-1]["type"]
    print(f"Node: {name} | Status: {status}")
```

✅ **Use case:** Monitor node readiness and detect failures proactively.

---

## 🔹 Example 38: Multi-Environment Deployment Selector

```python
#!/usr/bin/env python3
import subprocess

env = input("Select environment (dev/stage/prod): ")

playbook = f"deploy_{env}.yml"
subprocess.run(["ansible-playbook", playbook])

print(f"✅ Deployment executed in {env} environment.")
```

✅ **Use case:** Safe and dynamic deployment selection in shared DevOps pipelines.

---

## 🔹 Example 39: Security – Check SSH Logins from Unknown IPs

```python
#!/usr/bin/env python3
import re

log_file = "/var/log/auth.log"
trusted_ips = ["192.168.1.10", "10.0.0.5"]

with open(log_file, "r") as f:
    for line in f:
        match = re.search(r"sshd.*Accepted.*from (\d+\.\d+\.\d+\.\d+)", line)
        if match:
            ip = match.group(1)
            if ip not in trusted_ips:
                print(f"⚠️ Unauthorized login from {ip}")
```

✅ **Use case:** Detect potential intrusion or unauthorized SSH logins.

---

## 🔹 Example 40: Auto-Healing Pod Replacement + Slack Alert

```python
#!/usr/bin/env python3
import subprocess, json, requests

cmd = "kubectl get pods -o json"
pods = json.loads(subprocess.run(cmd, shell=True, capture_output=True, text=True).stdout)
slack_url = "https://hooks.slack.com/services/XXXX/YYYY/ZZZZ"

for pod in pods["items"]:
    name = pod["metadata"]["name"]
    status = pod["status"]["phase"]
    if status == "Failed":
        subprocess.run(f"kubectl delete pod {name}", shell=True)
        requests.post(slack_url, json={"text": f"💥 Pod {name} failed and was auto-deleted."})
```

✅ **Use case:** Self-healing cluster automation with proactive notifications.

---

## 🧠 Summary: What You’ve Built (31–40)

| Category             | Automation Type      | Technologies Used               |
| -------------------- | -------------------- | ------------------------------- |
| Cloud Resilience     | Backup, Scaling      | AWS, Azure, boto3, azcopy       |
| Kubernetes Ops       | Secrets, Pods, Nodes | kubectl, subprocess, json       |
| CI/CD Governance     | Audit, Log retention | Jenkins API, S3                 |
| Security             | SSH, Secret rotation | re, subprocess, Slack alerts    |
| Hybrid Orchestration | Ansible + Terraform  | ansible-playbook, terraform CLI |

---
