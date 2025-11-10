
# 🌐 **Networking Commands**

> “Behind every website you open or deployment you push,
> there’s an invisible world of packets, ports, and protocols.
> Networking commands are your window into that world.”

---

## 🧭 **1️⃣ `ping` — Check Network Connectivity**

**Purpose:**
Tests if your system can reach another host (by sending **ICMP Echo Request** packets).

**Syntax:**

```bash
ping <hostname or IP>
```

**Example:**

```bash
ping google.com
```

**Output:**

```
Pinging google.com [142.250.183.110] with 32 bytes of data:
Reply from 142.250.183.110: bytes=32 time=20ms TTL=115
```

**Use Case in DevOps:**
✅ Check if servers or services are reachable.
✅ Test latency between nodes.

**Analogy:**
Like shouting “Hey!” across a hallway and waiting for “Yes!” back.

---

## 🌍 **2️⃣ `tracert` / `traceroute` — Trace Packet Route**

**Purpose:**
Shows the **path (hops)** packets take to reach a destination.

**Syntax:**

* Windows:

  ```bash
  tracert google.com
  ```
* Linux/macOS:

  ```bash
  traceroute google.com
  ```

**Example Output:**

```
1  192.168.0.1
2  10.0.0.1
3  72.14.219.1
4  142.250.183.110
```

**Use Case:**
✅ Identify slow or broken network hops.
✅ Troubleshoot routing issues between servers.

**Analogy:**
Like tracking the checkpoints your courier passes before reaching the destination.

---

## 🔍 **3️⃣ `ipconfig` / `ifconfig` — View Network Configuration**

**Purpose:**
Displays **IP address**, **subnet mask**, **gateway**, and **DNS** information.

**Syntax:**

* Windows:

  ```bash
  ipconfig /all
  ```
* Linux/macOS:

  ```bash
  ifconfig
  ```

**Example Output:**

```
Ethernet adapter Local Area Connection:
   IPv4 Address. . . . . . . . . . : 192.168.0.10
   Subnet Mask . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . : 192.168.0.1
```

**Use Case:**
✅ Verify assigned IPs.
✅ Debug DHCP or DNS issues.

---

## 🔄 **4️⃣ `netstat` — Show Network Connections**

**Purpose:**
Displays **active connections**, **ports**, and **network statistics**.

**Syntax:**

```bash
netstat -an
```

**Example Output:**

```
Proto  Local Address          Foreign Address        State
TCP    0.0.0.0:80             0.0.0.0:0              LISTENING
TCP    192.168.0.10:5678      172.217.167.78:443     ESTABLISHED
```

**Use Case:**
✅ Check which ports are open.
✅ Detect suspicious connections.
✅ Find which app is using a port.

**Advanced Example:**

```bash
netstat -tulnp     # Linux: show TCP/UDP ports with process IDs
```

---

## 🌐 **5️⃣ `nslookup` / `dig` — DNS Lookup**

**Purpose:**
Resolves **domain names** to **IP addresses**.

**Syntax:**

```bash
nslookup google.com
```

or

```bash
dig google.com
```

**Example Output:**

```
Non-authoritative answer:
Name: google.com
Address: 142.250.183.110
```

**Use Case:**
✅ Verify DNS resolution.
✅ Debug domain or DNS issues.

**Analogy:**
Like checking a phonebook for someone’s number.

---

## 🚪 **6️⃣ `telnet` — Test Port Connectivity**

**Purpose:**
Tests if a specific **port** on a host is reachable.

**Syntax:**

```bash
telnet <hostname> <port>
```

**Example:**

```bash
telnet google.com 443
```

**Output:**

```
Connected to google.com
```

**Use Case:**
✅ Check if ports (like 22, 80, 443) are open.
✅ Troubleshoot firewall or security group rules.

---

## 🔧 **7️⃣ `curl` — Transfer Data from or to a Server**

**Purpose:**
Tests **HTTP/HTTPS APIs**, file transfers, or endpoints.

**Syntax:**

```bash
curl <url>
```

**Examples:**

```bash
curl https://api.github.com
curl -I https://www.google.com      # Fetch headers only
curl -X POST -d "user=dheeraj" https://example.com/login
```

**Use Case:**
✅ Test REST APIs.
✅ Debug backend service connectivity.
✅ Check response time, headers, and codes.

---

## 🧰 **8️⃣ `wget` — Download Files from the Internet**

**Purpose:**
Downloads files or webpages from a given URL.

**Syntax:**

```bash
wget <url>
```

**Example:**

```bash
wget https://example.com/file.zip
```

**Use Case:**
✅ Fetch configuration files, packages, or scripts on remote servers.
✅ Automate download tasks.

---

## 🧩 **9️⃣ `arp` — View MAC to IP Mapping**

**Purpose:**
Shows or modifies the **ARP table**, which maps **IP addresses to MAC addresses**.

**Syntax:**

```bash
arp -a
```

**Example Output:**

```
Interface: 192.168.0.10 --- 0x2
  Internet Address    Physical Address    Type
  192.168.0.1         00-14-22-01-23-45   dynamic
```

**Use Case:**
✅ Troubleshoot LAN communication.
✅ Detect duplicate or spoofed IPs.

