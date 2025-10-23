# 🌐 Networking in DevOps 

## 📘 Introduction

Networking is **critical in DevOps** for communication between servers, deployment pipelines, containers, cloud services, and monitoring tools.  
Understanding **network protocols, models, ports, and troubleshooting commands** is essential for DevOps engineers.

---

## 🧩 1. OSI & TCP/IP Model

### 🔹 OSI Model (7 Layers)
| Layer | Function | Example |
|-------|---------|---------|
| 7. Application | Provides services to apps | HTTP, FTP, DNS |
| 6. Presentation | Data translation & encryption | SSL/TLS, JPEG, ASCII |
| 5. Session | Session management | SSH sessions, API sessions |
| 4. Transport | Data delivery & reliability | TCP, UDP |
| 3. Network | Routing & IP addressing | IP, ICMP, routers |
| 2. Data Link | Frames & MAC addressing | Ethernet, Switches |
| 1. Physical | Physical transmission | Cables, NICs, Fiber |

### 🔹 TCP/IP Model (4 Layers)
| Layer | OSI Mapping | Example |
|-------|------------|---------|
| Application | OSI 5-7 | HTTP, FTP, DNS |
| Transport | OSI 4 | TCP, UDP |
| Internet | OSI 3 | IP, ICMP |
| Network Access | OSI 1-2 | Ethernet, Wi-Fi |

---

## 🧰 2. Key DevOps Ports & Protocols

| Port | Protocol | Usage in DevOps |
|------|---------|----------------|
| 22 | TCP | SSH access to servers, Git operations |
| 80 | TCP | HTTP web traffic, web app testing |
| 443 | TCP | HTTPS web traffic, secure API calls |
| 8080 | TCP | Jenkins default port, Tomcat, web apps |
| 3000-5000 | TCP | Node.js apps, development servers |
| 3306 | TCP | MySQL database |
| 5432 | TCP | PostgreSQL database |
| 2375 | TCP | Docker daemon (insecure) |
| 2376 | TCP | Docker daemon (secure TLS) |
| 6443 | TCP | Kubernetes API server |
| 5000 | TCP | Docker registry local |

> 💡 Knowing these ports is critical for **firewall rules, security groups, and CI/CD pipeline configuration**.

---

## 🔍 3. Networking Commands (Basic → Advanced)

### 🔹 Basic Commands
| Command | Purpose | Example |
|---------|--------|---------|
| `ping` | Test connectivity | `ping google.com` |
| `curl` | Test HTTP endpoints | `curl -I https://myapp.com` |
| `nslookup` | DNS resolution | `nslookup github.com` |
| `dig` | Advanced DNS lookup | `dig example.com` |
| `traceroute` | Route packets to destination | `traceroute google.com` |
| `ifconfig / ip a` | Check local IPs | `ip a` |
| `netstat / ss` | Check active connections | `netstat -tulnp` |

### 🔹 Intermediate Commands
| Command | Purpose | Example |
|---------|--------|---------|
| `telnet` | Check connectivity on port | `telnet myserver 22` |
| `nmap` | Port scanning | `nmap 192.168.1.10` |
| `iptables` | Firewall rules | `sudo iptables -L` |
| `curl -X POST` | Test APIs | `curl -X POST http://myapp.com/api` |

### 🔹 Advanced Commands
- `tcpdump` – Capture and analyze network packets:
```bash
sudo tcpdump -i eth0 port 80
````

* `wireshark` – GUI-based packet analysis
* `netcat (nc)` – Test TCP/UDP connections:

```bash
nc -zv myserver 8080
```

---

## ⚡ 4. Real DevOps Networking Examples

### 🔹 Example 1: Troubleshoot Jenkins Connectivity

```bash
# Check if Jenkins port 8080 is open
netstat -tulnp | grep 8080

# Test access from remote machine
curl -I http://jenkins-server:8080
```

### 🔹 Example 2: Kubernetes Node Communication

```bash
# Check if API server port 6443 is reachable
nc -zv <k8s-master-ip> 6443

# Check cluster DNS resolution
kubectl exec -it <pod-name> -- nslookup google.com
```

### 🔹 Example 3: Docker Container Networking

```bash
# List Docker networks
docker network ls

# Inspect bridge network
docker network inspect bridge

# Ping between containers
docker exec container1 ping container2
```

---

## 🧠 5. Networking Best Practices for DevOps

1. Always **open only required ports** in firewall/security groups
2. Use **SSH key-based authentication** instead of passwords
3. Prefer **HTTPS/SSL** for all CI/CD, API, and container communication
4. Implement **network segmentation** for production/staging/dev
5. Use **load balancers** to distribute traffic across nodes
6. Monitor **latency, packet loss, and throughput** continuously
7. Secure sensitive ports (2376, 22, 6443) with VPN or private networks

---

## 🧩 6. Hands-on Networking Lab for DevOps

**Scenario:** Deploy a multi-container application and verify networking.

1. Launch 2 Docker containers:

```bash
docker run -d --name web nginx
docker run -d --name app myapp:latest
```

2. Create a custom network:

```bash
docker network create devops-net
docker network connect devops-net web
docker network connect devops-net app
```

3. Verify connectivity:

```bash
docker exec web ping app
curl http://app:5000
```

4. Expose ports to host and test access:

```bash
docker run -d -p 8080:80 nginx
curl http://localhost:8080
```

✅ You now understand **Docker container networking, ports, and connectivity verification**, essential for CI/CD pipelines and Kubernetes clusters.

---

## 🏁 Summary

| Level           | Topics Covered                                                                                 |
| --------------- | ---------------------------------------------------------------------------------------------- |
| 🟢 Beginner     | OSI & TCP/IP, IP, DNS, basic commands                                                          |
| 🟡 Intermediate | Ports, telnet, curl, nmap, iptables                                                            |
| 🔴 Advanced     | Packet capture (tcpdump, wireshark), container/K8s networking, troubleshooting CI/CD pipelines |

> 💬 “Networking is the invisible backbone of DevOps — if it fails, everything else fails.”

---
