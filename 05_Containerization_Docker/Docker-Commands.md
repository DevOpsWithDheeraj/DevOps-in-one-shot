
# 🐳 **Docker Commands — Category-wise with Examples**

---

## 🧱 **1. Basic Docker Commands**

| Command                 | Description                       | Example               |
| ----------------------- | --------------------------------- | --------------------- |
| `docker --version`      | Check Docker version              | `docker --version`    |
| `docker info`           | Display system-wide information   | `docker info`         |
| `docker help`           | Show all commands and usage help  | `docker help`         |
| `docker login`          | Log in to Docker Hub registry     | `docker login`        |
| `docker logout`         | Log out from Docker Hub           | `docker logout`       |
| `docker search <image>` | Search for an image on Docker Hub | `docker search nginx` |

---

## 📦 **2. Docker Image Commands**

Images are the *blueprints* for containers.

| Command                             | Description                       | Example                                 |
| ----------------------------------- | --------------------------------- | --------------------------------------- |
| `docker pull <image>`               | Download image from Docker Hub    | `docker pull ubuntu:latest`             |
| `docker images`                     | List all local images             | `docker images`                         |
| `docker rmi <image>`                | Remove an image                   | `docker rmi nginx:latest`               |
| `docker tag <src> <target>`         | Tag an image with a new name      | `docker tag ubuntu:latest myubuntu:v1`  |
| `docker save -o <file>.tar <image>` | Save an image to a tar file       | `docker save -o nginx.tar nginx:latest` |
| `docker load -i <file>.tar`         | Load image from a tar file        | `docker load -i nginx.tar`              |
| `docker history <image>`            | Show image layers                 | `docker history nginx`                  |
| `docker inspect <image>`            | Show detailed info about an image | `docker inspect ubuntu`                 |
| `docker build -t <name> .`          | Build image from Dockerfile       | `docker build -t myapp:v1 .`            |

---

## 🧰 **3. Docker Container Commands**

Containers are **running instances** of images.

| Command                            | Description                           | Example                                          |
| ---------------------------------- | ------------------------------------- | ------------------------------------------------ |
| `docker run <image>`               | Run a new container                   | `docker run ubuntu`                              |
| `docker run -it <image> bash`      | Run container in interactive mode     | `docker run -it ubuntu bash`                     |
| `docker run -d <image>`            | Run container in detached mode        | `docker run -d nginx`                            |
| `docker run --name <name> <image>` | Give custom name to container         | `docker run --name web nginx`                    |
| `docker ps`                        | List running containers               | `docker ps`                                      |
| `docker ps -a`                     | List all containers (stopped too)     | `docker ps -a`                                   |
| `docker start <container>`         | Start a stopped container             | `docker start web`                               |
| `docker stop <container>`          | Stop a running container              | `docker stop web`                                |
| `docker restart <container>`       | Restart a container                   | `docker restart web`                             |
| `docker rm <container>`            | Remove a container                    | `docker rm web`                                  |
| `docker exec -it <container> bash` | Execute a command inside container    | `docker exec -it web bash`                       |
| `docker logs <container>`          | View container logs                   | `docker logs web`                                |
| `docker inspect <container>`       | Get detailed container info           | `docker inspect web`                             |
| `docker stats`                     | Display live resource usage           | `docker stats`                                   |
| `docker top <container>`           | Show running processes                | `docker top web`                                 |
| `docker rename <old> <new>`        | Rename a container                    | `docker rename web nginx-server`                 |
| `docker attach <container>`        | Attach to a running container session | `docker attach web`                              |
| `docker cp <src> <dest>`           | Copy files to/from a container        | `docker cp index.html web:/usr/share/nginx/html` |

---

## 🌐 **4. Docker Networking Commands**

| Command                                           | Description                  | Example                                    |
| ------------------------------------------------- | ---------------------------- | ------------------------------------------ |
| `docker network ls`                               | List all networks            | `docker network ls`                        |
| `docker network create <name>`                    | Create a new network         | `docker network create my-network`         |
| `docker network inspect <name>`                   | Inspect a network            | `docker network inspect bridge`            |
| `docker network connect <network> <container>`    | Connect container to network | `docker network connect my-network web`    |
| `docker network disconnect <network> <container>` | Disconnect container         | `docker network disconnect my-network web` |
| `docker network rm <network>`                     | Remove a network             | `docker network rm my-network`             |

**Example:**

```bash
docker network create --driver bridge my-net
docker run -d --name db --network my-net mysql
docker run -d --name app --network my-net myapp
```

---

## 💾 **5. Docker Volume Commands**

Volumes store **persistent data** outside containers.

| Command                        | Description            | Example                        |
| ------------------------------ | ---------------------- | ------------------------------ |
| `docker volume ls`             | List all volumes       | `docker volume ls`             |
| `docker volume create <name>`  | Create a volume        | `docker volume create mydata`  |
| `docker volume inspect <name>` | Inspect volume details | `docker volume inspect mydata` |
| `docker volume rm <name>`      | Remove volume          | `docker volume rm mydata`      |

**Example:**

