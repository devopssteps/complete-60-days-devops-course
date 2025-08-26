# day-26 Ansible + Jenkins Integration
### "Ansible + Jenkins Integration" is a powerful DevOps combination for automating infrastructure deployment through CI/CD pipelines. Below, you’ll get:
 - ✅ A clear explanation
 - 🛠️ Hands-on demo
 
### ✅ What is “Ansible + Jenkins Integration”?
Integrating Ansible with Jenkins allows you to:
 - Use Jenkins to trigger Ansible playbooks
 - Automate infrastructure provisioning, configuration management, and application deployment
 - Run Ansible as part of a CI/CD pipeline

## 🛠️ Hands-On Example: Run Ansible Playbook from Jenkins

### 🔧 Prerequisites
 - Jenkins installed (Docker or local)
 - Ansible installed on Jenkins machine
 - Git installed
 - SSH access to target nodes
 - Jenkins user has proper sudo/SSH permissions

### ✅ Step 1: Install Jenkins on Docker (Optional)
```sh
docker run -d --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```
Access Jenkins at: http://localhost:8080

### ✅ Step 2: Create Ansible Playbook in Git
site.yml
```sh
- name: Install Apache
  hosts: webservers
  become: true
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
```
hosts
```sh
[webservers]
192.168.1.101 ansible_user=ubuntu
```
Push this to your Git repo.

### ✅ Step 3: Configure Jenkins Job
 1. Install Plugins:
      - Git plugin
      - Ansible plugin (or just use shell commands)
 
 2. Create New Freestyle Project
 3. Source Code Management:
      - Git → Provide repo URL
 4. Build Section:
      - Add Execute Shell
      - Sample shell script:
   ```sh
   ansible-playbook -i hosts site.yml
   ```

### ✅ Step 4: Trigger the Job
 - Click Build Now
 - Jenkins checks out the repo and runs the Ansible playbook
 - Apache will be installed on the target servers

### command I use in this demo
```sh
sudo apt-get --purge remove apache2
sudo apt-get remove apache2-common
systemctl status apache2
```


## ✅ Now run ansible playbook by using Jenkins Pipeline (Declarative)
### Step 1:
 - Create a credentails in jenkins
    - manage jenkins -> Jenkins -> Credentials -> System ->Global credentials
    - Makesure what ID you put, we need it, after create the credentails 
### Now create a repo with the following files 
This repo have all data ```https://github.com/devopssteps/day26/tree/main```
 - Jenkinsfile
```sh
pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        //git 'https://github.com/devopssteps/day26.git'
        echo 'test'
      }
    }
    stage('Run Ansible') {
      steps {
        sshagent(['ansible']) {
            sh 'ansible-playbook -i hosts p1.yaml'
            //sh 'ssh -o StrictHostKeyChecking=no ubuntu@54.221.103.118 "echo Connected from Jenkins!"'
        }  
      }
    }
  }
}
```
Here i have created credentails name is ```ansible```
 - hosts
```sh
54.221.103.118 ansible_user=ubuntu ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
 - playbook file (p1.yaml)
```sh
- name: Install Apache
  hosts: all
  become: true
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
```

### Step 2: Now create a jenkins pipeline and build the pipeline 
 - Dashboard -> New Item -> pipeline -> ok -> scroll down -> go to pipeline -> Pipeline script from SCM -> Select Git -> type your git repo in Repository URL 
              -> Branch type main -> Jenkinsfile in Script path -> click ok
 - Now click build now
 - After successfull build you can see apache2 in installed in your target server
