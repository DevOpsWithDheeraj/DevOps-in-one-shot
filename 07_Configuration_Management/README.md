# ⚒️ Configuration Management 

## 📘 Introduction

**Configuration Management (CM)** is the practice of **automating setup, configuration, and management of servers and applications**.  
It ensures **consistency, repeatability, and version control** of infrastructure across environments.

**Popular Tools:**
- **Ansible** – Agentless, YAML-based
- **Puppet** – Agent/master architecture
- **Chef** – Ruby-based, declarative recipes

---

## 🧩 1. Why Configuration Management in DevOps?

- Automate server provisioning & software installation  
- Maintain consistency across environments (Dev, Staging, Prod)  
- Reduce human errors & manual intervention  
- Enable **Infrastructure as Code** (IaC) practices  

---

## 🐧 2. Ansible Basics

### 🔹 What is Ansible?
- Open-source, agentless CM tool  
- Uses **YAML-based Playbooks** to define tasks  
- Connects via **SSH** to target nodes  

### 🔹 Installation
```bash
sudo apt update
sudo apt install ansible -y
ansible --version
````

### 🔹 Inventory File

```ini
[webservers]
192.168.1.10
192.168.1.11

[dbservers]
192.168.1.20
```

### 🔹 Simple Ad-Hoc Command

```bash
ansible webservers -m ping
ansible dbservers -m shell -a "uptime"
```

---

## 🧰 3. Ansible Playbooks (Declarative Approach)

**Example: Install Nginx on Web Servers**

```yaml
---
- name: Install Nginx on Web Servers
  hosts: webservers
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Ensure Nginx is running
      service:
        name: nginx
        state: started
        enabled: yes
```

**Run Playbook:**

```bash
ansible-playbook install_nginx.yml
```

✅ This automatically provisions web servers with Nginx.

---

## 🐙 4. Puppet Basics

* Uses **Master-Agent architecture**
* Declarative language called **Puppet DSL**
* **Manifests** define desired state

**Example: Install Nginx on Puppet Agent**

```puppet
package { 'nginx':
  ensure => installed,
}

service { 'nginx':
  ensure => running,
  enable => true,
}
```

**Workflow:**

1. Puppet Master holds manifests
2. Agents pull configuration and apply

---

## 🍴 5. Chef Basics

* Chef uses **recipes & cookbooks** in **Ruby DSL**
* Manages nodes via **Chef Server or Chef Solo**

**Example Recipe to Install Nginx**

```ruby
package 'nginx' do
  action :install
end

service 'nginx' do
  action [:enable, :start]
end
```

* Cookbooks can include multiple recipes, templates, and files
* Integration with CI/CD pipelines possible

---

## ⚡ 6. Advanced Configuration Management

### 🔹 Roles & Idempotency

* **Roles** in Ansible organize tasks, handlers, and variables
* Idempotent commands ensure repeated runs **don’t break state**

### 🔹 Dynamic Inventory

* Pull inventory from **cloud providers (AWS, GCP, Azure)**
* Example for AWS EC2:

```yaml
plugin: aws_ec2
regions:
  - us-east-1
```

### 🔹 Secret Management

* Use **Ansible Vault** to encrypt sensitive data:

```bash
ansible-vault create secrets.yml
ansible-playbook deploy.yml --ask-vault-pass
```

### 🔹 CI/CD Integration

* Ansible Playbooks run in **Jenkins pipelines**
* Automated deployments after build stage

**Example Jenkins Stage**

```groovy
stage('Configure Servers') {
  steps {
    sh 'ansible-playbook -i inventory install_nginx.yml'
  }
}
```

---

## 🧩 7. Real-World DevOps CM Example

**Scenario:** Automated web application deployment

1. **Provision Servers** – AWS EC2 or on-prem
2. **Install Prerequisites** – Nginx, Docker, Node.js via Ansible
3. **Deploy Application** – Copy code, start services
4. **Configure Monitoring** – Install Prometheus node exporter via CM
5. **Update Configs Safely** – Use ConfigMaps/Secrets for sensitive info

✅ Fully automated, repeatable deployment across multiple servers.

---

## 🏁 8. Hands-on Lab

**Task:** Deploy Nginx + Node.js app on 2 servers

1. Create inventory file:

```ini
[webservers]
192.168.1.10
192.168.1.11
```

2. Playbook to install Nginx & Node.js:

```yaml
---
- name: Setup Web Servers
  hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Install Node.js
      apt:
        name: nodejs
        state: present

    - name: Start Nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

3. Run playbook:

```bash
ansible-playbook setup_webservers.yml
```

---

## 🏁 Summary

| Level           | Topics Covered                                                                           |
| --------------- | ---------------------------------------------------------------------------------------- |
| 🟢 Beginner     | Ad-hoc commands, inventory, basic Playbooks                                              |
| 🟡 Intermediate | Roles, dynamic inventory, secrets, CI/CD integration                                     |
| 🔴 Advanced     | Multi-server orchestration, idempotency, production-grade deployments, Puppet/Chef usage |

> 💬 “Configuration Management ensures **consistency, reliability, and automation**, the backbone of any DevOps pipeline.”

---

