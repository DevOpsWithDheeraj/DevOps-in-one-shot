
## 🐳 **1. What is Docker?**

**Docker** is an open-source **containerization platform** that allows developers and DevOps engineers to package an application and all its dependencies (libraries, configuration files, environment variables) into a **container** — ensuring that it runs **consistently across different environments** (development, testing, and production).

👉 Think of a container as a **lightweight, portable box** that includes everything your app needs to run.

---

## 💡 **2. Why Do We Need Docker?**

Before Docker, applications often suffered from the *“it works on my machine”* problem — where code ran perfectly in one environment but failed in another due to dependency or OS differences.

Docker solves this by providing:

* **Consistency:** Same environment everywhere.
* **Portability:** Run anywhere — your laptop, on-prem servers, or cloud.
* **Isolation:** Each app runs in its own container, independent of others.
* **Speed:** Containers start in seconds (unlike VMs which take minutes).
* **Efficiency:** Uses less memory and CPU because containers share the same OS kernel.

---

## 📦 **3. Docker Image and Container**

| Concept              | Description                                                                                                                                                   |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Docker Image**     | A **blueprint/template** for creating containers. It includes the OS libraries, dependencies, code, and configurations. (e.g., `python:3.10`, `nginx:latest`) |
| **Docker Container** | A **running instance** of an image. You can create multiple containers from a single image. Each container runs in isolation.                                 |

Example:

```bash
# Pull an image from Docker Hub
docker pull nginx

# Run the image (create and start a container)
docker run -d -p 8080:80 nginx
```

---

## 🧰 **4. Common Docker Commands**

| Command                     | Description                                  |
| --------------------------- | -------------------------------------------- |
| `docker pull <image>`       | Download an image from Docker Hub            |
| `docker images`             | List available images                        |
| `docker run <image>`        | Create & start a container                   |
| `docker ps`                 | List running containers                      |
| `docker ps -a`              | List all containers (including stopped ones) |
| `docker stop <id>`          | Stop a running container                     |
| `docker rm <id>`            | Remove a container                           |
| `docker rmi <image>`        | Remove an image                              |
| `docker exec -it <id> bash` | Access container shell                       |
| `docker logs <id>`          | View container logs                          |

---

## 🧱 **5. Docker vs Virtual Machine**

| Feature           | Docker (Container)                      | Virtual Machine                       |
| ----------------- | --------------------------------------- | ------------------------------------- |
| **Isolation**     | Process-level                           | Full OS-level                         |
| **Startup Time**  | Seconds                                 | Minutes                               |
| **Size**          | Lightweight (MBs)                       | Heavy (GBs)                           |
| **Performance**   | Near-native                             | Slightly slower                       |
| **OS Dependency** | Shares host OS kernel                   | Each VM has its own OS                |
| **Use Case**      | Microservices, CI/CD, Cloud-native apps | Monolithic apps, full OS environments |

👉 **Docker uses the host OS kernel**, while **VMs use a hypervisor** to run full operating systems.

---

## 🌐 **6. Port Mapping**

Containers have their own internal network. To access an app running inside a container from your host, you must **map ports**.

Example:

```bash
docker run -d -p host_port:container_port nginx

docker run -d -p 8080:80 nginx
```

This means:

* `80` → internal container port (Nginx default)
* `8080` → host port

Access the app at 👉 `http://localhost:8080`

---

## ⚙️ **7. Setting Environment Variables**

You can pass environment variables to containers using `-e` flag.

Example:

```bash
docker run -e USER=admin -e PASSWORD=1234 myapp
```

You can also define them in a **Dockerfile** (explained below) or in **docker-compose.yml**.

---

## 🧾 **8. Dockerfile (Building Custom Images)**

A **Dockerfile** is a text file that contains a series of instructions to build a Docker image.

Example:

```dockerfile
# Base image
FROM python:3.10

# Set working directory
WORKDIR /app

# Copy application files
COPY . /app

# Install dependencies
RUN pip install -r requirements.txt

# Set environment variable
ENV PORT=5000

# Expose port
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
```

Then build and run:

```bash
docker build -t myapp:v1 .
docker run -p 5000:5000 myapp:v1
```

---

## 🧩 **9. Docker Compose**

When you have **multiple containers** (e.g., backend, frontend, database), managing them manually is hard.
**Docker Compose** uses a YAML file to define and manage multiple containers together.

Example: `docker-compose.yml`

```yaml
version: '3.9'

services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    depends_on:
      - db

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: appdb
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:

```

Commands:

```bash
docker-compose up -d
docker-compose down
```

---

## 🏗️ **10. Multi-Stage Dockerfile**

Multi-stage builds help **reduce image size** and **improve security** by separating build and runtime stages.

Example:

```dockerfile
# Stage 1: Build
FROM golang:1.18 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

# Stage 2: Runtime
FROM alpine:latest
WORKDIR /app
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

✅ The final image only includes the compiled binary, not the build tools.

---

## 💾 **11. Docker Volumes (Persistent Storage)**

Containers are **ephemeral** — when they stop, data is lost.
**Volumes** are used to persist data outside the container.

Example:

```bash
docker run -d -v mydata:/var/lib/mysql mysql
```

* `mydata` is a Docker-managed volume.
* It stores MySQL data persistently.

You can list volumes:

```bash
docker volume ls
```

---

## 🌍 **12. Docker Network**

Docker networks allow containers to communicate securely.

Types of networks:

| Type        | Description                                |
| ----------- | ------------------------------------------ |
| **bridge**  | Default network for standalone containers. |
| **host**    | Container shares the host’s network.       |
| **overlay** | Used for multi-host Docker Swarm setups.   |

Example:

```bash
docker network create mynet
docker run -d --network=mynet --name=db mysql
docker run -d --network=mynet --name=app myapp
```

Now both containers can communicate using service names (`db`, `app`).

---