---

## ⚙️ **🔟 `route` — Display or Modify Routing Table**

**Purpose:**
Shows or sets **network routes** used by the system.

**Syntax:**

```bash
route print        # Windows
route -n           # Linux
```

**Example Output:**

```
Destination     Gateway         Genmask         Flags Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG    eth0
```

**Use Case:**
✅ Debug routing problems.
✅ Add or remove custom routes.

---

## 🧱 **11️⃣ `ssh` — Secure Remote Login**

**Purpose:**
Securely connects to a **remote server**.

**Syntax:**

```bash
ssh user@hostname
```

**Example:**

```bash
ssh ec2-user@54.210.123.45
```

**Use Case:**
✅ Manage Linux servers.
✅ Deploy code remotely.
✅ Run commands securely.

---

## 🧮 **12️⃣ `hostname` — Display or Set Host Name**

**Purpose:**
Shows or sets the system’s hostname.

**Syntax:**

```bash
hostname
```

**Example Output:**

```
dheeraj-devops-server
```

**Use Case:**
✅ Identify which server you’re on.
✅ Rename hosts for better organization.

---

## 🔐 **13️⃣ `netcat` (`nc`) — Network Swiss Army Knife**

**Purpose:**
Used for **port scanning**, **listening**, or **sending data** over the network.

**Syntax:**

```bash
nc -zv <host> <port-range>
```

**Example:**

```bash
nc -zv google.com 80-443
```

**Output:**

```
Connection to google.com 80 port [tcp/http] succeeded!
Connection to google.com 443 port [tcp/https] succeeded!
```

**Use Case:**
✅ Check open ports.
✅ Create quick TCP servers or clients.
✅ Test network connectivity.

---

## 🧠 **14️⃣ `scp` — Secure File Copy**

**Purpose:**
Copies files securely between local and remote machines over SSH.

**Syntax:**

```bash
scp <source> <destination>
```

**Example:**

```bash
scp myfile.txt ubuntu@192.168.1.5:/home/ubuntu/
```

**Use Case:**
✅ Transfer files securely between servers.
✅ Automate deployment scripts.

---

## 🌈 **15️⃣ `iftop` — Real-Time Network Bandwidth Monitor**

**Purpose:**
Displays **real-time network usage** (like `top`, but for network).

**Syntax:**

```bash
sudo iftop
```

**Output:**
Shows live incoming/outgoing data rates per connection.

**Use Case:**
✅ Monitor network traffic.
✅ Identify bandwidth-heavy processes.

---

## 🧰 **16️⃣ `tcpdump` — Packet Capture Tool**

**Purpose:**
Captures and analyzes network packets in real time.

**Syntax:**

```bash
sudo tcpdump -i eth0
```

**Example:**

```bash
sudo tcpdump host 8.8.8.8
```

**Use Case:**
✅ Deep network debugging.
✅ Inspect API or security traffic.
✅ Analyze packets in DevOps networking tests.

---

## ⚡ **17️⃣ `nmap` — Network Scanner**

**Purpose:**
Scans networks and identifies **hosts, open ports, and services**.

**Syntax:**

```bash
nmap <IP or hostname>
```

**Example:**

```bash
nmap 192.168.0.0/24
```

**Use Case:**
✅ Audit security.
✅ Discover live hosts in a subnet.
✅ Check firewall and service status.

---

## 🧭 **Summary Table**

| Command                 | Function                      | Example                        | Layer       |
| ----------------------- | ----------------------------- | ------------------------------ | ----------- |
| `ping`                  | Test reachability             | `ping google.com`              | Network     |
| `traceroute`            | Trace route to destination    | `traceroute google.com`        | Network     |
| `ipconfig` / `ifconfig` | Show IP config                | `ifconfig eth0`                | Data Link   |
| `netstat`               | Show active ports/connections | `netstat -an`                  | Transport   |
| `nslookup`              | Resolve domain names          | `nslookup github.com`          | Application |
| `telnet`                | Test port connectivity        | `telnet google.com 80`         | Transport   |
| `curl`                  | Test web/API endpoints        | `curl -I example.com`          | Application |
| `arp`                   | Show MAC-to-IP mapping        | `arp -a`                       | Data Link   |
| `route`                 | Show routing table            | `route -n`                     | Network     |
| `ssh`                   | Remote login                  | `ssh user@host`                | Application |
| `scp`                   | Secure copy                   | `scp file.txt user@host:/path` | Application |
| `netcat`                | Check open ports              | `nc -zv host 22`               | Transport   |
| `iftop`                 | Monitor bandwidth             | `iftop`                        | Data Link   |
| `tcpdump`               | Capture packets               | `tcpdump -i eth0`              | Data Link   |
| `nmap`                  | Scan network                  | `nmap 192.168.1.0/24`          | Network     |

---

## 🎯 **In DevOps Context**

These commands are your **first line of defense** for diagnosing:

* 🧩 **CI/CD connectivity** issues
* 🧱 **Firewall or Security Group** misconfigurations
* ⚙️ **Service-to-service communication**
* ☁️ **Cloud instance troubleshooting**

> “In DevOps, mastering these commands means you don’t just deploy code —
> you deploy confidence.”

---
