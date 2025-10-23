# 🧩 **Problem: Monitor Disk Usage and Trigger Auto-Cleanup**

## **Scenario**

As a **DevOps engineer**, you’re responsible for ensuring that Linux servers don’t run out of disk space.
You need to write a **Python script** that:

1. Checks disk usage of all file systems using the `df -h` command.
2. If any partition usage exceeds **80%**,

   * Logs the alert in a file
   * Deletes files older than **7 days** from `/tmp` and `/var/log/app/`
3. Sends a summary report in **JSON** format.

This is a **real-world DevOps task** that automates housekeeping on servers.

---

## 💻 **Solution: Python Script**

```python
import os
import json
import shutil
import subprocess
from datetime import datetime, timedelta

# Threshold for disk usage alert
THRESHOLD = 80
LOG_FILE = "/var/log/disk_cleanup.log"

# Directories to clean
CLEAN_DIRS = ["/tmp", "/var/log/app"]

# Function to get disk usage
def get_disk_usage():
    result = subprocess.run(["df", "-h", "--output=pcent,target"], capture_output=True, text=True)
    lines = result.stdout.strip().split("\n")[1:]  # skip header
    usage = {}
    for line in lines:
        percent, mount = line.strip().split()
        usage[mount] = int(percent.strip("%"))
    return usage

# Function to clean old files
def clean_old_files(directory, days=7):
    now = datetime.now()
    deleted_files = []
    for root, dirs, files in os.walk(directory):
        for file in files:
            file_path = os.path.join(root, file)
            try:
                file_time = datetime.fromtimestamp(os.path.getmtime(file_path))
                if (now - file_time) > timedelta(days=days):
                    os.remove(file_path)
                    deleted_files.append(file_path)
            except Exception:
                continue
    return deleted_files

# Function to log messages
def log_message(message):
    with open(LOG_FILE, "a") as f:
        f.write(f"{datetime.now()} - {message}\n")

# Main logic
if __name__ == "__main__":
    usage_report = get_disk_usage()
    cleanup_summary = {}

    for mount, percent in usage_report.items():
        if percent > THRESHOLD:
            log_message(f"ALERT: Disk usage at {percent}% on {mount}. Starting cleanup...")
            total_deleted = []
            for dir_path in CLEAN_DIRS:
                if os.path.exists(dir_path):
                    deleted_files = clean_old_files(dir_path)
                    total_deleted.extend(deleted_files)
            cleanup_summary[mount] = {
                "usage_before": f"{percent}%",
                "files_deleted": len(total_deleted)
            }
        else:
            cleanup_summary[mount] = {"usage": f"{percent}%", "status": "OK"}

    # Print summary in JSON
    print(json.dumps(cleanup_summary, indent=4))
    log_message("Disk cleanup completed successfully.")
```

---

## 🧠 **Step-by-Step Explanation**

### **1️⃣ Get Disk Usage**

```python
subprocess.run(["df", "-h", "--output=pcent,target"], capture_output=True, text=True)
```

* Runs Linux `df -h` command to get disk usage.
* Example output:

  ```
  Use% Mounted on
  45% /
  85% /home
  ```
* The script reads this output and creates a dictionary like:

  ```python
  {'/': 45, '/home': 85}
  ```

---

### **2️⃣ Clean Old Files**

```python
os.walk(directory)
```

* Walks through all files inside `/tmp` and `/var/log/app/`.
* Deletes files **older than 7 days**:

  ```python
  (now - file_time) > timedelta(days=7)
  ```
* Collects names of deleted files for reporting.

---

### **3️⃣ Logging**

```python
log_message(f"ALERT: Disk usage at {percent}% on {mount}. Starting cleanup...")
```

* Writes log entries in `/var/log/disk_cleanup.log` for auditing.
* Helps trace cleanup activity.

---

### **4️⃣ JSON Summary**

```python
print(json.dumps(cleanup_summary, indent=4))
```

* At the end, you get a neat JSON report like:

```json
{
    "/": {
        "usage": "45%",
        "status": "OK"
    },
    "/home": {
        "usage_before": "85%",
        "files_deleted": 22
    }
}
```

---

## ⚙️ **5️⃣ DevOps Use Case**

| DevOps Aspect   | Purpose                                     |
| --------------- | ------------------------------------------- |
| **Automation**  | Avoid manual disk cleanup                   |
| **Monitoring**  | Detects partitions crossing usage threshold |
| **Logging**     | Tracks cleanup activity                     |
| **Reporting**   | JSON summary for dashboards                 |
| **Maintenance** | Prevents service downtime due to full disks |

---
