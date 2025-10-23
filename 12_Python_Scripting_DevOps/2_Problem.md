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

Let’s walk through that **Disk Usage & Auto-Cleanup** script step-by-step, line-by-line logic, plus important notes, risks, and improvements you should consider for real-world use.

---

### 1) Imports & constants

```python
import os
import json
import shutil
import subprocess
from datetime import datetime, timedelta

THRESHOLD = 80
LOG_FILE = "/var/log/disk_cleanup.log"
CLEAN_DIRS = ["/tmp", "/var/log/app"]
```

* **Imports**:

  * `os` — filesystem traversal and path ops.
  * `json` — create JSON summary output.
  * `shutil` — (imported but not used in current code; can be used for directory removals).
  * `subprocess` — run shell command `df`.
  * `datetime`, `timedelta` — compute file ages.
* **Constants**:

  * `THRESHOLD` — percent usage above which cleanup triggers (80%).
  * `LOG_FILE` — where script appends its log lines.
  * `CLEAN_DIRS` — directories to scan and delete old files from.

---

### 2) `get_disk_usage()` — read `df` and build usage dictionary

```python
def get_disk_usage():
    result = subprocess.run(["df", "-h", "--output=pcent,target"], capture_output=True, text=True)
    lines = result.stdout.strip().split("\n")[1:]  # skip header
    usage = {}
    for line in lines:
        percent, mount = line.strip().split()
        usage[mount] = int(percent.strip("%"))
    return usage
```

* **What it does**:

  * Runs `df -h --output=pcent,target` to get lines like `45% /` and `85% /home`.
  * `capture_output=True, text=True` returns stdout as string.
  * `split("\n")[1:]` discards header line.
  * Each line is split into `percent` and `mount`.
  * Strips trailing `%` and converts to `int`, storing `usage[mount] = percent_int`.
* **Edge cases / cautions**:

  * If `df` fails (non-zero return), `result.stdout` may be empty — you should check `result.returncode` or `result.stderr`.
  * Some mount points contain spaces (rare but possible). `line.strip().split()` naïvely splits on whitespace; if mount path contains spaces this breaks parsing. Safer parsing would use `--output=pcent,target` and split by fixed width or use `shlex` or `df` without `-h` and parse differently.
  * `-h` produces human-friendly sizes but output here only uses percent and target, so it's OK.

---

### 3) `clean_old_files(directory, days=7)` — delete files older than `days`

```python
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
```

* **What it does**:

  * Walks the directory tree with `os.walk`.
  * For each file, checks modification time (`getmtime`).
  * If file is older than `days` (default 7), deletes it with `os.remove()` and adds path to `deleted_files`.
* **Details & caveats**:

  * Only deletes files, not empty directories. If you want to remove directories older than `days`, you'd need to check `dirs` or use `shutil.rmtree` with care.
  * The `except Exception: continue` silently ignores errors (permission issues, broken symlinks). In production you might want to log these exceptions instead of ignoring.
  * Deleting files requires appropriate permissions; script may need to run as root for some paths.
  * No dry-run option is provided; consider adding `--dry-run` to see what would be deleted without removing.

---

### 4) `log_message(message)` — append messages to a log file

```python
def log_message(message):
    with open(LOG_FILE, "a") as f:
        f.write(f"{datetime.now()} - {message}\n")
```

* **What it does**:

  * Opens `LOG_FILE` in append mode and writes a timestamped message.
* **Notes**:

  * Using Python’s `logging` module is better for rotation, levels, and thread-safety.
  * Ensure the script has write permission for `LOG_FILE`.

---

### 5) `if __name__ == "__main__":` — main control flow

```python
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

print(json.dumps(cleanup_summary, indent=4))
log_message("Disk cleanup completed successfully.")
```

* **Step-by-step**:

  1. Get current disk usage mapping (`mount -> percent`).
  2. For each mount point:

     * If `percent > THRESHOLD`:

       * Log an alert message to `LOG_FILE`.
       * Iterate the configured `CLEAN_DIRS`. If directory exists, call `clean_old_files` and collect deleted files.
       * Add an entry to `cleanup_summary` with `usage_before` and how many files were deleted.
     * Else, mark mount as OK in `cleanup_summary`.
  3. Print `cleanup_summary` as pretty JSON to STDOUT (useful for capture by cron or CI).
  4. Append a completion log message.
* **Behavioral notes**:

  * The script cleans the same set of directories regardless of which mount is above threshold — it does not check whether the directory sits on the affected mount. That means it might run unnecessary deletions on unrelated file systems.
  * The code uses `files_deleted` count but does not measure freed bytes; after deleting many small files usage might still be high. For better metric, compute used space before/after.

---

### 6) Risks, limitations, and safety

* **Accidental data loss** — deleting from `/var/log/app` can remove needed logs for audits. Always implement a dry-run and backups.
* **Permissions** — deletion requires appropriate user privileges.
* **Race conditions** — files may be modified while being checked/deleted.
* **Parsing fragility** — `df` parsing can break for unusual mount names; handle `subprocess.returncode` and `stderr`.
* **No notifications** — script logs to a file only. In production send Slack/email/CloudWatch alerts when thresholds hit.
* **No locking** — if the script runs concurrently (e.g., scheduled overlapping cron), it may double-delete or conflict. Use a pidfile or lock.

---

### 7) Recommended improvements (practical, production-ready)

1. **Use `logging` module** (with rotation) instead of `open()` to write log entries.
2. **Add dry-run flag** to preview deletions: `--dry-run` prints what would be deleted.
3. **Check `df` return code** and parse robustly (handle spaces in mount names). Example: use `df --output=pcent,target -P` for POSIX-compliant output, split with maxsplit=1.
4. **Only clean directories that reside on the affected mount** — determine mount of a directory with `os.statvfs()` or parse `/proc/mounts`.
5. **Log exceptions** in `clean_old_files` so you know which files failed to delete.
6. **Measure freed bytes** by summing `os.path.getsize()` of deleted files, and report `usage_after` (call `get_disk_usage()` again after cleanup).
7. **Add concurrency protection** — create a lock file or use file locking to prevent overlapping runs.
8. **Test mode & safety** — require a `--confirm` flag to run destructive actions if running interactively.
9. **Use retention policy & rotate logs** instead of deleting randomly — integrate with `logrotate`.
10. **Container / Kubernetes**: If running in containers, consider ephemeral storage differences and node-level cleanup mechanisms.

---

### 8) Quick example: robust `df` parsing (small snippet)

```python
result = subprocess.run(["df", "--output=pcent,target", "-P"], capture_output=True, text=True, check=True)
lines = result.stdout.strip().splitlines()[1:]
for line in lines:
    percent, mount = line.strip().split(None, 1)  # split at first whitespace only
    usage[mount] = int(percent.rstrip('%'))
```

* `split(None, 1)` protects against mounts containing spaces.

---

### 9) Testing & deployment tips

* **Run locally with `--dry-run`** on a test directory to validate logic.
* **Use unit tests** for `clean_old_files` using temporary directories (`tempfile.TemporaryDirectory`) and controlled file mtimes.
* **Schedule** as `cron` or systemd timer; ensure log rotation for `LOG_FILE`.
* **Alerting**: integrate with Slack/Teams/email when mount > threshold and after cleanup.

---
