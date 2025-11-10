
## 🐳 **What is a Dockerfile?**

A **Dockerfile** is a **text file** that contains a series of **instructions** (commands) which Docker reads to **automatically build an image**.

Think of it as a **recipe** for creating a Docker image:

* Each **instruction** in the Dockerfile adds a **layer** to the image.
* The final output of building a Dockerfile is a **Docker image** that you can run as a **container**.

---

## 🧱 **Basic Dockerfile Example**

```dockerfile
# Step 1: Base image
FROM python:3.9-slim

# Step 2: Set working directory
WORKDIR /app

# Step 3: Copy application files
COPY . .

# Step 4: Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Step 5: Expose port
EXPOSE 5000

# Step 6: Command to run the app
CMD ["python", "app.py"]
```

---

## 🔍 **Explain Each Term in Detail**

### 1️⃣ `FROM`

Defines the **base image** — the starting point of your build.
Examples:

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
FROM python:3.9-slim

LABEL maintainer="Dheeraj Kumar"
WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

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

## ⚙️ **Multi-Stage Dockerfile (Advanced & Important)**

Multi-stage builds help to:

* Reduce image size 🧩
* Separate build tools from runtime environment 🧱

Example — building a Go application:

```dockerfile
# ---------- Stage 1: Build ----------
FROM golang:1.20 AS builder
WORKDIR /app

COPY . .
RUN go build -o myapp .

# ---------- Stage 2: Run ----------
FROM alpine:latest
WORKDIR /app

# Copy only the compiled binary from the builder stage
COPY --from=builder /app/myapp .

CMD ["./myapp"]
```

### 🔍 Explanation:

* **Stage 1 (builder)**: Uses the full Golang image to compile your code.
* **Stage 2 (final)**: Copies only the final binary to a minimal Alpine image.
* Result: Small, secure image with only what’s needed to run the app.

---

## 🧠 **Analogy**

Think of it like a **restaurant kitchen**:

* `FROM` → The base kitchen setup.
* `RUN` → Cooking ingredients.
* `COPY` → Bringing ingredients in.
* `CMD` → Serving the final dish.
* `Multi-stage` → Cooking in one kitchen, plating in another (clean one).

---

## 🧩 **Best Practices**

✅ Use small base images like `alpine`
✅ Combine multiple `RUN` commands to reduce layers
✅ Use `.dockerignore` to skip unnecessary files
✅ Use `multi-stage builds` for small and secure images
✅ Keep secrets out of Dockerfiles

---
