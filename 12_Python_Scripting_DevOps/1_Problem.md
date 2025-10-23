## **Problem: Automate Log File Analysis and Alerting**

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

This is a **real-world DevOps Python script** for log monitoring and alerting. It’s extendable for cloud environments (e.g., AWS SES for email, S3 for logs, Slack notifications).

---
