### day-24 Ansible Roles & Variables
"Ansible Roles & Variables" is a must-learn topic for DevOps and infrastructure automation. Below is a complete explanation with hands-on examples.

## ✅ Topic: Ansible Roles & Variables – Explained with Hands-On Example

### 🔹 What Are Ansible Roles?
Roles are a way to organize your playbooks into reusable components.

A role has a standard folder structure and separates tasks, handlers, variables, files, templates, and more.

### 📁 Default Role Structure
```sh
my-role/
├── tasks/
│   └── main.yml
├── handlers/
│   └── main.yml
├── vars/
│   └── main.yml
├── templates/
│   └── index.html.j2
├── files/
│   └── example.txt
├── defaults/
│   └── main.yml
└── meta/
    └── main.yml
```

### 🧠 What Are Variables in Ansible?
Variables allow you to dynamically control values in tasks, templates, and playbooks.
Defined in:
 - vars/, defaults/ folders
 - Directly in playbooks
 - Passed as extra vars (--extra-vars)
 - From inventory files or host/group vars

### 🛠 Hands-On: Create and Use a Role

### ✅ Step 1: Create a Role
```sh
ansible-galaxy init apache_role
```
This creates a pre-defined structure in apache_role/.

### ✅ Step 2: Edit Role Files
🔧 apache_role/tasks/main.yml
```sh
- name: Install Apache
  apt:
    name: apache2
    state: present
  become: true

- name: Copy HTML Template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
  become: true

- name: Start Apache
  service:
    name: apache2
    state: started
    enabled: true
  become: true
```

📄 apache_role/templates/index.html.j2
```sh
<html>
  <head><title>{{ site_title }}</title></head>
  <body>
    <h1>Welcome to {{ site_title }}</h1>
  </body>
</html>
```

📦 apache_role/vars/main.yml
```sh
site_title: "My Awesome Website"
```

### ✅ Step 3: Use Role in a Playbook
site.yml
```sh
- name: Setup Apache Server
  hosts: all
  become: true
  roles:
    - apache_role
```
### ✅ Step 4: Run the Playbook
```sh
ansible-playbook -i inventory.ini site.yml
```

### 🎯 What Happens?
Ansible will:
 - Install Apache
 - Use variables (site_title) in templates
 - Start the Apache service
 - Organize everything cleanly inside a role
