
## 🧩 **What is a Multi-Stage Dockerfile?**

A **multi-stage Dockerfile** is a technique in Docker that allows you to **use multiple `FROM` statements** in a single Dockerfile.
Each stage builds on top of the previous one, and you can **copy only the necessary files** (like a binary or artifact) from one stage to another.

👉 **Goal:**
To create **small, optimized images** by removing build dependencies, source code, and temporary files that are only needed during the build process.

---

## 💡 **Why We Need Multi-Stage Builds**

Without multi-stage builds:

* You install compilers, build tools, etc., in your image → **image becomes huge** (e.g., 1 GB+).
* Your final image includes unnecessary files.

With multi-stage builds:

* You build your app in one stage (with build tools).
* Then copy only the final binary or artifact into a **clean runtime image**.

✅ Result: **Smaller, cleaner, more secure image.**

---

## 🏗️ **Basic Example: Go Application**

Let’s say you have a Go application (`main.go`).

### 🧱 **Dockerfile (Multi-Stage Example)**

```dockerfile
# -------- Stage 1: Build Stage --------
FROM golang:1.22 AS builder

# Set working directory
WORKDIR /app

# Copy Go files
COPY . .

# Download dependencies and build binary
RUN go mod download
RUN go build -o myapp

# -------- Stage 2: Run Stage --------
FROM alpine:latest

# Set working directory
WORKDIR /root/

# Copy only the binary from builder stage
COPY --from=builder /app/myapp .

# Expose port (optional)
EXPOSE 8080

# Run the app
CMD ["./myapp"]
```
---

## ⚙️ **How to Build & Run**

```bash
docker build -t go-multistage-app .
docker run -p 8080:8080 go-multistage-app
```

---

## 📦 **Multi-Stage for Node.js Example**

```dockerfile
# Stage 1: Build React App
FROM node:20 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Stage 2: NGINX for serving build
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

🧠 Here:

* Stage 1 uses Node to build the app.
* Stage 2 only contains NGINX + static build files → final image is very small (~30MB).

---

## 🧾 **Advantages**

✅ Smaller final image size
✅ Faster deployment and security scanning
✅ Cleaner layers (no temporary files)
✅ Easier to maintain build logic in one Dockerfile

---

## 🧱 **Real-World Use Case (Java + Maven Example)**

```dockerfile
# Stage 1: Build JAR
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Run JAR
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY --from=builder /app/target/myapp.jar .
EXPOSE 8080
CMD ["java", "-jar", "myapp.jar"]
```

🧩 **Result:**
Final image only has the JAR + Java runtime — not Maven, dependencies, or source code.

---

## 🧠 **Summary Table**

| Feature                    | Single-Stage | Multi-Stage             |
| -------------------------- | ------------ | ----------------------- |
| Build tools in final image | ✅ Yes        | ❌ No                    |
| Image size                 | Large        | Small                   |
| Security                   | Lower        | Higher                  |
| Complexity                 | Simple       | Moderate                |
| Best for                   | Small apps   | Production-grade builds |

---
