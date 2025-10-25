# 🐍 Python Shell Scripting for DevOps

Python Shell Scripting allows you to automate repetitive **system admin, CI/CD, monitoring, and deployment** tasks by combining **Python’s power** with **Linux shell commands**.


---

## 🔹 Example 1: System Health Monitoring Script

```python
#!/usr/bin/env python3
import os
import psutil
import platform
from datetime import datetime

def system_health():
    print(f"System Report: {platform.node()}")
    print(f"Time: {datetime.now()}")
    print(f"OS: {platform.system()} {platform.release()}")
    print("-" * 50)
    print(f"CPU Usage: {psutil.cpu_percent()}%")
    print(f"Memory Usage: {psutil.virtual_memory().percent}%")
    print(f"Disk Usage: {psutil.disk_usage('/').percent}%")
    print(f"Network: {psutil.net_if_addrs()}")

    for interface_name, interface_addresses in psutil.net_if_addrs().items():
      for address in interface_addresses:
         if address.family == 2:  # AF_INET (IPv4)
            print(f"{interface_name}: {address.address}")

if __name__ == "__main__":
    system_health()
```

✅ **Use case:** Run in Jenkins pipeline or cron job to log system health of build servers.

---

### 🧩 Line-by-Line Explanation

#### `#!/usr/bin/env python3`

* This is called a **shebang line**.
* It tells the system to **execute the script using Python 3**.
* Useful when running the script directly in Linux/Unix (e.g., `./sys_health.py`).

---

#### `import os`

* Imports the **os module**, which allows interaction with the **operating system**.
* (Note: In this script, `os` isn’t directly used, but it’s often used for file/directory and environment operations.)

---

#### `import psutil`

* Imports the **psutil (process and system utilities)** library.
* It provides system information like **CPU usage, memory, disk, network stats**, and more.

---

#### `import platform`

* Imports the **platform module**, which gives details about the **underlying OS, version, and hardware**.

---

#### `from datetime import datetime`

* Imports the **datetime** class from the **datetime** module.
* Used to fetch the **current date and time** for the report.

---

#### `def system_health():`

* Defines a **function** named `system_health`.
* The main logic for collecting and displaying system metrics resides here.

---

#### `print(f"System Report: {platform.node()}")`

* Prints a heading with the **system’s hostname** (machine name).
* `platform.node()` → returns the system’s **network name** (e.g., `my-laptop`, `server01`).

---

#### `print(f"Time: {datetime.now()}")`

* Prints the **current timestamp** when the script runs.
* `datetime.now()` → returns the current **local date and time**.

---

#### `print(f"OS: {platform.system()} {platform.release()}")`

* Prints the **operating system name and version**.
* `platform.system()` → e.g., `'Windows'`, `'Linux'`, `'Darwin'`
* `platform.release()` → e.g., `'10'`, `'5.15.0-100-generic'`

---

#### `print("-" * 50)`

* Prints a **separator line** made of 50 hyphens for better readability.

  ```
  --------------------------------------------------
  ```

---

#### `print(f"CPU Usage: {psutil.cpu_percent()}%")`

* Shows the **CPU utilization percentage** at that moment.
* `psutil.cpu_percent()` measures how much of the CPU is being used.

---

#### `print(f"Memory Usage: {psutil.virtual_memory().percent}%")`

* Displays the **percentage of memory (RAM)** currently in use.
* `psutil.virtual_memory()` returns a memory stats object; `.percent` extracts the usage percentage.

---

#### `print(f"Disk Usage: {psutil.disk_usage('/').percent}%")`

* Shows **disk space usage** for the root directory `/` (main partition).
* On Windows, you could replace `'/'` with `'C:\\'`.

---

#### `print(f"Network: {psutil.net_if_addrs()}")`

* Displays **network interface addresses** (like IPs, MAC addresses, etc.) for each adapter.
* It prints a dictionary mapping each network interface (e.g., `eth0`, `Wi-Fi`) to its addresses.

---

##### ✅ **Simplified Version (Show Only IPv4 Addresses)**

```python
import psutil

for interface_name, interface_addresses in psutil.net_if_addrs().items():
    for address in interface_addresses:
        if address.family == 2:  # AF_INET (IPv4)
            print(f"{interface_name}: {address.address}")
```

---

### 🧩 **Explanation**

* `psutil.net_if_addrs()` → gives a dictionary of interfaces and their addresses.
* `address.family == 2` → means it's an **IPv4** address (AF_INET).
* `address.address` → actual IP address string.

---

### 🖥️ **Example Output**

```
Ethernet: 192.168.1.10
Wi-Fi: 192.168.0.105
Loopback Pseudo-Interface 1: 127.0.0.1
```

---

#### `if __name__ == "__main__":`

* A **Python convention** that checks if the script is run **directly** (not imported as a module).
* Ensures that the following code runs only when you execute the script yourself.

---

#### `system_health()`

* Calls the `system_health()` function.
* When the script runs, this triggers all the print statements above to generate the report.

---

### 🖥️ Example Output

```
System Report: my-laptop
Time: 2025-10-25 14:22:08.123456
OS: Windows 10
--------------------------------------------------
CPU Usage: 23.4%
Memory Usage: 68.9%
Disk Usage: 42.1%
Network: {'Wi-Fi': [snicaddr(family=2, address='192.168.1.10', ...)], ...}
```

---

## 🔹 Example 2: Check Service Status and Restart if Down

```python
#!/usr/bin/env python3
import subprocess

services = ["nginx", "sshd", "docker"]

for service in services:
    result = subprocess.run(["systemctl", "is-active", service], capture_output=True, text=True)
    if "inactive" in result.stdout or "failed" in result.stdout:
        print(f"{service} is down. Restarting...")
        subprocess.run(["systemctl", "restart", service])
    else:
        print(f"{service} is running fine.")
```

