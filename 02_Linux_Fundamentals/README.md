

# 1️⃣ What is Linux?

Linux is an **open-source, Unix-like operating system kernel** created by Linus Torvalds in 1991.  
An Operating System (OS) acts as an interface between user and hardware.

Linux = Kernel + GNU Tools + Utilities

---

# 2️⃣ History of Linux

- 1969 – UNIX developed at Bell Labs  
- 1983 – GNU Project started by Richard Stallman  
- 1991 – Linus Torvalds released Linux Kernel  
- 1992 – Linux became open source under GPL  

---

# 3️⃣ Why Linux is Important for DevOps?

✅ Most servers run Linux  
✅ Cloud platforms (AWS, Azure, GCP) use Linux  
✅ Automation & scripting friendly  
✅ Secure & stable  
✅ Container ecosystem (Docker, Kubernetes) built around Linux  

---

# 4️⃣ Linux Distributions

- Ubuntu
- Debian
- RHEL
- CentOS
- Fedora
- Amazon Linux
- Kali Linux

---

# 5️⃣ Applications of Linux

- Servers
- Cloud Computing
- DevOps Automation
- Cybersecurity
- Embedded Systems
- Supercomputers

---

# 6️⃣ Linux Architecture Overview

User Space → Shell → Kernel → Hardware

Kernel Responsibilities:
- Process Management
- Memory Management
- Device Drivers
- File System

---

# 7️⃣ Linux File System Structure

/ – Root  
/home – User files  
/etc – Configuration files  
/var – Logs  
/bin – Basic commands  
/usr – User programs  
/tmp – Temporary files  
/boot – Boot loader files  

---

# 8️⃣ Basic Linux Commands

## File & Directory Management

pwd → Print working directory  
ls → List files  
cd dir → Change directory  
mkdir dir → Create directory  
rmdir dir → Remove empty directory  
touch file → Create file  
cp a b → Copy file  
mv a b → Move/rename  
rm file → Delete file  
stat file → File details  

## File Viewing

cat file  
less file  
more file  
head file  
tail file  
tail -f file (Live log monitoring)

---

# 9️⃣ Intermediate Commands

## Searching & Filtering

grep "word" file  
find / -name file.txt  
locate file  
which ls  
whereis python  

## Text Processing

cut -d: -f1 /etc/passwd  
sort file  
uniq file  
wc -l file  
tr a-z A-Z  
sed 's/old/new/' file  
awk '{print $1}' file  

## Compression

tar -cvf file.tar dir  
tar -xvf file.tar  
gzip file  
gunzip file.gz  
zip file.zip file  
unzip file.zip  

---

# 🔟 User & Permission Management

## User Management

useradd user  
passwd user  
usermod -aG group user  
userdel user  
groupadd group  
groupdel group  
id user  

## Permission Management

chmod 755 file  
chown user file  
chgrp group file  
umask  

Permission Format:
rwx rwx rwx  
Owner Group Others  

---

# 1️⃣1️⃣ Process Management

ps  
top  
htop  
kill PID  
kill -9 PID  
nice -n 10 command  
renice 5 PID  
jobs  
bg  
fg  
nohup command &  

---

# 1️⃣2️⃣ Package Management

## Ubuntu/Debian

apt update  
apt install nginx  
apt remove nginx  
dpkg -i package.deb  

## RHEL/CentOS

yum install httpd  
dnf install httpd  
rpm -ivh package.rpm  

---

# 1️⃣3️⃣ Networking Commands

ping google.com  
ifconfig  
ip a  
netstat -tulnp  
ss -tulnp  
traceroute google.com  
curl example.com  
wget file_url  
scp file user@host:/path  
rsync -av source dest  
ssh user@host  
telnet host port  
nmap host  

---

# 1️⃣4️⃣ Disk & Storage

df -h  
du -sh dir  
lsblk  
mount /dev/sdb1 /mnt  
umount /mnt  
fdisk -l  
mkfs.ext4 /dev/sdb1  
blkid  

---

# 1️⃣5️⃣ System Monitoring & Logs

uptime  
free -m  
vmstat  
iostat  
dmesg  
journalctl  
watch df -h  

---

Linux is the backbone of DevOps 🚀
