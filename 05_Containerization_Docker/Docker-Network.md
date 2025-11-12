
## 🐳 What is Docker Networking?

Docker Networking allows containers to **communicate** with each other, the **host machine**, and the **outside world (Internet)**.
It provides **isolated virtual networks** where containers can connect securely.

Think of Docker networks like **virtual LANs** for your containers.

---

## 💡 Why We Need Docker Network

1. **Container Communication:**
   Containers can talk to each other without exposing ports to the host.

2. **Isolation:**
   Containers in different networks cannot access each other unless configured.

3. **Flexibility:**
   You can create custom networks for different environments (dev, test, prod).

4. **Portability:**
   The same network configuration works across machines.

---

## ⚙️ Types of Docker Networks

| Network Type | Description                                                                          | Example Use                            |
| ------------ | ------------------------------------------------------------------------------------ | -------------------------------------- |
| **bridge**   | Default network for containers. Isolated from the host, but can access the Internet. | Local container communication          |
| **host**     | Shares host’s network stack. No isolation between container and host.                | Performance-critical apps              |
| **none**     | Completely isolated. No network access.                                              | Security testing or isolated workloads |
| **overlay**  | Connects containers across multiple Docker hosts (Swarm/Kubernetes).                 | Multi-host communication               |
| **macvlan**  | Assigns a MAC address to container (appears as a physical device).                   | When container needs its own IP in LAN |

---

## 🧩 Default Networks

Run:

```bash
docker network ls
```

Output:

```
NETWORK ID     NAME      DRIVER    SCOPE
f9b3b9f12abc   bridge    bridge    local
c7b4b8e24e3d   host      host      local
a4b2c3d4e5f6   none      null      local
```

These are automatically created when Docker is installed.

---

## 🔹 1. Bridge Network (Default)

When you run a container without specifying a network, it connects to the **bridge** network.

Example:

```bash
docker run -d --name web1 nginx
docker run -d --name web2 nginx
```

Both `web1` and `web2` are in the default `bridge` network.

Check network:

```bash
docker network inspect bridge
```

You’ll see container IPs like:

```
"Containers": {
    "a1b2c3d4e5": {
        "Name": "web1",
        "IPv4Address": "172.17.0.2/16"
    },
    "b2c3d4e5f6": {
        "Name": "web2",
        "IPv4Address": "172.17.0.3/16"
    }
}
```

They can communicate using IP addresses, but **not by names** unless they share a custom bridge network.

---

## 🔹 2. Custom Bridge Network (Best Practice)

Create your own bridge network:

```bash
docker network create mynetwork
```

Now run containers in this network:

```bash
docker run -d --name db --network mynetwork mysql
docker run -d --name web --network mynetwork nginx
```

➡️ Now `web` can reach `db` by name (`db:3306`), thanks to Docker’s internal DNS.

Example ping:

```bash
docker exec web ping db
```

---

## 🔹 3. Host Network

Container shares host network directly:

```bash
docker run -d --network host nginx
```

➡️ Container uses host’s IP and ports directly (no isolation).
Use when performance is critical, e.g., monitoring tools like Prometheus or Grafana.

---

## 🔹 4. None Network

Run isolated container:

```bash
docker run -d --network none nginx
```

➡️ No internet, no communication — total isolation.

---

## 🔹 5. Overlay Network (Multi-Host)

Used in Docker Swarm for cross-node communication:

```bash
docker network create -d overlay my-overlay
```

This lets containers on **different Docker hosts** talk to each other securely.

---

## 🔹 6. Macvlan Network

Give container its **own IP** in your LAN:

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 my-macvlan
```

Run container:

```bash
docker run -d --name myapp --network my-macvlan nginx
```

➡️ Now `myapp` appears as a real device in your LAN (like 192.168.1.50).

---

## 🔧 Useful Docker Network Commands

| Command                                           | Description                         |
| ------------------------------------------------- | ----------------------------------- |
| `docker network ls`                               | List all networks                   |
| `docker network inspect <network>`                | View details of a network           |
| `docker network create <name>`                    | Create new network                  |
| `docker network rm <name>`                        | Remove a network                    |
| `docker network connect <network> <container>`    | Connect a container to a network    |
| `docker network disconnect <network> <container>` | Disconnect container from a network |

---

## 🧠 Example Scenario

Let’s say you have:

* **nginx (frontend)**
* **mysql (database)**

Create a network:

```bash
docker network create app-net
```

Run both containers:

```bash
docker run -d --name db --network app-net -e MYSQL_ROOT_PASSWORD=root mysql:latest
docker run -d --name web --network app-net -p 8080:80 nginx
```

✅ `web` can access `db` via the name `db`, without exposing MySQL to the host.

---

## 🧩 In Docker Compose (YAML Example)

`docker-compose.yml`:

```yaml
version: "3.8"
services:
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: root
    networks:
      - app-net

  web:
    image: nginx
    ports:
      - "8080:80"
    networks:
      - app-net

networks:
  app-net:
    driver: bridge
```

Run:

```bash
docker-compose up -d
```

➡️ Both services share the same **`app-net`** network.

---

## 🔚 Summary

| Concept            | Description                             |
| ------------------ | --------------------------------------- |
| **Bridge Network** | Default isolated network for containers |
| **Custom Bridge**  | Enables name-based communication        |
| **Host Network**   | Shares host’s network stack             |
| **None Network**   | Fully isolated                          |
| **Overlay**        | Multi-host communication                |
| **Macvlan**        | Container gets its own LAN IP           |

---
