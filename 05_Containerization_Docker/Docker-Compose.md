
## 🚢 What is **Docker Compose**?

**Docker Compose** is a tool used to **define and run multi-container Docker applications**.

Instead of running many `docker run` commands manually, you can describe all containers, networks, and volumes in **one file — `docker-compose.yml`**, and then start everything with a single command:

```bash
docker-compose up
```

---

## 🧩 Why We Need Docker Compose

Imagine a simple web application:

* A **frontend** (React/Angular)
* A **backend** (Python Flask / Node.js)
* A **database** (MySQL / MongoDB)

Each of these runs in its **own container**. Managing all of them with separate Docker commands is painful.
**Docker Compose** simplifies it — you define all containers and how they interact in a single YAML file.

---

## 🏗️ Basic Structure of `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    image: nginx
    ports:
      - "8080:80"

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
```

### Explanation:

* **version:** Compose file format version.
* **services:** Each service is a container.

  * `web`: runs the nginx container and maps port `8080` on host to `80` in container.
  * `db`: runs a MySQL container with an environment variable to set the root password.

---

## 🧠 Common Commands

| Command                  | Description                                           |
| ------------------------ | ----------------------------------------------------- |
| `docker-compose up`      | Start all services defined in `docker-compose.yml`    |
| `docker-compose up -d`   | Start in detached (background) mode                   |
| `docker-compose down`    | Stop and remove all containers, networks, and volumes |
| `docker-compose ps`      | List running containers                               |
| `docker-compose logs`    | View container logs                                   |
| `docker-compose build`   | Build or rebuild services defined with Dockerfile     |
| `docker-compose restart` | Restart services                                      |

---

## 🧱 Example: Web App + Database

Let’s build a **Flask + MySQL** example.

### 1️⃣ Folder Structure

```
flask-mysql-app/
│
├── app/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
└── docker-compose.yml
```

---

### 2️⃣ `app.py`

```python
from flask import Flask
import os
import mysql.connector

app = Flask(__name__)

@app.route('/')
def hello():
    db = mysql.connector.connect(
        host=os.getenv('DB_HOST', 'db'),
        user=os.getenv('DB_USER', 'root'),
        password=os.getenv('DB_PASSWORD', 'root')
    )
    return "Connected to MySQL successfully!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

---

### 3️⃣ `requirements.txt`

```
flask
mysql-connector-python
```

---

### 4️⃣ `Dockerfile`

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

---

### 5️⃣ `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    build: ./app
    ports:
      - "5000:5000"
    environment:
      DB_HOST: db
      DB_USER: root
      DB_PASSWORD: root
    depends_on:
      - db

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

---

### 🔄 Run the App

```bash
docker-compose up
```

✅ Flask app will run on `http://localhost:5000` <br>
✅ MySQL runs in a separate container. <br>
✅ Compose automatically creates a **network** so `web` can connect to `db` using its service name (`db`). <br>

---

## ⚙️ Advanced Features

### 🧩 1. **Environment Variables File**

You can keep sensitive data in `.env` file:

```
MYSQL_ROOT_PASSWORD=mysecret
```

Then use it in `docker-compose.yml`:

```yaml
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
```

---

### 🧩 2. **Multiple Containers for Scaling**

```bash
docker-compose up --scale web=3
```

This runs **3 containers** for the `web` service behind the same network.

---

### 🧩 3. **Networks**

Compose automatically creates a default network, but you can define custom ones:

```yaml
networks:
  mynet:
    driver: bridge
```

Then assign it:

```yaml
services:
  web:
    networks:
      - mynet
  db:
    networks:
      - mynet
```

---

### 🧩 4. **Volumes**

Persistent data for databases or shared files:

```yaml
volumes:
  db_data:
    driver: local
```

---

## 🆚 Docker Compose vs Kubernetes

| Feature       | Docker Compose        | Kubernetes                     |
| ------------- | --------------------- | ------------------------------ |
| Scope         | Local / small setups  | Production-grade orchestration |
| Scaling       | Manual                | Automatic                      |
| Health checks | Limited               | Advanced                       |
| Best for      | Development / testing | Large-scale deployment         |

---

## 💡 Summary

| Concept      | Description                                                          |
| ------------ | -------------------------------------------------------------------- |
| **Purpose**  | Manage multi-container apps easily                                   |
| **File**     | `docker-compose.yml`                                                 |
| **Command**  | `docker-compose up/down`                                             |
| **Benefits** | Simplifies orchestration, config management, networking, and scaling |

---
