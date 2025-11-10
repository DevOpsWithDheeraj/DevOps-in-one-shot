
## 🐳 **What is a Dockerfile?**

A **Dockerfile** is a **text file** that contains a series of **instructions** (commands) which Docker reads to **automatically build an image**.

Think of it as a **recipe** for creating a Docker image:

* Each **instruction** in the Dockerfile adds a **layer** to the image.
* The final output of building a Dockerfile is a **Docker image** that you can run as a **container**.

---

## 1. 🧱 **Basic Dockerfile **

```dockerfile
# Step 1: Base image
FROM python:3.9-alpine

# Step 2: Set working directory
WORKDIR /app

# Step 3: Copy application files
COPY . .

# Step 4: Install dependencies
RUN pip install -r requirements.txt

# Step 5: Expose port
EXPOSE 5000

# Step 6: Command to run the app
CMD ["python", "app.py"]
```

---

## 🔍 **Explain Each Term in Detail**

### 1️⃣ `FROM`

Defines the **base image** — the starting point of your build. Examples:
```dockerfile
FROM ubuntu:20.04
FROM python:3.9-alpine
FROM node:18
```

* You can think of it as: “Start from this operating system/environment.”
* Every Dockerfile **must begin with `FROM`** (except for *scratch images*).

---

### 2️⃣ `WORKDIR`

Sets the **current working directory** inside the container.
Example:

```dockerfile
WORKDIR /app
```

* Equivalent to running `cd /app` inside the container.
* All following commands will execute in this directory.

---

### 3️⃣ `COPY`

Copies files from your **local machine** (build context) into the **container**.
Example:

```dockerfile
COPY . .
```

* First `.` = local directory
* Second `.` = current directory inside the container (WORKDIR)

You can also copy specific files:

```dockerfile
COPY requirements.txt /app/requirements.txt
```

---

### 4️⃣ `RUN`

Executes commands **inside the image** during build time.
Example:

```dockerfile
RUN pip install -r requirements.txt
```

* Used to install packages or build dependencies.
* Each `RUN` creates a new **layer** in the image.

---

### 5️⃣ `EXPOSE`

Documents which **port** the container listens on at runtime.
Example:

```dockerfile
EXPOSE 5000
```

It does **not** actually publish the port — it’s just metadata.
You still need `-p` or `--publish` when running the container:

```bash
docker run -p 5000:5000 myapp
```

---

### 6️⃣ `CMD`

Specifies the **default command** that runs when the container starts.
Example:

```dockerfile
CMD ["python", "app.py"]
```

* It’s the **main process** of your container.
* Only **one CMD** per Dockerfile is allowed (the last one overrides previous).

---

### 7️⃣ `ENTRYPOINT`

Also defines a command to run, but is **hard-coded** — usually for defining the main executable.
Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

If you run:

```bash
docker run myapp
```

It will execute → `python app.py`

If you run:

```bash
docker run myapp test.py
```

It will execute → `python test.py`

---

### 8️⃣ `ENV`

Sets **environment variables** inside the image.
Example:

```dockerfile
ENV APP_ENV=production
ENV PATH="/usr/src/app/bin:$PATH"
```

---

### 9️⃣ `ARG`

Defines **build-time variables**, available only while building the image.
Example:

```dockerfile
ARG VERSION=1.0
RUN echo "Version is $VERSION"
```

You can override it at build time:

```bash
docker build --build-arg VERSION=2.0 -t myapp .
```

---

### 🔟 `LABEL`

Adds **metadata** to the image.
Example:

```dockerfile
LABEL maintainer="Dheeraj Kumar <dheeraj@infosys.com>"
LABEL version="1.0"
```

---

## 🧩 **Complete Example**

```dockerfile
FROM python:3.9-alpine

LABEL maintainer="Dheeraj Kumar"
WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
ENV FLASK_ENV=production

EXPOSE 5000
CMD ["python", "app.py"]
```

To build and run:

```bash
docker build -t flask-app .
docker run -p 5000:5000 flask-app
```

---

## 2. ⚙️ **Multi-Stage Dockerfile**

### 🧩 What is a Multi-Stage Dockerfile?

A **Multi-Stage Dockerfile** allows you to use **multiple `FROM` statements** in a single Dockerfile — each creating a **separate build stage**.

👉 **Purpose:**
To **reduce the final image size** and **separate build dependencies from runtime dependencies**.

In simple terms:

> You compile or build your app in one stage (using a heavy image),
> and then copy only the required files into a lightweight final image.

---

### ⚙️ Why Multi-Stage Builds are Useful

| Problem (Without Multi-Stage)                                | Solution (With Multi-Stage)                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------- |
| Final image is large (includes build tools, compilers, etc.) | Only the required binaries or files are copied to final image |
| Manual cleanup required                                      | Automatically creates clean, small image                      |
| Complex Dockerfile                                           | Cleaner, modular structure                                    |

---

### 🧱 Example: Multi-Stage Dockerfile (Node.js + Nginx)

Let's say you have a React application that needs to be built and served using **Nginx**.

---

#### 🧾 Dockerfile

```dockerfile
# ---------- Stage 1: Build Stage ----------
FROM node:18 AS builder
WORKDIR /app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy source code and build the app
COPY . .
RUN npm run build

# ---------- Stage 2: Production Stage ----------
FROM nginx:alpine
WORKDIR /usr/share/nginx/html

# Copy build files from builder stage
COPY --from=builder /app/build .

# Expose port 80 and run Nginx
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

#### 🧠 Step-by-Step Explanation

##### **Stage 1: Build (Node.js)**

* `FROM node:18 AS builder` → use Node.js image for building.
* `WORKDIR /app` → set working directory inside container.
* `COPY package*.json ./` → copy dependency files first (for caching).
* `RUN npm install` → install all dependencies.
* `COPY . .` → copy all project files.
* `RUN npm run build` → build the optimized production version (creates `/app/build` folder).

##### **Stage 2: Run (Nginx)**

* `FROM nginx:alpine` → lightweight image to serve static files.
* `COPY --from=builder /app/build .` → copy built files from previous stage.
* `EXPOSE 80` → expose port 80 for web traffic.
* `CMD ["nginx", "-g", "daemon off;"]` → start Nginx in foreground.

---

### 📦 Build and Run the Docker Image

```bash
# Build the image
docker build -t my-react-app .

# Run the container
docker run -d -p 8080:80 my-react-app
```

Now, open your browser and go to 👉 **[http://localhost:8080](http://localhost:8080)**
You’ll see your React app running, built efficiently in just one Dockerfile.

---

### 🚀 Advantages of Multi-Stage Builds

✅ Smaller image size
✅ Faster deployment
✅ Clean separation of build and runtime
✅ Easier to maintain
✅ No leftover build tools in final image

---

### 🧰 Bonus Example (Go Application)

```dockerfile
# Stage 1: Build
FROM golang:1.20 AS builder
WORKDIR /app
COPY . .
RUN go build -o main .

# Stage 2: Run
FROM alpine
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]
```
---

## 🧩 **Best Practices**

✅ Use small base images like `alpine` <br>
✅ Combine multiple `RUN` commands to reduce layers <br>
✅ Use `.dockerignore` to skip unnecessary files <br>
✅ Use `multi-stage builds` for small and secure images <br>
✅ Keep secrets out of Dockerfiles <br>

---
