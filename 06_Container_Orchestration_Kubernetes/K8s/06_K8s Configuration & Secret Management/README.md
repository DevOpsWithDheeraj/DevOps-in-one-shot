

# 🚀 **Kubernetes Configuration & Secret Management**

Modern applications need:

* Environment variables
* App configuration
* URLs, file paths
* Credentials, DB passwords
* API keys

Instead of hardcoding these in Pods or Docker images, Kubernetes uses:

### ✔ **ConfigMap → Non-sensitive configuration**

### ✔ **Secret → Sensitive configuration (base64-encoded or encrypted)**

---

# 1️⃣ **ConfigMap – Store App Configuration (Non-Sensitive Data)**

A **ConfigMap** stores **plain, non-confidential configuration data**.

### ✔ What can it store?

* Environment variables
* App config files (JSON, YAML, INI, properties)
* Command-line arguments
* Key-value pairs

### ❌ Should NOT store:

* Passwords
* Tokens
* API keys

---

# ✔ Ways to use ConfigMaps in Pods

1. As environment variables
2. As command-line arguments
3. As mounted configuration files

---

# 📘 Example 1: Simple ConfigMap (Key-Value)

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_MODE: "production"
  APP_PORT: "8080"
```

### Use in Pod (as env vars)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: app
spec:
  containers:
  - name: demo
    image: nginx
    env:
    - name: APP_MODE
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_MODE
    - name: APP_PORT
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_PORT
```

---

# 📘 Example 2: ConfigMap as a File

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: config-file
data:
  config.json: |
    {
      "version": "1.0",
      "logging": "info"
    }
```

Mount as file:

```yaml
volumeMounts:
- name: config
  mountPath: /etc/config
volumes:
- name: config
  configMap:
    name: config-file
```

Now inside the container:

```
/etc/config/config.json
```

---

# 🧠 Key Benefits of ConfigMaps

* Update configuration **without rebuilding Docker image**
* Keep code and config separate
* Version control friendly
* Can be reloaded dynamically with sidecar tools (like Reloader)

---

# 2️⃣ **Secret – Store Sensitive Information Securely**

A **Secret** stores sensitive data such as:

### ✔ Secure items to store:

* Passwords
* API keys
* TLS certificates
* Tokens
* SSH keys

### ✔ Advantages over ConfigMaps:

* Base64 encoded
* Can be **encrypted at rest** with KMS
* Mounted as tmpfs → never written to disk
* Can restrict access via RBAC

---

# 📘 Types of Secrets

* `Opaque` – general secrets
* `docker-registry` – Docker credentials
* `tls` – cert + private key

---

# 📘 Example 1: Opaque Secret (Base64 Encoded)

Create secret:

```
echo -n "admin123" | base64
```

Output:

```
YWRtaW4xMjM=
```

Create Secret YAML:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: YWRtaW4=
  password: YWRtaW4xMjM=
```

---

# 📘 Use Secret as Environment Variables

```yaml
env:
- name: DB_USER
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: username
- name: DB_PASS
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

Inside the container:

```
DB_USER = admin
DB_PASS = admin123
```

---

# 📘 Example 2: Secret as Mounted File

```yaml
volumes:
- name: certs
  secret:
    secretName: tls-secret
volumeMounts:
- name: certs
  mountPath: "/etc/tls"
```

Files inside container:

```
/etc/tls/tls.crt
/etc/tls/tls.key
```

---

# 📘 Example 3: Secret for TLS (HTTPS)

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: tls-secret
type: kubernetes.io/tls
data:
  tls.crt: <base64>
  tls.key: <base64>
```

Used in:

* Ingress for HTTPS
* Applications needing SSL

---

# 🧠 Key Benefits of Secrets

* Sensitive info not visible in Pod spec
* RBAC + encryption at rest support
* Auto-mounted in tmpfs → RAM only
* Decouples credentials from application

---

# 🔥 ConfigMap vs Secret (Quick Comparison)

| Feature             | ConfigMap         | Secret              |
| ------------------- | ----------------- | ------------------- |
| Purpose             | App configuration | Sensitive data      |
| Encoding            | Plain text        | Base64              |
| Encryption at rest  | No                | Yes (with KMS)      |
| Mounted as env/file | Yes               | Yes                 |
| Common Usage        | URLs, settings    | Passwords, API keys |

---

# 🧠 Best Practices for Config & Secret Management

### ✔ ConfigMaps

* Keep only **non-sensitive** configs
* Separate config per environment
* Use versioning (GitOps)

### ✔ Secrets

* Encrypt with KMS (AWS, GCP, Azure)
* Use sealed-secrets or External Secrets Operator
* Restrict with RBAC
* Never put secrets in images or GitHub

---

# 📦 Real-World Usage Example: App + DB

### ConfigMap for the configuration:

```yaml
APP_ENV=prod
LOG_LEVEL=info
```

### Secret for DB credentials:

```yaml
username: admin
password: admin123
```

### Pod uses both:

```yaml
env:
- name: APP_ENV
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_ENV

- name: DB_USERNAME
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: username
```

---

# 🧠 Final Summary

| Component     | Purpose                                             |
| ------------- | --------------------------------------------------- |
| **ConfigMap** | Store non-sensitive configuration (env vars, files) |
| **Secret**    | Store sensitive data (passwords, tokens, TLS certs) |
| **Pod Usage** | Env variables or mounted files                      |
| **Security**  | Secrets support encryption & RBAC                   |

---
