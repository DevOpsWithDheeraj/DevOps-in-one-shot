## 🧠 **41. Automated Backup to AWS S3**

```python
import boto3
import os
from datetime import datetime

backup_dir = "/var/log"
bucket_name = "my-backup-bucket"
s3 = boto3.client('s3')

for file in os.listdir(backup_dir):
    full_path = os.path.join(backup_dir, file)
    if os.path.isfile(full_path):
        key = f"backups/{datetime.now().strftime('%Y-%m-%d')}/{file}"
        s3.upload_file(full_path, bucket_name, key)
        print(f"Uploaded {file} to S3 bucket {bucket_name}")
```

**💡Use case:** Automates log or config file backups to S3 — crucial in AWS-based production environments.

---

## ⚙️ **42. Docker Container Health Monitor**

```python
import subprocess, json

result = subprocess.run(["docker", "ps", "--format", "{{.Names}}"], capture_output=True, text=True)
containers = result.stdout.strip().split('\n')

for c in containers:
    inspect = subprocess.run(["docker", "inspect", c], capture_output=True, text=True)
    data = json.loads(inspect.stdout)[0]
    health = data.get("State", {}).get("Health", {}).get("Status", "N/A")
    print(f"Container: {c}, Health: {health}")
```

**💡Use case:** Checks and reports Docker container health — can be tied to alerting systems.

---

## 🧩 **43. Check Jenkins Build Status via API**

```python
import requests

jenkins_url = "http://localhost:8080/job/devops-pipeline/lastBuild/api/json"
user, token = "admin", "jenkins-api-token"

r = requests.get(jenkins_url, auth=(user, token))
data = r.json()
print(f"Build #{data['number']} Status: {data['result']}")
```

**💡Use case:** Monitors Jenkins job results — useful for CI/CD dashboards or ChatOps alerts.

---

## 📦 **44. Kubernetes Pod Auto-Restarter**

```python
import subprocess

pods = subprocess.getoutput("kubectl get pods --no-headers").splitlines()
for pod in pods:
    name, status = pod.split()[:2]
    if status not in ("Running", "Completed"):
        subprocess.run(["kubectl", "delete", "pod", name])
        print(f"Restarted pod {name}")
```

**💡Use case:** Automatically restarts failed pods — ensures high availability of microservices.

---

## 🧮 **45. Generate Infrastructure Cost Report**

```python
import boto3

ce = boto3.client('ce', region_name='us-east-1')
response = ce.get_cost_and_usage(
    TimePeriod={'Start': '2025-10-01', 'End': '2025-10-31'},
    Granularity='DAILY',
    Metrics=['UnblendedCost']
)

for day in response['ResultsByTime']:
    print(day['TimePeriod']['Start'], ":", day['Total']['UnblendedCost']['Amount'])
```

**💡Use case:** Tracks AWS daily costs — part of FinOps (Financial Operations) automation.

---

## 🔁 **46. Auto Restart Failed Systemd Services**

```python
import subprocess

services = subprocess.getoutput("systemctl list-units --type=service --state=failed --no-legend").splitlines()
for s in services:
    name = s.split()[0]
    subprocess.run(["sudo", "systemctl", "restart", name])
    print(f"Restarted failed service: {name}")
```

**💡Use case:** Ensures system services recover automatically from failures.

---

## 🧰 **47. Automated SSL Certificate Expiry Alert**

```python
import ssl, socket
from datetime import datetime

hostname = "example.com"
ctx = ssl.create_default_context()
with socket.create_connection((hostname, 443)) as sock:
    with ctx.wrap_socket(sock, server_hostname=hostname) as ssock:
        cert = ssock.getpeercert()
        expiry = datetime.strptime(cert['notAfter'], "%b %d %H:%M:%S %Y %Z")
        days_left = (expiry - datetime.now()).days
        print(f"{hostname} SSL expires in {days_left} days")
```

**💡Use case:** Prevents downtime by alerting before SSL expiration — integrates with cron/email alerts.

---

## 📈 **48. Server Uptime & Resource Health Log**

```python
import psutil, platform, datetime

print("System:", platform.node())
print("Uptime (sec):", datetime.datetime.now() - datetime.datetime.fromtimestamp(psutil.boot_time()))
print("CPU Usage:", psutil.cpu_percent(2), "%")
print("Memory Usage:", psutil.virtual_memory().percent, "%")
print("Disk Usage:", psutil.disk_usage('/').percent, "%")
```

**💡Use case:** Monitors Linux server health metrics — often used in automation dashboards.

---

## 🧹 **49. Log Rotation Script**

```python
import os, shutil
from datetime import datetime

log_dir = "/var/log/myapp"
archive_dir = "/var/log/myapp/archive"
os.makedirs(archive_dir, exist_ok=True)

for log_file in os.listdir(log_dir):
    if log_file.endswith(".log"):
        src = os.path.join(log_dir, log_file)
        dst = os.path.join(archive_dir, f"{log_file}_{datetime.now():%Y%m%d}")
        shutil.move(src, dst)
        print(f"Rotated {log_file}")
```

**💡Use case:** Keeps log directories clean and organized — common in batch/ETL servers.

---

## 🔐 **50. Check SSH Connectivity Across Servers**

```python
import paramiko

servers = ["10.0.0.1", "10.0.0.2"]
user = "ec2-user"
key_path = "/home/ec2-user/.ssh/id_rsa"

for host in servers:
    try:
        key = paramiko.RSAKey.from_private_key_file(key_path)
        client = paramiko.SSHClient()
        client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        client.connect(host, username=user, pkey=key)
        print(f"✅ Connected to {host}")
    except Exception as e:
        print(f"❌ Failed to connect {host}: {e}")
    finally:
        client.close()
```

**💡Use case:** Validates SSH connectivity — useful for Ansible pre-checks or infra audits.

---

Would you like me to continue with **Examples 51–60**, focusing on **Python automation for CI/CD pipelines, GitHub Actions, Terraform, and Ansible integration**?
