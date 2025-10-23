
# ☁️ Cloud Computing 

## 📘 Introduction

**Cloud Computing** provides on-demand delivery of **computing resources** (servers, storage, databases, networking, software) over the internet.  

**Key Benefits:**
- **Scalability:** Scale resources up/down dynamically  
- **Cost Efficiency:** Pay-as-you-go model  
- **High Availability:** Distributed architecture with redundancy  
- **Global Accessibility:** Access from anywhere  

**Popular Cloud Providers:**
- **AWS (Amazon Web Services)**  
- **Azure**  
- **Google Cloud Platform (GCP)**  

---

## 🧩 1. Cloud Computing Models

| Model | Description | Example |
|-------|-------------|---------|
| **IaaS** (Infrastructure as a Service) | Provides virtualized computing resources | AWS EC2, Google Compute Engine |
| **PaaS** (Platform as a Service) | Provides platform to deploy applications | AWS Elastic Beanstalk, Azure App Service |
| **SaaS** (Software as a Service) | Provides software over the internet | Gmail, Salesforce, Slack |
| **FaaS** (Function as a Service) | Serverless compute for code execution | AWS Lambda, Azure Functions |

---

## 🐧 2. Cloud Deployment Models

| Model | Description |
|-------|-------------|
| **Public Cloud** | Managed by provider; shared infrastructure |
| **Private Cloud** | Managed internally; dedicated infrastructure |
| **Hybrid Cloud** | Mix of public + private cloud |
| **Multi-Cloud** | Multiple cloud providers for redundancy & flexibility |

---

## 🧰 3. AWS Basics (IaaS & PaaS)

### 🔹 AWS Core Services
| Service | Type | Description |
|---------|------|-------------|
| EC2 | IaaS | Virtual servers |
| S3 | Storage | Object storage for files & backups |
| RDS | DBaaS | Managed relational databases |
| Lambda | FaaS | Serverless compute |
| VPC | Networking | Virtual private cloud |
| ELB | Load Balancer | Distributes traffic |
| CloudWatch | Monitoring | Logs & metrics |

---

## 🔹 4. Example: Deploy Web App in AWS (Beginner)

**Scenario:** Deploy a Node.js app

1. **Create EC2 Instance**
```bash
aws ec2 run-instances --image-id ami-0c02fb55956c7d316 --count 1 --instance-type t2.micro --key-name MyKey --security-group-ids sg-123456 --subnet-id subnet-123456
````

2. **Install Node.js**

```bash
ssh -i MyKey.pem ec2-user@<EC2_IP>
sudo yum install -y nodejs npm
```

3. **Deploy App**

```bash
git clone https://github.com/DevOpsWithDheeraj/node-app.git
cd node-app
npm install
node app.js
```

4. **Access via Public IP**

* Open browser → `http://<EC2_IP>:3000`

✅ Basic deployment with AWS EC2.

---

## ⚡ 5. Advanced Cloud Computing Concepts

### 🔹 Auto Scaling & Load Balancing

* **Auto Scaling Group (ASG):** Automatically adjusts EC2 instances
* **Elastic Load Balancer (ELB):** Distributes traffic across instances

### 🔹 Serverless Architecture

* Run code without managing servers
* Example: AWS Lambda with API Gateway triggers

```python
# AWS Lambda example (Python)
def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Hello from Lambda!'
    }
```

### 🔹 Infrastructure as Code in Cloud

* Use **Terraform** or **CloudFormation** to provision cloud resources
* Example: EC2 + S3 deployment via Terraform

```hcl
provider "aws" { region = "us-east-1" }

resource "aws_s3_bucket" "app_bucket" { bucket = "devops-app-bucket" }

resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"
}
```

### 🔹 Security & Networking

* **IAM Roles & Policies:** Manage permissions
* **VPC & Subnets:** Isolate networks
* **Security Groups:** Firewall rules for instances

---

## 🧩 6. Real-World DevOps Cloud Example

**Scenario:** Deploy CI/CD pipeline in AWS

1. **Code Repository:** GitHub
2. **CI/CD Tool:** Jenkins / AWS CodePipeline
3. **Compute:** EC2 instances auto-scaled
4. **Serverless Functions:** AWS Lambda for event-based tasks
5. **Storage:** S3 for artifacts & logs
6. **Monitoring:** CloudWatch + Grafana dashboards

✅ Fully automated deployment pipeline in cloud with **scalable, highly available architecture**.

---

## 🏁 7. Hands-on Lab

**Task:** Deploy a multi-tier web app in AWS

1. Create **VPC**, subnets, and security groups
2. Deploy **EC2 instances** for backend and frontend
3. Configure **RDS** for database
4. Attach **ELB** for load balancing
5. Enable **Auto Scaling** for backend
6. Integrate **CloudWatch** for logs and metrics

---

## 🏁 Summary

| Level           | Topics Covered                                                             |
| --------------- | -------------------------------------------------------------------------- |
| 🟢 Beginner     | Cloud models (IaaS, PaaS, SaaS), AWS core services, EC2 deployment         |
| 🟡 Intermediate | Auto-scaling, ELB, serverless functions, IAM, VPC networking               |
| 🔴 Advanced     | CI/CD pipelines, IaC deployment, hybrid/multi-cloud strategies, monitoring |

> 💬 “Cloud computing enables DevOps teams to **provision, scale, and manage infrastructure dynamically**, making software delivery faster, reliable, and globally accessible.”

---
