# 🐧 Linux Fundamentals

## 📘 Introduction

Linux is the **foundation of DevOps and Cloud Computing**.  
Most servers, containers, and CI/CD systems run on Linux.  
As a DevOps Engineer, **you must master Linux commands, file systems, permissions, networking, and scripting**.

---

## 🧠 What is Linux?

Linux is an **open-source operating system** based on the **Unix** architecture.  
It manages:
- Hardware resources (CPU, Memory, Storage)
- File systems
- Processes and services
- Users and permissions

### 🔹 Why DevOps Engineers Need Linux
- Almost all cloud servers (AWS EC2, GCP, Azure) run Linux  
- Used in Docker, Kubernetes, Jenkins, and Ansible  
- Enables scripting, automation, and troubleshooting  

---

## ⚙️ 1. Linux Architecture Overview

```

+-------------------------------+
|   Applications (e.g. Jenkins) |
+-------------------------------+
|   Shell / Command Line (Bash) |
+-------------------------------+
|   Kernel (Heart of Linux)     |
+-------------------------------+
|   Hardware (CPU, RAM, Disk)   |
+-------------------------------+

````

- **Kernel:** Controls system resources  
- **Shell:** Interface between user and kernel  
- **File System:** Organizes data into files/directories  

---

## 📂 2. Linux File System Structure

| Directory | Description |
|------------|-------------|
| `/` | Root directory |
| `/home` | User directories |
| `/bin` | Basic commands |
| `/etc` | Configuration files |
| `/var` | Logs, variable data |
| `/tmp` | Temporary files |
| `/usr` | User applications |
| `/opt` | Optional software |

**Command Example:**
```bash
ls /etc
cat /etc/os-release
````

---

## 🧱 3. Basic Linux Commands

| Command | Description            | Example            |
| ------- | ---------------------- | ------------------ |
| `pwd`   | Show current directory | `pwd`              |
| `ls`    | List files             | `ls -l /var/log`   |
| `cd`    | Change directory       | `cd /home`         |
| `cat`   | Display file content   | `cat /etc/passwd`  |
| `cp`    | Copy files             | `cp file1 file2`   |
| `mv`    | Move/Rename files      | `mv old new`       |
| `rm`    | Remove files           | `rm file.txt`      |
| `touch` | Create empty file      | `touch demo.txt`   |
| `mkdir` | Create directory       | `mkdir projects`   |
| `rmdir` | Delete directory       | `rmdir old_folder` |

---

## 🔍 4. File Viewing & Searching Commands

| Command | Description         | Example                   |
| ------- | ------------------- | ------------------------- |
| `head`  | View first lines    | `head -n 10 logfile.txt`  |
| `tail`  | View last lines     | `tail -f /var/log/syslog` |
| `grep`  | Search text         | `grep error logfile.txt`  |
| `find`  | Find files          | `find / -name "*.log"`    |
| `less`  | Scroll through file | `less /etc/passwd`        |
| `wc`    | Count words/lines   | `wc -l file.txt`          |

**Real Example:**

```bash
grep "ERROR" /var/log/nginx/error.log
```

✅ Helps you **troubleshoot production issues**.

---

## 👤 5. User & Permission Management

### Create and Manage Users

```bash
sudo useradd dheeraj
sudo passwd dheeraj
```

### File Permissions

Each file has 3 permissions — **Read (r), Write (w), Execute (x)** for:

* User (owner)
* Group
* Others

**Example:**

```
-rwxr-xr--  dheeraj  devops  deploy.sh
```

| Command  | Description            |
| -------- | ---------------------- |
| `chmod`  | Change permissions     |
| `chown`  | Change ownership       |
| `id`     | View user identity     |
| `groups` | View group memberships |

**Examples:**

```bash
chmod 755 script.sh
chown dheeraj:devops script.sh
```

---

## ⚡ 6. Process Management

| Command     | Description           | Example                  |
| ----------- | --------------------- | ------------------------ |
| `ps`        | View processes        | `ps aux`                 |
| `top`       | Live process view     | `top`                    |
| `htop`      | Enhanced process view | `htop`                   |
| `kill`      | Kill process by PID   | `kill 1234`              |
| `systemctl` | Manage services       | `systemctl status nginx` |

**Example:**

```bash
sudo systemctl restart jenkins
```

✅ Used frequently by DevOps teams to **restart pipelines or web servers**.

---

## 🌐 7. Networking Commands

| Command         | Description                 | Example                         |
| --------------- | --------------------------- | ------------------------------- |
| `ping`          | Test connectivity           | `ping google.com`               |
| `ifconfig/ip a` | Show IP addresses           | `ip a`                          |
| `netstat`       | Display network connections | `netstat -tulnp`                |
| `curl`          | Test URLs / APIs            | `curl https://api.github.com`   |
| `nslookup`      | DNS lookup                  | `nslookup google.com`           |
| `scp`           | Secure file copy            | `scp file.txt user@server:/tmp` |