✅ **Use case:** Health-check for DevOps-managed services.

---

## 🔹 Example 3: Disk Usage Alert Script

```python
#!/usr/bin/env python3
import shutil
import smtplib
from email.message import EmailMessage

def check_disk_usage():
    total, used, free = shutil.disk_usage("/")
    percent_used = used / total * 100
    if percent_used > 80:
        send_alert(percent_used)

def send_alert(usage):
    msg = EmailMessage()
    msg["Subject"] = "⚠️ Disk Usage Alert"
    msg["From"] = "admin@server.com"
    msg["To"] = "devops-team@company.com"
    msg.set_content(f"Disk usage is at {usage:.2f}%. Please clean up space.")
    
    with smtplib.SMTP("smtp.gmail.com", 587) as smtp:
        smtp.starttls()
        smtp.login("admin@server.com", "app-password")
        smtp.send_message(msg)

check_disk_usage()
```

✅ **Use case:** Automatically sends alert when EC2 or VM disk space crosses 80%.

---

## 🔹 Example 4: Automated Log Rotation Script

```python
#!/usr/bin/env python3
import os, shutil
from datetime import datetime

log_dir = "/var/log/myapp"
archive_dir = "/var/log/myapp/archive"

if not os.path.exists(archive_dir):
    os.makedirs(archive_dir)

for file in os.listdir(log_dir):
    if file.endswith(".log"):
        src = os.path.join(log_dir, file)
        dst = os.path.join(archive_dir, f"{file}_{datetime.now().strftime('%Y%m%d')}")
        shutil.move(src, dst)
        open(src, 'w').close()  # create a new empty log file
```

✅ **Use case:** DevOps log maintenance in app servers (non-ELK setups).

---

## 🔹 Example 5: AWS EC2 Instance Start/Stop Script (Boto3)

```python
#!/usr/bin/env python3
import boto3

ec2 = boto3.client('ec2')

def stop_instances():
    response = ec2.describe_instances(Filters=[{'Name': 'instance-state-name', 'Values': ['running']}])
    for reservation in response['Reservations']:
        for instance in reservation['Instances']:
            print(f"Stopping {instance['InstanceId']}")
            ec2.stop_instances(InstanceIds=[instance['InstanceId']])

stop_instances()
```

✅ **Use case:** Save AWS cost by automatically stopping non-prod EC2s after hours.

---

## 🔹 Example 6: Backup & Compress Logs Automatically

```python
#!/usr/bin/env python3
import os, tarfile, datetime

backup_dir = "/backup/logs"
source_dir = "/var/log/myapp"
today = datetime.date.today()

if not os.path.exists(backup_dir):
    os.makedirs(backup_dir)

tar_path = os.path.join(backup_dir, f"logs_{today}.tar.gz")

with tarfile.open(tar_path, "w:gz") as tar:
    tar.add(source_dir, arcname=os.path.basename(source_dir))

print(f"Backup created at {tar_path}")
```

✅ **Use case:** Automate log archiving and rotation before EOD batch execution.

---

## 🔹 Example 7: Jenkins Build Log Parser

```python
#!/usr/bin/env python3
import re

with open("/var/log/jenkins/jenkins.log", "r") as file:
    lines = file.readlines()

error_logs = [line for line in lines if re.search("ERROR|FAILURE", line)]
print(f"Total errors found: {len(error_logs)}")

for line in error_logs[:10]:
    print(line.strip())
```

✅ **Use case:** Quickly detect failed builds or error patterns in Jenkins logs.

---

## 🔹 Example 8: Kubernetes Pod Status Check

```python
#!/usr/bin/env python3
import subprocess
import json

cmd = "kubectl get pods -o json"
result = subprocess.run(cmd, shell=True, capture_output=True, text=True)
pods = json.loads(result.stdout)

for pod in pods["items"]:
    name = pod["metadata"]["name"]
    status = pod["status"]["phase"]
    print(f"{name}: {status}")
```

✅ **Use case:** Simple K8s pod monitor script for non-Dashboard clusters.

---

## 🔹 Example 9: Git Auto Pull and Deploy Script

```python
#!/usr/bin/env python3
import subprocess
import os

repo_path = "/home/devops/myapp"

os.chdir(repo_path)
subprocess.run(["git", "fetch"])
subprocess.run(["git", "pull", "origin", "main"])
subprocess.run(["systemctl", "restart", "myapp"])

print("Deployment successful 🚀")
```

✅ **Use case:** Lightweight auto-deploy pipeline for internal environments.

---

## 🔹 Example 10: Database Backup Script (MySQL/PostgreSQL)

```python
#!/usr/bin/env python3
import subprocess
import datetime

date = datetime.datetime.now().strftime("%Y%m%d_%H%M")
backup_file = f"/backup/db_backup_{date}.sql"

cmd = f"mysqldump -u root -pYourPassword mydb > {backup_file}"
subprocess.run(cmd, shell=True)
print(f"Database backup saved as {backup_file}")
```

✅ **Use case:** Run nightly DB backups via cron or Jenkins job.

---

## 🧩 Key Concepts Learned

| Concept         | Purpose in DevOps                     |
| --------------- | ------------------------------------- |
| `subprocess`    | Execute shell commands safely         |
| `os` / `shutil` | File, directory, and process handling |
| `boto3`         | AWS Automation                        |
| `psutil`        | System monitoring                     |
| `json`, `re`    | Log parsing and JSON processing       |
| `smtplib`       | Automated notifications               |

---
