# day 27 :project-Deploy-Nginx-Simple-Web-App-on-AWS-EC2-using-Ansible.md

# 👉 Deploy Nginx + Simple Web App on AWS EC2 using Ansible

### 📌 Hands-On Project Demo

### 1. Launch an EC2 Instance

On your Jenkins/PC (control node), launch an Ubuntu EC2 instance (target node).
<br>
Make sure you:
 - Assign a security group that allows SSH (22) + HTTP (80).
 - Attach your SSH key pair.
Let’s say the instance IP is: 54.210.xx.xx

### 
### 2. Create Ansible Inventory
hosts
```sh
[web]
54.210.xx.xx ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/my-aws-key.pem
```

### 3. Write Ansible Playbook
install_nginx.yml
```sh
---
- name: Install and configure Nginx + Web App
  hosts: web
  become: yes

  tasks:
    - name: Update apt packages
      apt:
        update_cache: yes

    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start and enable Nginx
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Deploy custom index.html
      copy:
        content: |
          <html>
          <head><title>Welcome to Ansible Project</title></head>
          <body>
            <h1>Hello from Ansible </h1>
            <p>This page is deployed using Ansible on AWS EC2.</p>
          </body>
          </html>
        dest: /var/www/html/index.html
```
### 4. Run the Playbook
```sh
ansible-playbook -i hosts install_nginx.yml
```

### 5. Verify
Open your browser:
```sh
http://54.210.xx.xx
```
✅ You’ll see “Hello from Ansible ” page served by Nginx.
