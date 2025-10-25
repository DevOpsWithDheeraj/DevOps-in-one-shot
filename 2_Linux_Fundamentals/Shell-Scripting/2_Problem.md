## 🔹 Example 11: Ping Multiple Servers and Report Status

```python
#!/usr/bin/env python3
import subprocess

servers = ["8.8.8.8", "google.com", "amazon.com", "myserver.internal"]
for server in servers:
    response = subprocess.run(["ping", "-c", "2", server], stdout=subprocess.PIPE, stderr=subprocess.PIPE)
    if response.returncode == 0:
        print(f"{server} ✅ is reachable.")
    else:
        print(f"{server} ❌ is down.")
```

✅ **Use case:** Quickly check connectivity to app servers, DBs, or APIs during outages.

---

## 🔹 Example 12: Automated SSL Certificate Expiry Checker

```python
#!/usr/bin/env python3
import ssl, socket
from datetime import datetime

def ssl_expiry_check(hostname):
    ctx = ssl.create_default_context()
    with socket.create_connection((hostname, 443)) as sock:
        with ctx.wrap_socket(sock, server_hostname=hostname) as ssock:
            cert = ssock.getpeercert()
            exp_date = datetime.strptime(cert['notAfter'], '%b %d %H:%M:%S %Y %Z')
            remaining = exp_date - datetime.utcnow()
            print(f"{hostname}: expires in {remaining.days} days")

hosts = ["google.com", "amazon.com", "yourapp.company.com"]
for host in hosts:
    ssl_expiry_check(host)
```

✅ **Use case:** Prevent production downtime by tracking certificate expiry early.

---

## 🔹 Example 13: Docker Container Monitoring Script

```python
#!/usr/bin/env python3
import subprocess, json

cmd = "docker ps --format '{{json .}}'"
result = subprocess.run(cmd, shell=True, capture_output=True, text=True)

for line in result.stdout.strip().split('\n'):
    container = json.loads(line)
    print(f"Container: {container['Names']}, Status: {container['Status']}")
```

✅ **Use case:** Monitor Docker containers without Docker Desktop UI.

---

## 🔹 Example 14: Automated File Transfer via SCP

```python
#!/usr/bin/env python3
import subprocess

source = "/var/log/myapp.log"
destination = "ec2-user@10.0.0.5:/home/ec2-user/backups/"

cmd = ["scp", source, destination]
subprocess.run(cmd)

print("File transferred successfully ✅")
```

✅ **Use case:** Automatically move logs or artifacts between build and deploy servers.

---

## 🔹 Example 15: Check Jenkins Job Status via API

```python
#!/usr/bin/env python3
import requests
from requests.auth import HTTPBasicAuth

jenkins_url = "http://jenkins.company.com/job/deploy-app/lastBuild/api/json"
user, token = "jenkins_user", "jenkins_token"

resp = requests.get(jenkins_url, auth=HTTPBasicAuth(user, token))
data = resp.json()

print(f"Job Name: {data['fullDisplayName']}")
print(f"Status: {data['result']}")
print(f"Duration: {data['duration'] / 1000}s")
```

✅ **Use case:** Integrate Jenkins build monitoring into internal dashboards or alerts.

---

## 🔹 Example 16: Cleanup Old Files Older than 7 Days

```python
#!/usr/bin/env python3
import os, time

path = "/var/log/myapp"
days = 7
now = time.time()

for f in os.listdir(path):
    file_path = os.path.join(path, f)
    if os.path.isfile(file_path):
        if os.stat(file_path).st_mtime < now - days * 86400:
            os.remove(file_path)
            print(f"Deleted old file: {file_path}")
```

✅ **Use case:** Automatically remove stale log files or temp data.

---

## 🔹 Example 17: Ansible Playbook Runner via Python

```python
#!/usr/bin/env python3
import subprocess

playbook = "/home/devops/playbooks/deploy.yml"
inventory = "/home/devops/inventory.ini"

cmd = ["ansible-playbook", "-i", inventory, playbook]
subprocess.run(cmd)
```

✅ **Use case:** Integrate Python automation scripts with Ansible-based configuration management.

---

## 🔹 Example 18: Simple Web Server Health Check Script

```python
#!/usr/bin/env python3
import requests

urls = ["https://www.google.com", "https://myapp.company.com"]

for url in urls:
    try:
        response = requests.get(url, timeout=5)
        print(f"{url}: {response.status_code}")
    except requests.exceptions.RequestException:
        print(f"{url}: DOWN ❌")
```

✅ **Use case:** Used in synthetic monitoring or pre-deployment sanity checks.

---

## 🔹 Example 19: CPU & Memory Threshold Alert with Slack Notification

```python
#!/usr/bin/env python3
import psutil, requests

cpu = psutil.cpu_percent(1)
mem = psutil.virtual_memory().percent
slack_url = "https://hooks.slack.com/services/XXXX/YYYY/ZZZZ"

if cpu > 80 or mem > 85:
    message = {
        "text": f"⚠️ High Resource Usage: CPU {cpu}% | Memory {mem}%"
    }
    requests.post(slack_url, json=message)
```

✅ **Use case:** Real-time Slack alerts for infra resource usage breaches.

---

## 🔹 Example 20: Kubernetes Pod Auto-Restart on CrashLoopBackOff

```python
#!/usr/bin/env python3
import subprocess, json

cmd = "kubectl get pods -o json"
pods = json.loads(subprocess.run(cmd, shell=True, capture_output=True, text=True).stdout)

for pod in pods["items"]:
    name = pod["metadata"]["name"]
    status = pod["status"]["containerStatuses"][0]["state"]
    if "waiting" in status and status["waiting"]["reason"] == "CrashLoopBackOff":
        print(f"Restarting {name}")
        subprocess.run(f"kubectl delete pod {name}", shell=True)
```

✅ **Use case:** Auto-recovery for misbehaving pods in K8s clusters.

---

## 🧠 Summary — What You’ve Mastered

| Category         | Python Skill Used         | DevOps Use Case                |
| ---------------- | ------------------------- | ------------------------------ |
| Infra Automation | `subprocess`, `os`        | Manage servers, files, logs    |
| Cloud Management | `boto3`, `requests`       | AWS, Jenkins, K8s integrations |
| Monitoring       | `psutil`, `ssl`, `json`   | Health checks, metrics         |
| Notifications    | `smtplib`, `requests`     | Email & Slack alerts           |
| CI/CD            | Git, Jenkins API, Ansible | Pipeline automation            |

---

— and the script executes the corresponding module dynamically?
It would look like a **complete DevOps Python automation project** ready for GitHub portfolio.
