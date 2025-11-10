# 🏗️ Infrastructure as Code (IaC) with Terraform & CloudFormation 

## 📘 Introduction

**Infrastructure as Code (IaC)** allows DevOps teams to **provision and manage infrastructure using code**, rather than manual processes.  
It ensures **repeatability, version control, and automation** across environments.

**Popular IaC Tools:**
- **Terraform** – cloud-agnostic, declarative HCL language  
- **AWS CloudFormation** – AWS-native IaC, JSON/YAML templates  

---

## 🧩 1. Benefits of IaC

- Automated provisioning of servers, networks, and services  
- Version control of infrastructure (Git)  
- Consistency across Dev, Staging, and Production  
- Faster deployment and rollback  

---

## 🐧 2. Terraform Basics

### 🔹 What is Terraform?
- Open-source tool for **declarative infrastructure provisioning**  
- Supports **multi-cloud**: AWS, Azure, GCP, Kubernetes  
- Uses **HCL (HashiCorp Configuration Language)**

### 🔹 Installation
```bash
sudo apt update
sudo apt install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
sudo apt-add-repository "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt update
sudo apt install terraform
terraform version
````

---

## 🔹 3. Terraform Workflow

1. **Write configuration** → `.tf` files
2. **Initialize Terraform** → `terraform init`
3. **Plan changes** → `terraform plan`
4. **Apply changes** → `terraform apply`
5. **Destroy resources** → `terraform destroy`

---

## 🔹 4. Basic Terraform Example – AWS EC2

**main.tf**

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"
  tags = {
    Name = "Terraform-EC2-Web"
  }
}
```

**Commands:**

```bash
terraform init
terraform plan
terraform apply -auto-approve
terraform destroy -auto-approve
```

✅ Creates an EC2 instance in AWS automatically.

---

## ☁️ 5. Terraform Advanced Concepts

### 🔹 Variables & Outputs

```hcl
variable "instance_type" { default = "t2.micro" }

resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = var.instance_type
}

output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

### 🔹 Modules

* Reusable Terraform code blocks

```hcl
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  name   = "devops-vpc"
  cidr   = "10.0.0.0/16"
}
```

### 🔹 Remote State

* Store state in **S3 bucket** for team collaboration

```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "global/s3/terraform.tfstate"
    region = "us-east-1"
  }
}
```

---

## 🐦 6. AWS CloudFormation Basics

* AWS-native IaC tool
* Declarative JSON/YAML templates define AWS resources
* Stack: Collection of resources managed together

### 🔹 Simple Example – EC2 Instance

**ec2.yml**

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c02fb55956c7d316
      Tags:
        - Key: Name
          Value: CloudFormation-EC2
```

**Commands:**

```bash
aws cloudformation create-stack --stack-name my-stack --template-body file://ec2.yml
aws cloudformation describe-stacks --stack-name my-stack
aws cloudformation delete-stack --stack-name my-stack
```

---

## ⚡ 7. Advanced CloudFormation Concepts

1. **Parameters** – dynamic input values for templates
2. **Outputs** – export data between stacks
3. **Conditions** – create resources conditionally
4. **Nested Stacks** – modular templates for large infra
5. **Change Sets** – preview infrastructure updates before applying

---

## 🧩 8. Real-World DevOps IaC Example

**Scenario:** Deploy Web App on AWS with Terraform + CloudFormation

1. **Terraform** provisions:

   * VPC, subnets, security groups
   * EC2 instances for backend
   * RDS database

2. **CloudFormation** provisions:

   * Application load balancer
   * Auto-scaling groups
   * S3 bucket for static content

3. **CI/CD Integration:**

   * Jenkins pipeline triggers Terraform apply after code merge
   * Application is deployed automatically to provisioned infrastructure

---

## 🏁 9. Hands-on Lab

**Task:** Deploy EC2 + S3 using Terraform

1. Create `main.tf`:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "app_bucket" {
  bucket = "devops-iac-bucket"
  acl    = "private"
}

resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"
}
```

2. Commands:

```bash
terraform init
terraform plan
terraform apply -auto-approve
terraform destroy -auto-approve
```

✅ Infrastructure is fully automated and version-controlled.

---

## 🏁 Summary

| Level           | Topics Covered                                                                 |
| --------------- | ------------------------------------------------------------------------------ |
| 🟢 Beginner     | Terraform & CloudFormation installation, basic EC2/S3 deployment               |
| 🟡 Intermediate | Variables, outputs, modules, parameters, stacks                                |
| 🔴 Advanced     | Remote state, nested stacks, CI/CD integration, production-grade IaC pipelines |

> 💬 “IaC transforms infrastructure into **code, versioned, testable, and repeatable**, making DevOps truly automated and scalable.”

---
