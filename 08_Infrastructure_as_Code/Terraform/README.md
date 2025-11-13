
# 🧩 What is Terraform?

**Terraform** is an **open-source Infrastructure as Code (IaC) tool** developed by **HashiCorp**.
It allows you to **automate the creation, modification, and destruction of infrastructure** — across multiple cloud providers like AWS, Azure, Google Cloud, etc.

In simple terms:
👉 Instead of manually creating servers, databases, or networks in AWS Console —
you can **write code** (in `.tf` files) and **let Terraform do it automatically**.

---

## 🧠 Why Do We Use Terraform?

| Feature                 | Description                                                     |
| ----------------------- | --------------------------------------------------------------- |
| **Automation**          | Automates provisioning of infrastructure.                       |
| **Consistency**         | Ensures the same configuration is applied every time.           |
| **Multi-Cloud**         | Works with AWS, Azure, GCP, VMware, etc.                        |
| **Version Control**     | Configurations are stored as code (e.g., in Git).               |
| **Dependency Handling** | Terraform automatically manages dependencies between resources. |

---

## 🏗️ Terraform Architecture

### 1. **Configuration Files (.tf)**

You define infrastructure using **HCL (HashiCorp Configuration Language)** in `.tf` files.

### 2. **Terraform Core**

It takes your configuration, interacts with providers, and creates the **execution plan**.

### 3. **Providers**

These are plugins that allow Terraform to interact with cloud platforms (like AWS, Azure, etc.).

> Example: `provider "aws"` is used to connect to AWS.

### 4. **State File (`terraform.tfstate`)**

Terraform keeps track of your deployed infrastructure in this file — so it knows what exists and what needs to change.

---

## ⚙️ Terraform Workflow (Lifecycle)

The **Terraform workflow** usually has **5 main steps**:

1. **Write** — Define infrastructure in `.tf` files.
2. **Initialize** — Run `terraform init` to download the required provider plugins.
3. **Plan** — Run `terraform plan` to see what Terraform will create, modify, or destroy.
4. **Apply** — Run `terraform apply` to actually create or update infrastructure.
5. **Destroy** — Run `terraform destroy` to delete infrastructure.

---

## 📄 Example: Create an AWS EC2 Instance

### 🗂 Step 1: Create Configuration File — `main.tf`

```hcl
# Specify provider
provider "aws" {
  region = "us-east-1"
}

# Create a Key Pair (optional)
resource "aws_key_pair" "mykey" {
  key_name   = "mykey"
  public_key = file("~/.ssh/id_rsa.pub")
}

# Create a Security Group
resource "aws_security_group" "web_sg" {
  name        = "web-sg"
  description = "Allow inbound HTTP traffic"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# Create an EC2 Instance
resource "aws_instance" "web_server" {
  ami           = "ami-0c55b159cbfafe1f0" # Amazon Linux 2 AMI
  instance_type = "t2.micro"
  key_name      = aws_key_pair.mykey.key_name
  security_groups = [aws_security_group.web_sg.name]

  tags = {
    Name = "MyWebServer"
  }
}
```

---

### 🧪 Step 2: Initialize Terraform

```bash
terraform init
```

This downloads the AWS provider plugin.

---

### 🔍 Step 3: Preview the Changes

```bash
terraform plan
```

Shows what resources Terraform will create.

---

### 🚀 Step 4: Apply the Configuration

```bash
terraform apply
```

Terraform asks for confirmation → type **`yes`** → your EC2 instance will be created in AWS.

---

### 🧹 Step 5: Destroy the Infrastructure (when not needed)

```bash
terraform destroy
```

This deletes all resources created by Terraform.

---

## 📦 Terraform Important Files

| File                | Description                                 |
| ------------------- | ------------------------------------------- |
| `main.tf`           | Main configuration file.                    |
| `variables.tf`      | Defines input variables.                    |
| `outputs.tf`        | Defines output values (like public IPs).    |
| `terraform.tfstate` | Stores the current state of infrastructure. |
| `terraform.tfvars`  | Stores variable values.                     |

---

## 💡 Example with Variables

`variables.tf`:

```hcl
variable "region" {
  default = "us-east-1"
}
variable "instance_type" {
  default = "t2.micro"
}
```

`main.tf`:

```hcl
provider "aws" {
  region = var.region
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type

  tags = {
    Name = "ExampleInstance"
  }
}
```

---

## 🧱 Terraform Key Concepts

| Concept      | Description                                               |
| ------------ | --------------------------------------------------------- |
| **Provider** | Plugin to manage specific platforms (AWS, Azure, etc.)    |
| **Resource** | Basic building block (like EC2 instance, VPC, etc.)       |
| **Variable** | Dynamic input for configurations                          |
| **Output**   | Data shown after applying configuration                   |
| **State**    | Snapshot of your real infrastructure                      |
| **Module**   | Reusable group of Terraform resources                     |
| **Backend**  | Where the state file is stored (local or remote, like S3) |

---

## 🧰 Common Terraform Commands

| Command                | Description                     |
| ---------------------- | ------------------------------- |
| `terraform init`       | Initialize Terraform            |
| `terraform validate`   | Validate syntax of config files |
| `terraform plan`       | Preview the changes             |
| `terraform apply`      | Apply changes                   |
| `terraform show`       | Show current resources          |
| `terraform destroy`    | Delete resources                |
| `terraform output`     | Display output values           |
| `terraform fmt`        | Format code                     |
| `terraform state list` | List managed resources          |

---

## 🔄 Real-World Example in DevOps Pipeline

In DevOps, Terraform is often integrated with:

* **GitHub Actions / Jenkins** for CI/CD
* **AWS S3 + DynamoDB** for remote state management
* **Ansible** or **Chef** for post-deployment configuration

**Example flow:**

1. Developer pushes code to GitHub.
2. Jenkins triggers a Terraform job.
3. Terraform provisions AWS infrastructure.
4. Ansible configures the deployed servers.

---

## ✅ Summary

| Aspect          | Terraform                              |
| --------------- | -------------------------------------- |
| Type            | Infrastructure as Code (IaC)           |
| Language        | HCL (HashiCorp Configuration Language) |
| Purpose         | Automate infra provisioning            |
| Multi-Cloud     | Yes                                    |
| Maintains State | Yes                                    |
| Example Use     | Create EC2, VPC, S3, etc.              |

---
