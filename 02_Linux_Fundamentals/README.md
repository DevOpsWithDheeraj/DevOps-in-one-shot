
# Linux in One Shot

## What is Linux?
- Linux is a free and open-source operating system based on Unix that manages computer hardware and software resources.
- It acts as a bridge between computer hardware and software, managing system resources like CPU, memory, storage, and devices.

---

## History of Linux

### 1. Unix Background (1970s)
- Linux was inspired by Unix, developed in 1969 at AT&T's Bell Labs.
- Unix was powerful but expensive and mostly used in universities and big companies.

### 2. GNU Project (1983)
- In 1983, Richard Stallman started the GNU Project to create a free Unix-like operating system.
- GNU created many tools (compiler, shell, libraries) but no complete kernel.

### 3. Birth of Linux (1991)
- In 1991, Linus Torvalds, a student from Finland, created a free operating system kernel as a hobby.
- He announced it on the Minix newsgroup and released Linux 0.01.
- It was combined with GNU tools, forming GNU/Linux.

### 4. Open Source Growth (1990s)
- Linux became open-source under the GPL License.
- Developers worldwide started contributing.
- Popular distributions like Red Hat, Debian, and Ubuntu were developed.

### 5. Modern Era (2000–Present)
- Linux powers most servers, cloud platforms (AWS, Azure, GCP), Android, and supercomputers.

---

## Why Linux is Important for DevOps?

### 1. Most Servers Run on Linux
- Around 90%+ of cloud servers run on Linux.
- Major cloud providers like AWS, Azure, and GCP heavily use Linux.

### 2. Strong Command Line (CLI)
- DevOps work is automation-heavy.
- Linux provides powerful CLI tools like grep, awk, sed, ssh, etc.

### 3. Automation & Scripting
- DevOps = Automation.
- Shell scripting is mainly done in Linux.
- Tools like Ansible work best with Linux environments.

### 4. Containerization Support
- Containers are built on Linux kernel features.
- Tools like Docker and Kubernetes are Linux-native.

### 5. Networking & Server Management
- DevOps engineers manage servers, users, processes, and logs.
- Linux gives full control over networking and system configurations.

---

## Linux Distributions
Ubuntu, Debian, CentOS, RHEL (Red Hat Enterprise Linux), Fedora, etc.

---

## Linux Architecture Overview

### 1. Hardware Layer
- Physical layer of the system.
- Includes CPU, RAM, Hard Disk/SSD, Network Interface Card, I/O devices.
- Linux interacts with hardware through the Kernel.

### 2. Kernel (Core of Linux)
- The heart of Linux.
- Sits between hardware and user applications.
- Responsibilities:
  - Process Management (CPU scheduling, create/kill processes)
  - Memory Management (RAM allocation, virtual memory)
  - File System Management (ext4, xfs, etc.)

### 3. Shell (Interface Layer)
- Command interpreter.
- Bridge between user → shell → kernel → hardware.
- Popular shells: Bash, sh, zsh, ksh.
- Example: `ls -l` → Shell sends request → Kernel accesses filesystem → Output displayed.

### 4. User Space (Applications)
- System applications, Web Servers (Nginx, Apache), Databases, DevOps tools (Docker, K8s CLI, Git).
- Everything installed runs in user space, not kernel space.

---

## Linux File System Structure

Linux follows a hierarchical directory structure. Everything starts from the root directory `/`.

| Directory | Description |
|------------|------------| `/` | Root Directory |
| `/bin` | Essential user binaries |
| `/sbin` | System binaries |
| `/etc` | Configuration files |
| `/home` | User home directories |
| `/root` | Root user home directory |
| `/var` | Variable files (logs, etc.) |
| `/usr` | Installed applications & libraries |
| `/lib` | Libraries |
| `/tmp` | Temporary files |
| `/dev` | Device files |
| `/proc` | Process information |
| `/boot` | Boot loader files |
| `/opt` | Optional third-party software |

---

## Basic Linux Commands

### File & Directory Management
`pwd`, `ls`, `cd`, `mkdir`, `rmdir`, `touch`, `cp`, `mv`, `rm`, `stat`

### File Viewing Commands
`cat`, `less`, `more`, `head`, `tail`, `tail -f`

---

## Intermediate Linux Commands

### Searching & Filtering
`grep`, `egrep`, `find`, `locate`, `which`

### Text Processing
`cut`, `sort`, `uniq`, `wc`, `tr`, `sed`, `awk`

### Compression & Archiving
`tar`, `gzip`, `gunzip`, `zip`, `unzip`

---

## User & Permission Management

### User Management
`useradd`, `usermod`, `userdel`, `passwd`, `groupadd`, `groupmod`, `groupdel`, `id`

### Permission Management
`chmod`, `chown`, `chgrp`, `umask`

---

## Process Management
`ps`, `top`, `htop`, `kill`, `kill -9`, `nice`, `renice`, `jobs`, `bg`, `fg`, `nohup`, `cron`

---

## Package Management

### Ubuntu/Debian
`apt`, `apt-get`, `dpkg`

### RHEL/CentOS/Amazon Linux
`yum`, `dnf`, `rpm`

---

## Networking Commands
`ping`, `ifconfig`, `ip`, `netstat`, `ss`, `traceroute`, `curl`, `wget`, `scp`, `rsync`, `ssh`, `telnet`

---

## Disk & Storage Management
`df`, `du`, `lsblk`, `mount`, `umount`, `fdisk`, `mkfs`, `blkid`

---

## System Monitoring & Logs
`uptime`, `free`, `vmstat`, `iostat`, `dmesg`, `journalctl`, `watch`
"""
