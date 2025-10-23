# **Problem: Automate Log File Analysis and Alerting**

**Scenario:**
You are a DevOps engineer. Your application generates log files daily in `/var/log/app/`. Each log file contains multiple lines in the following format:

```
[2025-10-23 14:22:11] INFO User login success: user_id=101
[2025-10-23 14:22:15] ERROR Payment failed: order_id=456
[2025-10-23 14:23:10] WARNING Disk space low: /dev/sda1
```

You need a **Python script** that:

1. Scans all `.log` files in `/var/log/app/`.
2. Extracts **ERROR** and **WARNING** messages.
3. Counts the number of occurrences per type.
4. Sends an **email alert** if any ERROR is found.
5. Outputs a **summary report** in JSON format.

---

### **Solution: Python Script**

```python
import os
import re
import json
import smtplib
from email.mime.text import MIMEText
from collections import defaultdict

# Directory containing log files
LOG_DIR = "/var/log/app/"
ALERT_EMAIL = "devops-team@example.com"

# Function to parse logs
def parse_logs(directory):
    pattern = r"\[(.*?)\] (INFO|ERROR|WARNING) (.*)"
    summary = defaultdict(int)
    messages = []

    for file in os.listdir(directory):
        if file.endswith(".log"):
            with open(os.path.join(directory, file), "r") as f:
                for line in f:
                    match = re.match(pattern, line)
                    if match:
                        timestamp, level, message = match.groups()
                        if level in ["ERROR", "WARNING"]:
                            summary[level] += 1
                            messages.append(f"{timestamp} - {level}: {message}")
    return summary, messages

# Function to send email alert
def send_email(subject, body, to_email):
    msg = MIMEText(body)
    msg["Subject"] = subject
    msg["From"] = "alert@company.com"
    msg["To"] = to_email

    try:
        with smtplib.SMTP("localhost") as server:
            server.sendmail("alert@company.com", [to_email], msg.as_string())
        print("Email alert sent successfully.")
    except Exception as e:
        print(f"Failed to send email: {e}")

# Main execution
if __name__ == "__main__":
    summary, messages = parse_logs(LOG_DIR)
    
    # Print JSON summary
    summary_json = json.dumps(summary, indent=4)
    print("Log Summary:")
    print(summary_json)

    # Send email if ERROR exists
    if summary.get("ERROR", 0) > 0:
        send_email(
            subject=f"[ALERT] {summary['ERROR']} Errors Found in Logs",
            body="\n".join(messages),
            to_email=ALERT_EMAIL
        )
```

---

### ✅ **Key Features Covered**

1. **File Handling:** Reads multiple log files dynamically.
2. **Regex Parsing:** Extracts timestamp, log level, and messages.
3. **Data Aggregation:** Counts ERRORs and WARNINGs using `defaultdict`.
4. **JSON Reporting:** Outputs structured summary for monitoring dashboards.
5. **Email Alerts:** Automated alerting for critical errors.

---

## 🧩 **Step-by-Step Explanation**

### 1️⃣ **Imports and Configuration**

```python
import os
import re
import json
import smtplib
from email.mime.text import MIMEText
from collections import defaultdict
```

**What each import does:**

* `os`: for reading files and directories.
* `re`: for **regex pattern matching** (to extract log details).
* `json`: to create structured summaries (JSON format).
* `smtplib` & `MIMEText`: used to send email alerts.
* `defaultdict`: helps count ERROR/WARNING without initializing keys.

```python
LOG_DIR = "/var/log/app/"
ALERT_EMAIL = "devops-team@example.com"
```

These are configuration variables — you can change them as per your environment.

---

### 2️⃣ **Function: parse_logs(directory)**

```python
def parse_logs(directory):
    pattern = r"\[(.*?)\] (INFO|ERROR|WARNING) (.*)"
    summary = defaultdict(int)
    messages = []
```

