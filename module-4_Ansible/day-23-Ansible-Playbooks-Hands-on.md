# day-23 Ansible Playbooks (Hands-on)

### 🔹 Hands-On Demo: Ansible Playbook for Apache2
### Step 1: Create your Ansible Project Directory
```sh
mkdir ansible-demo && cd ansible-demo
```
### Step 2: Create the Inventory File (hosts)
```sh
[web]
3.88.176.215 ansible_user=ubuntu ansible_ssh_private_key_file=~/ansible-work/rajiv.pem
```
 - Replace IP and key with your server details.

### Step 3: Create the Playbook (install_apache.yml)
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

    - name: Deploy custom index.html
      copy:
        dest: /var/www/html/index.html
        content: |
          <h1>Hello from Ansible Playbook </h1>
          <p>Apache2 installed and managed by Ansible.</p>
```

### Step 4: Run the Playbook
```sh
ansible-playbook -i hosts install_apache.yml
```

### 👉 What happens:
  - Updates apt cache
  - Installs latest Apache2
  - Starts & enables Apache2
  - Deploys a custom index.html

### Step 5: Verify
Open your browser:
```sh
http://3.88.176.215
```
You should see:
```sh
Hello from Ansible Playbook 
Apache2 installed and managed by Ansible.
```
