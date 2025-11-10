# 🐳 Containerization with Docker 

## 📘 Introduction

**Containerization** is a lightweight virtualization technique that packages **applications with all their dependencies** into isolated units called **containers**.  

**Docker** is the most widely used container platform in DevOps. It ensures that **apps run consistently across environments** — from developer machines to production servers.

---

## 🧩 1. Containerization Basics

### 🔹 What is a Container?
- Isolated runtime environment for an application  
- Shares host OS kernel  
- Lightweight compared to virtual machines  

### 🔹 Key Concepts
| Concept | Description |
|---------|------------|
| Image | Read-only template for containers (e.g., `nginx:latest`) |
| Container | Running instance of an image |
| Dockerfile | Script to build images |
| Registry | Store for images (Docker Hub, ECR, Artifactory) |
| Volume | Persistent storage for containers |
| Network | Container communication (bridge, host, overlay) |

---

## 🧰 2. Docker Installation

**Linux Example:**
```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
docker --version
````

Add user to Docker group:

```bash
sudo usermod -aG docker $USER
```

Verify installation:

```bash
docker run hello-world
```

---

## 🔹 3. Docker Basic Commands

| Command         | Purpose                          | Example                             |
| --------------- | -------------------------------- | ----------------------------------- |
| `docker pull`   | Download image                   | `docker pull nginx:latest`          |
| `docker images` | List local images                | `docker images`                     |
| `docker run`    | Start a container                | `docker run -d -p 8080:80 nginx`    |
| `docker ps`     | List running containers          | `docker ps`                         |
| `docker exec`   | Execute command inside container | `docker exec -it container_id bash` |
| `docker stop`   | Stop container                   | `docker stop container_id`          |
| `docker rm`     | Remove container                 | `docker rm container_id`            |
| `docker rmi`    | Remove image                     | `docker rmi nginx:latest`           |

---

## 🔹 4. Dockerfile – Build Images as Code

**Example Dockerfile (Node.js app):**

```dockerfile
# Base image
FROM node:18

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy app source code
COPY . .

# Expose app port
EXPOSE 3000

# Run app
CMD ["node", "index.js"]
```

**Build & Run:**

```bash
docker build -t dheeraj/node-app:1.0 .
docker run -d -p 3000:3000 dheeraj/node-app:1.0
```

---

## 🧩 5. Docker Networking Basics

### Types of Networks:

| Type    | Description                                           |
| ------- | ----------------------------------------------------- |
| bridge  | Default, container-to-container on same host          |
| host    | Container shares host network                         |
| overlay | Multi-host container communication (Docker Swarm/K8s) |

**Example: Container communication**

```bash
docker network create devops-net
docker run -d --name web --network devops-net nginx
docker run -d --name app --network devops-net myapp:latest
docker exec web ping app
```

---

## 🐳 6. Docker Volumes

* Persistent storage for containers
* Mount local directories or managed volumes

**Example:**

```bash
docker volume create app-data
docker run -d -v app-data:/app/data myapp:latest
```

---

## ⚡ 7. Docker Compose – Multi-Container Apps

**docker-compose.yml Example:**

```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  app:
    image: dheeraj/node-app:latest
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
```

**Commands:**

```bash
docker-compose up -d   # Start containers
docker-compose down    # Stop containers
docker-compose logs    # View logs
```

✅ Automates multi-container setups.

---

## 🔹 8. Docker Registry

* **Docker Hub** – public registry
* **AWS ECR, Nexus, Artifactory** – private registry
* Push & pull images to share across environments:

```bash
docker tag node-app:1.0 dheeraj/node-app:1.0
docker push dheeraj/node-app:1.0
docker pull dheeraj/node-app:1.0
```

---

## 🔹 9. Advanced Docker Concepts

1. **Multi-stage builds** – reduce image size

```dockerfile
# Build stage
FROM node:18 as build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production stage
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```

2. **Docker Secrets & Env Variables** – secure sensitive data

```bash
docker run -e DB_PASS=secret myapp
```

3. **Healthchecks & Auto-restart**

```dockerfile
HEALTHCHECK --interval=30s CMD curl -f http://localhost:3000/health || exit 1
```

4. **Integration with CI/CD**

* Jenkins pipeline builds Docker image → pushes to registry → deploys to Kubernetes

**Example Jenkins Pipeline snippet:**

```groovy
stage('Docker Build & Push') {
  steps {
    sh 'docker build -t dheeraj/node-app:latest .'
    sh 'docker push dheeraj/node-app:latest'
  }
}
```

---

## 🧩 10. Hands-on Docker Lab (DevOps Scenario)

**Scenario:** Deploy a web + app stack on a single host

1. Create Docker network:

```bash
docker network create devops-net
```

2. Run containers:

```bash
docker run -d --name web --network devops-net -p 8080:80 nginx
docker run -d --name app --network devops-net -p 3000:3000 myapp:latest
```

3. Verify container connectivity:

```bash
docker exec web ping app
curl http://localhost:3000
```

4. Automate with Docker Compose:

```bash
docker-compose up -d
```

✅ Multi-container deployment achieved, ready for CI/CD integration.

---

## 🏁 Summary

| Level           | Topics Covered                                                                             |
| --------------- | ------------------------------------------------------------------------------------------ |
| 🟢 Beginner     | Docker installation, images, containers, basic commands                                    |
| 🟡 Intermediate | Dockerfile, volumes, networking, multi-container apps                                      |
| 🔴 Advanced     | Multi-stage builds, CI/CD integration, healthchecks, secrets, production-ready deployments |

> 💬 “Containers make applications **portable, consistent, and scalable**, the foundation of modern DevOps pipelines.”

---
