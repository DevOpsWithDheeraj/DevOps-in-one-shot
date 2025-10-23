# 📊 Monitoring & Logging (Prometheus, Grafana, ELK) 

## 📘 Introduction

Monitoring & Logging is a **critical part of DevOps** to ensure application reliability, performance, and proactive alerting.  

**Key Goals:**
- Track system and application metrics  
- Detect failures and bottlenecks early  
- Collect, analyze, and visualize logs  
- Integrate alerts into DevOps workflows  

**Popular Tools:**
- **Prometheus** – Metrics collection & alerting  
- **Grafana** – Visualization & dashboards  
- **ELK Stack** – Elasticsearch, Logstash, Kibana for centralized logging  

---

## 🧩 1. Prometheus – Metrics Monitoring

### 🔹 What is Prometheus?
- Open-source monitoring & alerting toolkit  
- Pull-based model: Prometheus **scrapes metrics** from exporters  
- Stores metrics as **time-series data**  

### 🔹 Installation
```bash
# Linux example
sudo useradd --no-create-home --shell /bin/false prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.49.0/prometheus-2.49.0.linux-amd64.tar.gz
tar xvfz prometheus-2.49.0.linux-amd64.tar.gz
sudo mv prometheus-2.49.0.linux-amd64 /etc/prometheus
````

### 🔹 Configuration (`prometheus.yml`)

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']
```

**Start Prometheus:**

```bash
./prometheus --config.file=prometheus.yml
```

---

## 🐧 2. Node Exporter – System Metrics

* Exposes Linux system metrics (CPU, memory, disk, network)
* Install & run:

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.1/node_exporter-1.7.1.linux-amd64.tar.gz
tar xvfz node_exporter-1.7.1.linux-amd64.tar.gz
cd node_exporter-1.7.1.linux-amd64
./node_exporter &
```

**Prometheus scrapes metrics from Node Exporter.**

---

## 📊 3. Grafana – Visualization

### 🔹 What is Grafana?

* Open-source **dashboard & visualization tool**
* Connects to **Prometheus, Elasticsearch, MySQL, etc.**
* Supports **alerting** and annotations

### 🔹 Installation

```bash
sudo apt update
sudo apt install -y grafana
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

* Access Grafana at `http://<server-ip>:3000`
* Default login: `admin/admin`

### 🔹 Create Dashboard

1. Add **Prometheus** as a data source
2. Create **panels** for CPU, memory, and network metrics
3. Set **thresholds & alerts**

---

## 🐘 4. ELK Stack – Centralized Logging

### 🔹 Components

| Component     | Purpose                                 |
| ------------- | --------------------------------------- |
| Elasticsearch | Stores and indexes logs                 |
| Logstash      | Collects, transforms, and forwards logs |
| Kibana        | Visualizes logs and creates dashboards  |

### 🔹 Installation Example (Single Node)

```bash
# Elasticsearch
sudo apt install elasticsearch
sudo systemctl start elasticsearch

# Logstash
sudo apt install logstash
sudo systemctl start logstash

# Kibana
sudo apt install kibana
sudo systemctl start kibana
```

---

## 🔹 5. Logstash Pipeline Example

**logstash.conf**

```conf
input {
  file {
    path => "/var/log/nginx/access.log"
    start_position => "beginning"
  }
}
filter {
  grok {
    match => { "message" => "%{COMBINEDAPACHELOG}" }
  }
}
output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "nginx-logs"
  }
  stdout { codec => rubydebug }
}
```

* Start Logstash:

```bash
sudo logstash -f logstash.conf
```

* Logs are stored in Elasticsearch and visualized via Kibana.

---

## ⚡ 6. Advanced Monitoring & Logging Concepts

### 🔹 Prometheus Alerting

**alert.rules.yml**

```yaml
groups:
- name: node_alerts
  rules:
  - alert: HighCPU
    expr: node_cpu_seconds_total > 80
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "High CPU usage detected"
```

* Integrate with **Alertmanager** to send notifications via Slack, email, or PagerDuty.

### 🔹 Grafana Advanced

* Multi-data source dashboards
* Annotations for deployments
* Alert notifications for metrics crossing thresholds

### 🔹 ELK Advanced

* Centralize logs from multiple servers using **beats** (Filebeat, Metricbeat)
* Implement **retention policies** and **index lifecycle management**
* Use Kibana to visualize trends and detect anomalies

---

## 🧩 7. Real-World DevOps Monitoring Example

**Scenario:** Node.js app deployed on Kubernetes

1. **Metrics:**

   * Node Exporter + Prometheus for node metrics
   * Application metrics exposed via `/metrics` endpoint

2. **Dashboards:**

   * Grafana dashboard for CPU, memory, and request latency

3. **Logs:**

   * Filebeat collects app logs → Logstash parses → Elasticsearch stores → Kibana visualizes

4. **Alerts:**

   * Prometheus Alertmanager triggers Slack notifications for CPU > 80% or app errors

✅ Full-stack monitoring and logging for proactive DevOps operations.

---

## 🏁 8. Hands-on Lab

**Task:** Monitor Linux server & Node.js app

1. Install **Node Exporter** and **Prometheus**
2. Configure Prometheus to scrape metrics
3. Install **Grafana** and create dashboard for CPU & memory
4. Deploy **ELK stack** and configure Logstash to collect Node.js app logs
5. Set up **alerts** for high CPU usage or log errors

---

## 🏁 Summary

| Level           | Topics Covered                                                                                |
| --------------- | --------------------------------------------------------------------------------------------- |
| 🟢 Beginner     | Install Prometheus, Node Exporter, Grafana, basic dashboards                                  |
| 🟡 Intermediate | ELK stack setup, log collection, visualization                                                |
| 🔴 Advanced     | Alerting with Prometheus, multi-node metrics, CI/CD integration, log aggregation and analysis |

> 💬 “Monitoring & Logging transform DevOps from reactive troubleshooting to **proactive operations**, enabling performance optimization and faster issue resolution.”

---