* **`pattern`**: a regex that captures 3 parts of each log line:

  * Timestamp → `[2025-10-23 14:22:15]`
  * Log Level → `INFO`, `ERROR`, `WARNING`
  * Message → `Payment failed: order_id=456`

  Breakdown of the regex:

  ```
  \[(.*?)\]   → anything inside [ ]
  (INFO|ERROR|WARNING) → log level
  (.*) → message after that
  ```

* **`summary`** → stores count of ERROR and WARNING logs.

* **`messages`** → stores actual log lines for later reporting.

---

### 3️⃣ **Iterate Over All Log Files**

```python
for file in os.listdir(directory):
    if file.endswith(".log"):
        with open(os.path.join(directory, file), "r") as f:
            for line in f:
                match = re.match(pattern, line)
                if match:
                    timestamp, level, message = match.groups()
                    if level in ["ERROR", "WARNING"]:
                        summary[level] += 1
                        messages.append(f"{timestamp} - {level}: {message}")
```

**Explanation:**

* Loops through every `.log` file in `/var/log/app/`.
* Opens each file safely using `with open(...)`.
* Reads each line and matches it with the regex.
* If the log line contains an `ERROR` or `WARNING`:

  * Count it (`summary[level] += 1`)
  * Save it in `messages` for later alerting.

✅ Example:

```
[2025-10-23 14:22:15] ERROR Payment failed: order_id=456
```

Becomes:

```
summary = {'ERROR': 1}
messages = ['2025-10-23 14:22:15 - ERROR: Payment failed: order_id=456']
```

Finally, it returns both `summary` and `messages`.

---

### 4️⃣ **Function: send_email(subject, body, to_email)**

```python
def send_email(subject, body, to_email):
    msg = MIMEText(body)
    msg["Subject"] = subject
    msg["From"] = "alert@company.com"
    msg["To"] = to_email
```

* Creates a simple text-based email using Python’s built-in email library.
* `MIMEText` allows formatting the message body.

```python
    try:
        with smtplib.SMTP("localhost") as server:
            server.sendmail("alert@company.com", [to_email], msg.as_string())
        print("Email alert sent successfully.")
    except Exception as e:
        print(f"Failed to send email: {e}")
```

* Connects to local SMTP server (you can replace `"localhost"` with Gmail or AWS SES).
* Sends the email if there are any `ERROR` logs.
* Catches exceptions to handle email failures gracefully.

---

### 5️⃣ **Main Section**

```python
if __name__ == "__main__":
    summary, messages = parse_logs(LOG_DIR)
```

* Runs `parse_logs()` to get the summary and message list.

```python
    summary_json = json.dumps(summary, indent=4)
    print("Log Summary:")
    print(summary_json)
```

* Converts the summary into JSON (human-readable, also usable by APIs or dashboards like Grafana or ELK).

```python
    if summary.get("ERROR", 0) > 0:
        send_email(
            subject=f"[ALERT] {summary['ERROR']} Errors Found in Logs",
            body="\n".join(messages),
            to_email=ALERT_EMAIL
        )
```

* If any **ERROR** exists, it calls `send_email()` to notify the DevOps team.
* Email body contains the list of all problematic log lines.

---

## 📊 **Sample Output**

If logs contain:

```
[2025-10-23 14:22:11] INFO User login success: user_id=101
[2025-10-23 14:22:15] ERROR Payment failed: order_id=456
[2025-10-23 14:23:10] WARNING Disk space low: /dev/sda1
```

Output:

```
Log Summary:
{
    "ERROR": 1,
    "WARNING": 1
}
Email alert sent successfully.
```

---

## 🚀 **DevOps Concepts Applied**

| DevOps Concept  | Where It’s Used                      |
| --------------- | ------------------------------------ |
| **Monitoring**  | Parsing & analyzing logs             |
| **Alerting**    | Email notifications on ERROR         |
| **Automation**  | Fully automated log scanning         |
| **Reporting**   | JSON summary for dashboards          |
| **Reliability** | Exception handling during email send |

---
