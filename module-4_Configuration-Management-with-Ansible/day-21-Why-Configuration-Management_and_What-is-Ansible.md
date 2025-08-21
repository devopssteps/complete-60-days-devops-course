### Day-21: Why Configuration Management? What is Ansible?

### ✅ Explanation + Hands-On: "Why Configuration Management? What is Ansible?"

### 🔹 Why Configuration Management?
### 🔧 Problem Without It:
Imagine setting up 50 servers manually:
 - Install packages
 - Configure users
 - Start services
 - Apply patches
🛑 Manual setup = error-prone, inconsistent, hard to scale

### ✅ What Is Configuration Management?
Configuration Management is the practice of automating the setup, configuration, and management of systems — to ensure consistency, repeatability, and scalability.
Popular tools:
 - Ansible
 - Puppet
 - Chef
 - SaltStack

### 🧠 What is Ansible?
 - Ansible is an open-source automation tool by Red Hat.
 - Used for: provisioning, configuration management, application deployment
 - Agentless (uses SSH)
 - Written in YAML (Playbooks)

## 🛠️ Hands-On Example: Ansible Basics
### ✅ Step 1: Install Ansible (on Ubuntu)
```sh
sudo apt update
sudo apt install ansible -y
ansible --version
```

### ✅ Step 2: Create an Inventory File
```sh
# inventory.ini
[webservers]
192.168.1.101
192.168.1.102
```

### ✅ Step 3: Test Connectivity (SSH key-based access assumed)
```sh
ansible  -i hosts all -m ping
OR
ansible -i inventory.ini all -m ping
```
You’ll get:
```sh
192.168.1.101 | SUCCESS => {"changed": false, "ping": "pong"}
```

### ✅ Step 4: Create a Playbook (Install Apache)
```sh
---
- name: Install and Start Apache2 on Ubuntu
  hosts: web
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
        cache_valid_time: 3600

    - name: Install Apache2 (latest version)
      apt:
        name: apache2
        state: latest

    - name: Ensure Apache2 is running and enabled
      service:
        name: apache2
        state: started
        enabled: yes
```

### ✅ Step 5: Run the Playbook
```sh
ansible-playbook -i inventory.ini install_apache.yml
```

### ✅ Benefits of Using Ansible
| Feature         | Why It Matters                       |
| --------------- | ------------------------------------ |
| Agentless       | No need to install anything on nodes |
| YAML Playbooks  | Easy to read and write               |
| Idempotent      | Safe to re-run multiple times        |
| Scalable        | Automate 1 or 1000 servers           |
| Fast onboarding | Simple learning curve                |