```bash
docker run -d --name db -v mydata:/var/lib/mysql mysql
```

---

## 🧩 **6. Dockerfile Related Commands**

| Command                    | Description                 | Example                            |
| -------------------------- | --------------------------- | ---------------------------------- |
| `docker build -t <tag> .`  | Build image from Dockerfile | `docker build -t myapp:v1 .`       |
| `docker build -f <file> .` | Specify Dockerfile name     | `docker build -f Dockerfile.dev .` |
| `docker history <image>`   | Show image build history    | `docker history myapp:v1`          |

---

## ⚙️ **7. Docker Compose Commands**

Used for multi-container apps defined in a `docker-compose.yml` file.

| Command                 | Description                         | Example                   |
| ----------------------- | ----------------------------------- | ------------------------- |
| `docker compose up`     | Start all services                  | `docker compose up`       |
| `docker compose up -d`  | Run in detached mode                | `docker compose up -d`    |
| `docker compose down`   | Stop and remove containers/networks | `docker compose down`     |
| `docker compose ps`     | List running compose services       | `docker compose ps`       |
| `docker compose logs`   | Show logs                           | `docker compose logs web` |
| `docker compose build`  | Build images                        | `docker compose build`    |
| `docker compose config` | Validate and view merged config     | `docker compose config`   |

---

## 🕸️ **8. Docker Swarm Commands**

| Command                         | Description                    | Example                                               |
| ------------------------------- | ------------------------------ | ----------------------------------------------------- |
| `docker swarm init`             | Initialize a Swarm cluster     | `docker swarm init --advertise-addr 192.168.1.10`     |
| `docker swarm join`             | Join node to cluster           | `docker swarm join --token <token> 192.168.1.10:2377` |
| `docker node ls`                | List all nodes                 | `docker node ls`                                      |
| `docker service create`         | Deploy a service               | `docker service create --name web -p 80:80 nginx`     |
| `docker service ls`             | List services                  | `docker service ls`                                   |
| `docker service ps <service>`   | List containers in service     | `docker service ps web`                               |
| `docker service scale`          | Scale services                 | `docker service scale web=5`                          |
| `docker stack deploy`           | Deploy stack from compose file | `docker stack deploy -c docker-compose.yml mystack`   |
| `docker stack services mystack` | List stack services            | `docker stack services mystack`                       |
| `docker swarm leave`            | Leave swarm                    | `docker swarm leave --force`                          |

---

## 🧼 **9. Docker System Maintenance Commands**

| Command                  | Description               | Example                  |
| ------------------------ | ------------------------- | ------------------------ |
| `docker system df`       | Show disk usage           | `docker system df`       |
| `docker system prune`    | Remove unused data        | `docker system prune -a` |
| `docker image prune`     | Remove unused images      | `docker image prune`     |
| `docker container prune` | Remove stopped containers | `docker container prune` |
| `docker volume prune`    | Remove unused volumes     | `docker volume prune`    |

---

## 🧪 **10. Docker Registry / Push-Pull Commands**

| Command                                  | Description                | Example                                |
| ---------------------------------------- | -------------------------- | -------------------------------------- |
| `docker tag <image> <user>/<repo>:<tag>` | Tag image for Docker Hub   | `docker tag myapp:v1 dheeraj/myapp:v1` |
| `docker push <user>/<repo>:<tag>`        | Push image to Docker Hub   | `docker push dheeraj/myapp:v1`         |
| `docker pull <user>/<repo>:<tag>`        | Pull image from Docker Hub | `docker pull dheeraj/myapp:v1`         |

---

## 🧾 **11. Docker Security & Inspect**

| Command                   | Description                          | Example                |
| ------------------------- | ------------------------------------ | ---------------------- |
| `docker inspect <obj>`    | Inspect image/container details      | `docker inspect nginx` |
| `docker diff <container>` | Show changes to container filesystem | `docker diff web`      |
| `docker stats`            | Show real-time resource usage        | `docker stats`         |

---

## 🧠 **12. Miscellaneous Useful Commands**

| Command                               | Description               | Example                        |
| ------------------------------------- | ------------------------- | ------------------------------ |
| `docker export <container> -o <file>` | Export container as tar   | `docker export web -o web.tar` |
| `docker import <file>`                | Import exported container | `docker import web.tar`        |
| `docker events`                       | Show real-time events     | `docker events`                |
| `docker port <container>`             | Show port mapping         | `docker port web`              |

---

## 🧰 Example Workflow

```bash
# Step 1: Pull image
docker pull nginx

# Step 2: Run container
docker run -d --name web -p 8080:80 nginx

# Step 3: Check running containers
docker ps

# Step 4: View logs
docker logs web

# Step 5: Stop and remove
docker stop web && docker rm web
```

---

## 🧩 Summary Mindmap

```
Docker
├── Images → build, pull, tag, push
├── Containers → run, exec, stop, rm
├── Networks → create, connect, inspect
├── Volumes → create, mount, inspect
├── Compose → up, down, build
├── Swarm → init, join, deploy, scale
├── System → prune, stats, df
```

---
