

#  What is Ansible?

Ansible is an open-source automation tool used for IT configuration management, application deployment, orchestration, and provisioning.

It helps DevOps engineers automate repetitive tasks like setting up servers, deploying applications, or configuring networks — all using simple, human-readable files called Playbooks (written in YAML).

🧠 Simple Definition

> Ansible automates infrastructure setup and application management — so you don’t have to manually log in to servers and configure them one by one.

It was developed by **Red Hat** and is **agentless** (doesn’t need any agent on target machines).
It communicates with remote servers using **SSH** (Linux) or **WinRM** (Windows).

---

## ⚙️ Why Do We Need Ansible?

Imagine you have **100 servers**, and you want to:

* Install Nginx on all of them
* Update configuration files
* Restart services

Doing this manually is slow and error-prone.
With **Ansible**, you just define **what you want** (desired state) in a YAML file, and it makes it happen automatically.

---

## 🏗️ Ansible Architecture

Here’s how Ansible works under the hood 👇

```
           ┌────────────────────────────────────────┐
           │          Ansible Control Node          │
           │  (where Ansible is installed)          │
           └────────────────────────────────────────┘
                         │
          SSH/WinRM      │
                         ▼
    ┌───────────────┬───────────────┬───────────────┐
    │ Managed Node  │ Managed Node  │ Managed Node  │
    │ (Server 1)    │ (Server 2)    │ (Server 3)    │
    └───────────────┴───────────────┴───────────────┘
```

### 🔹 Main Components:

1. **Control Node**

   * Machine where Ansible is installed.
   * Runs playbooks and connects to managed nodes.

2. **Managed Nodes (Targets)**

   * Systems being automated by Ansible.
   * No agent needed — just SSH access and Python installed.

3. **Inventory File**

   * List of target servers.
   * Example: `/etc/ansible/hosts`

   ```ini
   [webservers]
   web1.example.com
   web2.example.com

   [dbservers]
   db1.example.com
   ```

4. **Modules**

   * Reusable units of work in Ansible.
   * Examples: `yum`, `apt`, `copy`, `service`, `user`, etc.

5. **Playbooks (YAML files)**

   * Define the desired configuration and tasks.

6. **Tasks**

   * Single unit of action — like installing a package or starting a service.

7. **Roles**

   * Structured way to organize playbooks for large projects (like microservices).

---

## 🧾 How Ansible Works Step by Step

1. You write a **Playbook** (in YAML).
2. You define your **inventory file** (list of hosts).
3. You run the playbook using the `ansible-playbook` command.
4. Ansible connects via **SSH** to each host.
5. Executes **modules** and brings the system to the desired state.

---

## 📘 Example 1: Basic Ansible Ad-hoc Command

Install Nginx on remote servers:

```bash
ansible webservers -m apt -a "name=nginx state=present" -b
```

Explanation:

* `webservers` → group name from inventory
* `-m apt` → use the **apt module**
* `-a` → module arguments
* `-b` → run as sudo

---

## 📘 Example 2: Simple Playbook

Let’s create a file named **install_nginx.yml**:

```yaml
---
- name: Install and Start Nginx
  hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start Nginx Service
      service:
        name: nginx
        state: started
```

Run it using:

```bash
ansible-playbook install_nginx.yml
```

✅ This will:

1. Connect to all servers under `[webservers]`
2. Install Nginx
3. Start the service automatically

---

## 📘 Example 3: Using Variables

Define variables to make the playbook reusable:

```yaml
---
- name: Install Web Server
  hosts: webservers
  become: yes
  vars:
    pkg_name: nginx
  tasks:
    - name: Install Package
      apt:
        name: "{{ pkg_name }}"
        state: present

    - name: Start Service
      service:
        name: "{{ pkg_name }}"
        state: started
```

---

## 📘 Example 4: Using Templates (Jinja2)

Create a **template file**: `index.html.j2`

```html
<h1>Welcome to {{ ansible_hostname }}</h1>
```

Playbook to copy the template:

```yaml
---
- name: Deploy Template
  hosts: webservers
  become: yes
  tasks:
    - name: Copy index.html
      template:
        src: index.html.j2
        dest: /var/www/html/index.html
```

Each server will get its own hostname in the HTML file.

---

## 🧩 Example 5: Roles Structure

When your setup grows big, use **roles**:

```
roles/
 └── webserver/
     ├── tasks/main.yml
     ├── templates/index.html.j2
     ├── vars/main.yml
     └── handlers/main.yml
```

Then in your playbook:

```yaml
- hosts: webservers
  roles:
    - webserver
```

---

## 🧮 Handlers Example

Handlers are triggered only when a task changes something.

```yaml
---
- name: Example with Handler
  hosts: webservers
  become: yes
  tasks:
    - name: Update Nginx Config
      copy:
        src: nginx.conf
        dest: /etc/nginx/nginx.conf
      notify: Restart Nginx

  handlers:
    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

---

## 🧰 Common Ansible Commands

| Command                         | Description                       |
| ------------------------------- | --------------------------------- |
| `ansible all -m ping`           | Check connectivity to all servers |
| `ansible all -a "uptime"`       | Run a command on all servers      |
| `ansible-playbook playbook.yml` | Run a playbook                    |
| `ansible-galaxy init role_name` | Create a new role structure       |
| `ansible-inventory --list`      | View inventory details            |

---

## 🔐 Ansible Vault (for Secrets)

Encrypt sensitive data like passwords or tokens.

```bash
ansible-vault create secrets.yml
ansible-vault encrypt existing_file.yml
ansible-playbook site.yml --ask-vault-pass
```

---

## 🧩 Integration in DevOps

Ansible fits into the DevOps lifecycle:

* **CI/CD pipelines** for deployment automation
* **Provisioning** servers in AWS, Azure, GCP
* **Configuration management** for consistency
* **Security automation** with compliance checks

Example: Automating deployment in a Jenkins pipeline using an Ansible playbook.

---

## ⚡ Advantages

✅ Agentless (no extra software on targets)
✅ Simple YAML syntax
✅ Reusable and scalable (roles, templates)
✅ Cross-platform (Linux, Windows, cloud)
✅ Strong community support

---

## 🚀 Real-World Example

Deploy a web app on AWS EC2 instances:

1. Use Terraform to create EC2 instances.
2. Use Ansible to:

   * Install Docker
   * Pull your app image
   * Start containers automatically.

Example task:

```yaml
- name: Run Docker Container
  hosts: app_servers
  become: yes
  tasks:
    - name: Pull Image
      docker_image:
        name: myapp
        tag: latest
        source: pull

    - name: Run Container
      docker_container:
        name: myapp_container
        image: myapp:latest
        state: started
        ports:
          - "80:80"
```

---

## 🏁 Summary

| Concept        | Description                                 |
| -------------- | ------------------------------------------- |
| **Ansible**    | Automation & configuration management tool  |
| **Language**   | YAML (declarative)                          |
| **Connection** | SSH / WinRM                                 |
| **Core File**  | Inventory + Playbook                        |
| **Modules**    | Predefined tasks (apt, service, copy, etc.) |
| **Roles**      | Organize big projects                       |
| **Vault**      | Encrypt secrets                             |

---