**Real Example (DevOps):**

```bash
curl -I https://myapp.com
```

✅ Used to verify **application health after deployment**.

---

## 💾 8. Disk & File Management

| Command  | Description         | Example                |
| -------- | ------------------- | ---------------------- |
| `df -h`  | Check disk usage    | `df -h`                |
| `du -sh` | Check folder size   | `du -sh /var/log`      |
| `lsblk`  | List disks          | `lsblk`                |
| `fdisk`  | Disk partition tool | `sudo fdisk -l`        |
| `mount`  | Mount file systems  | `mount /dev/sdb1 /mnt` |

---

## 🧮 9. Package Management

| OS            | Commands            |
| ------------- | ------------------- |
| Ubuntu/Debian | `apt`, `dpkg`       |
| RedHat/CentOS | `yum`, `dnf`, `rpm` |

**Examples:**

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable nginx
```

✅ Useful for setting up Jenkins, Docker, Prometheus, etc.

---

## 📜 10. Shell Scripting (Automation)

Shell scripts automate routine system or DevOps tasks.

**Example Script:** `deploy.sh`

```bash
#!/bin/bash
echo "Starting application deployment..."
cd /var/www/myapp
git pull origin main
docker-compose down
docker-compose up -d
echo "Deployment complete!"
```

**Run Script:**

```bash
chmod +x deploy.sh
./deploy.sh
```

✅ Automates **Git pull + Docker deployment** — a real DevOps use case.

---

## 🧩 11. System Monitoring & Logs

| Command                     | Description       |
| --------------------------- | ----------------- |
| `uptime`                    | Show system load  |
| `free -h`                   | Show memory usage |
| `df -h`                     | Show disk space   |
| `dmesg`                     | Kernel messages   |
| `journalctl -xe`            | View service logs |
| `tail -f /var/log/messages` | Live log tracking |

**Example (Troubleshooting Jenkins):**

```bash
sudo journalctl -u jenkins -f
```

---

## 🔐 12. SSH & Remote Access

**Generate SSH Keys**

```bash
ssh-keygen -t rsa
```

**Copy SSH Key to Remote**

```bash
ssh-copy-id user@remote-server
```

**Access Remote Server**

```bash
ssh user@remote-server
```

✅ Used daily in DevOps to connect to EC2, Kubernetes nodes, or Ansible-managed hosts.

---

## 💡 13. Advanced Linux for DevOps

| Concept                   | Description         | Example                          |
| ------------------------- | ------------------- | -------------------------------- |
| **Crontab**               | Schedule jobs       | `crontab -e`                     |
| **SELinux**               | Security layer      | `getenforce`                     |
| **Systemctl**             | Service control     | `systemctl restart docker`       |
| **Hostnamectl**           | Change hostname     | `hostnamectl set-hostname web01` |
| **Environment Variables** | System configs      | `export PATH=$PATH:/opt/bin`     |
| **I/O Redirection**       | Send output to file | `ls > files.txt`                 |

---

## 🧪 Real DevOps Scenario Example

### 🎯 Problem:

After deployment, your web app isn't responding.
You must troubleshoot and restart the service.

### 🧰 Steps:

```bash
# Step 1: Check if service is active
systemctl status nginx

# Step 2: Check port usage
netstat -tulnp | grep 80

# Step 3: Check logs
tail -n 50 /var/log/nginx/error.log

# Step 4: Restart service
sudo systemctl restart nginx

# Step 5: Verify response
curl -I http://localhost
```

✅ You’ve performed **real-world production troubleshooting**.

---

## 🧾 Summary

| Level               | Key Topics                                  |
| ------------------- | ------------------------------------------- |
| 🟢 **Beginner**     | File system, commands, permissions          |
| 🟡 **Intermediate** | Networking, processes, packages             |
| 🔴 **Advanced**     | Shell scripting, SSH, cron jobs, monitoring |

---

## 🧩 Practice Tasks

### 🟢 Beginner

* List all `.log` files in `/var/log`
* Create, move, and delete files in `/tmp`

### 🟡 Intermediate

* Write a bash script to check disk usage every 10 mins
* Schedule it with `crontab`

### 🔴 Advanced

* Automate web app deployment (Git → Docker → Restart Nginx)
* Secure SSH access to remote Linux EC2 instance

---

## 🏁 Final Takeaway

Linux is the **backbone of DevOps**.
From writing scripts, managing deployments, monitoring logs, or troubleshooting — **every DevOps task starts with Linux**.

> 💬 “If you know Linux well, you control the DevOps world.”

---

