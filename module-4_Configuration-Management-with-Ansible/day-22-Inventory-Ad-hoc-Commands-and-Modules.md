### day-22 Inventory, Ad-hoc Commands, Modules         
Inventory, Ad-hoc Commands, Modules" is key to understanding how Ansible works in practice.

Below is a complete breakdown:
  - ✅ Explanation
  - 🛠 Hands-on examples

## ✅ What are Inventory, Ad-hoc Commands, and Modules in Ansible?

### 📁 1. Inventory in Ansible
 - A file that lists the hosts (servers) Ansible will manage.
 - Format: INI, YAML, or dynamic inventory scripts.

Example: inventory.ini
```sh
[webservers]
192.168.1.101
192.168.1.102

[dbservers]
192.168.1.103
```

### 🧪 2. Ad-Hoc Commands
 - One-liner commands to perform quick tasks on remote hosts.
 - Great for testing, debugging, or small changes.
Example: Ping all servers
```sh
ansible -i hosts all -m ping
```
Install package on webservers
```sh
ansible -i hosts webservers -m apt -a "name=apache2 state=present" -b
```

### 🔧 3. Modules in Ansible
 - Modules are tools that Ansible uses to perform actions.
 - Over 400+ built-in modules (e.g., ping, apt, copy, service, user).

Common Modules:
| Module        | Purpose             |
| ------------- | ------------------- |
| `ping`        | Check connectivity  |
| `apt` / `yum` | Install packages    |
| `service`     | Start/stop services |
| `copy`        | Copy files          |
| `command`     | Run shell commands  |
| `user`        | Manage users        |

## 🛠️ Hands-On Example
### 🔧 Step 1: Create Inventory File
inventory.ini
```sh
[webservers]
192.168.1.101 ansible_user=ubuntu
192.168.1.102 ansible_user=ubuntu
```

### 🔧 Step 2: Ad-Hoc Commands with Modules
✅ Ping all servers
```sh
ansible -i inventory.ini all -m ping
```
✅ Install Apache on webservers
```sh
ansible -i inventory.ini webservers -m apt -a "name=apache2 state=present update_cache=true" -b
```
✅ Start Apache service
```sh
ansible -i inventory.ini webservers -m service -a "name=apache2 state=started" -b
```
✅ Copy an index.html
```sh
ansible -i inventory.ini webservers -m copy -a "src=./index.html dest=/var/www/html/index.html" -b
```
✅ Add a new user
```sh
ansible -i inventory.ini all -m user -a "name=devops state=present" -b
```
