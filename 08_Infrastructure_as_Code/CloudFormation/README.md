
# ☁️ What is AWS CloudFormation?

**AWS CloudFormation** is an **Infrastructure as Code (IaC)** service provided by **Amazon Web Services (AWS)**.

It allows you to **define, provision, and manage AWS infrastructure** using **code** written in **YAML** or **JSON** files called **CloudFormation templates**.

👉 In short:
Instead of clicking around the AWS Console to create resources like EC2, S3, or VPC —
you **describe everything in a template**, and **CloudFormation automatically builds it for you**.

---

## 🧠 Why Use CloudFormation?

| Feature                   | Description                                                               |
| ------------------------- | ------------------------------------------------------------------------- |
| **Automation**            | Automatically provisions AWS resources.                                   |
| **Consistency**           | Ensures identical environments every time (e.g., Dev, QA, Prod).          |
| **Dependency Management** | Automatically handles resource creation order (e.g., VPC → Subnet → EC2). |
| **Rollback**              | Automatically rolls back if deployment fails.                             |
| **Integration**           | Deeply integrated with AWS ecosystem.                                     |
| **Version Control**       | Templates can be stored in Git for tracking changes.                      |

---

## 🏗️ CloudFormation Architecture

### 1. **Template (YAML or JSON)**

Describes what resources you want (like EC2, S3, IAM role, etc.)

### 2. **Stack**

A stack is a **collection of AWS resources** that are created, updated, or deleted together using a single template.

### 3. **Change Set**

Before applying a change, CloudFormation can show what will change (like `terraform plan`).

---

## 🧱 CloudFormation Template Structure

A CloudFormation template has **five main sections**:

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Create an EC2 instance"

Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    Description: EC2 Instance type

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: ami-0c55b159cbfafe1f0
      InstanceType: !Ref InstanceType
      KeyName: my-key
      SecurityGroups: 
        - !Ref MySecurityGroup

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Allow HTTP traffic
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

Outputs:
  InstanceID:
    Description: "Instance ID"
    Value: !Ref MyEC2Instance
```

---

## 🔍 Key Template Sections Explained

| Section                      | Description                                                |
| ---------------------------- | ---------------------------------------------------------- |
| **AWSTemplateFormatVersion** | (Optional) Template version (latest: 2010-09-09).          |
| **Description**              | Short info about the template.                             |
| **Parameters**               | Input values to customize stack behavior.                  |
| **Resources**                | Actual AWS resources to create (mandatory section).        |
| **Outputs**                  | Information displayed after stack creation (e.g., IP, ID). |
| **Mappings**                 | Key-value pairs for region-specific or static data.        |
| **Conditions**               | Logic to include/exclude resources (e.g., Prod vs Dev).    |
| **Metadata**                 | Extra data for documentation or tools.                     |

---

## ⚙️ CloudFormation Workflow

1. **Write a Template** – in YAML/JSON (like `template.yaml`).
2. **Upload Template** – via AWS Console, CLI, or S3 bucket.
3. **Create a Stack** – CloudFormation provisions resources automatically.
4. **Monitor Progress** – View events/logs in the CloudFormation console.
5. **Update Stack** – Modify and re-apply template (CloudFormation updates only what changed).
6. **Delete Stack** – Deletes all resources automatically.

---

## 🧪 Example: Create EC2 Instance using CloudFormation

**Template: `ec2_instance.yaml`**

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Description: "Example CloudFormation Template - Create EC2 Instance"

Parameters:
  KeyName:
    Description: "Name of an existing EC2 KeyPair"
    Type: AWS::EC2::KeyPair::KeyName
  InstanceType:
    Description: "EC2 Instance Type"
    Type: String
    Default: t2.micro
  AmiId:
    Description: "AMI ID for EC2"
    Type: AWS::EC2::Image::Id
    Default: ami-0c55b159cbfafe1f0

Resources:
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: "Allow SSH and HTTP access"
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      KeyName: !Ref KeyName
      ImageId: !Ref AmiId
      SecurityGroupIds:
        - !Ref MySecurityGroup
      Tags:
        - Key: Name
          Value: MyCFNInstance

Outputs:
  InstancePublicIP:
    Description: "Public IP of the EC2 instance"
    Value: !GetAtt MyEC2Instance.PublicIp
```

---

### 🧾 Deploy this Template

**Option 1: Using AWS Management Console**

1. Go to **AWS CloudFormation → Create Stack → With new resources (standard)**.
2. Upload the `ec2_instance.yaml` file.
3. Enter parameters (Key Pair, etc.).
4. Click **Create Stack**.

**Option 2: Using AWS CLI**

```bash
aws cloudformation create-stack \
  --stack-name my-ec2-stack \
  --template-body file://ec2_instance.yaml \
  --parameters ParameterKey=KeyName,ParameterValue=mykey
```

---

## 📦 CloudFormation Outputs Example

After the stack is created, you’ll see outputs like:

```
InstancePublicIP = 18.223.45.11
```

This is the **public IP** of your new EC2 instance.

---

## 🔁 Updating and Deleting a Stack

* **Update Stack:**

  ```bash
  aws cloudformation update-stack \
    --stack-name my-ec2-stack \
    --template-body file://updated_template.yaml
  ```

* **Delete Stack:**

  ```bash
  aws cloudformation delete-stack --stack-name my-ec2-stack
  ```

---

## 🧩 Key CloudFormation Features

| Feature                    | Description                                                       |
| -------------------------- | ----------------------------------------------------------------- |
| **Nested Stacks**          | Reuse smaller templates inside larger ones.                       |
| **Cross-Stack References** | Share outputs between stacks.                                     |
| **Change Sets**            | Preview changes before applying them.                             |
| **Drift Detection**        | Detect if resources were changed manually outside CloudFormation. |
| **Stack Policies**         | Control which resources can be updated.                           |
| **StackSets**              | Deploy a single template across multiple AWS accounts or regions. |

---

## ⚖️ CloudFormation vs Terraform

| Feature               | **CloudFormation**            | **Terraform**                       |
| --------------------- | ----------------------------- | ----------------------------------- |
| **Provider Support**  | Only AWS                      | Multi-Cloud (AWS, Azure, GCP, etc.) |
| **Language**          | YAML / JSON                   | HCL                                 |
| **State Management**  | Managed by AWS                | Local or remote (S3, etc.)          |
| **Drift Detection**   | Yes                           | Yes (with `terraform refresh`)      |
| **Change Preview**    | Change Sets                   | `terraform plan`                    |
| **Execution**         | Managed (via AWS Console/CLI) | CLI/Automation tools                |
| **Community Modules** | AWS SAM / CFN registry        | Terraform Registry                  |

---

## ✅ Summary

| Concept             | Description                   |
| ------------------- | ----------------------------- |
| **Service**         | AWS CloudFormation            |
| **Type**            | Infrastructure as Code (IaC)  |
| **Template Format** | YAML or JSON                  |
| **Main Entity**     | Stack                         |
| **Automation**      | Fully managed by AWS          |
| **Rollback**        | Automatic rollback on failure |
| **Integration**     | Deep with AWS ecosystem       |

---

## 🚀 Real-World Use Case (DevOps Context)

You can integrate CloudFormation with:

* **CodePipeline / CodeBuild** → for CI/CD automation.
* **Ansible / Jenkins** → to deploy infrastructure automatically.
* **CloudFormation StackSets** → to deploy infra across multiple AWS accounts (useful in Infosys-level enterprise setups).

---
