# Day-68: CI/CD-Integration-Using-Python-in-Jenkins

### Automate Docker Build, Push & Notifications
🎯 Goal
 - Use Python inside Jenkins
 - Build Docker image
 - Push image to Docker Hub
 - Deploy Docker container
 - Send Slack / Email notifications
 - Replace shell-heavy pipelines with Python automation

### 🧱 Architecture (Explain First)
```sh
GitHub Push
   ↓
Jenkins Pipeline
   ↓
Python Script
   ├── Docker build
   ├── Docker push
   ├── Docker run
   └── Send notification (Slack / Email)
```
### 🧰 Prerequisites
 - ✔ Jenkins running (Docker or VM)
 - ✔ Docker installed on Jenkins node
 - ✔ Python 3 installed
 - ✔ Docker Hub account
 - ✔ Slack Webhook OR Email SMTP

### 🟢 STEP 1: Sample Docker App (Node.js)
📁 app/server.js
```sh
const http = require("http");

http.createServer((req, res) => {
  res.end("Hello from Docker App");
}).listen(3000);
```

📁 Dockerfile
```sh
FROM node:18
WORKDIR /app
COPY app/ .
EXPOSE 3000
CMD ["node", "server.js"]
```

### 🟢 STEP 2: Python Script for CI/CD Automation
📄 File: cicd.py
```sh
import subprocess
import smtplib
from email.mime.text import MIMEText
import requests
import os

IMAGE = "devopssteps/node-demo:latest"
CONTAINER = "node-demo"
# Slack now not allow to add the webshook direct in your code so we need to add in jenkins credentails then call it gere
#SLACK_WEBHOOK = "my-slack-url"  
SLACK_WEBHOOK = os.getenv("SLACK_WEBHOOK")

def run_cmd(cmd):
    subprocess.check_call(cmd, shell=True)

def docker_build_push():
    run_cmd(f"docker build -t {IMAGE} .")
    run_cmd(f"docker push {IMAGE}")

def docker_deploy():
    run_cmd(f"docker rm -f {CONTAINER} || true")
    run_cmd(f"docker run -d --name {CONTAINER} -p 3000:3000 {IMAGE}")

def slack_notify(msg):
    requests.post(SLACK_WEBHOOK, json={"text": msg})

def email_notify(msg):
    sender = "rajiv19831@gmail.com"
    receiver = "rajiv19831@gmail.com"

    email = MIMEText(msg)
    email["Subject"] = "CI/CD Pipeline Status"
    email["From"] = sender
    email["To"] = receiver

    with smtplib.SMTP("smtp.gmail.com", 587) as server:
        server.starttls()
        server.login(sender, "aaaa")
        server.send_message(email)

if __name__ == "__main__":
    try:
        docker_build_push()
        docker_deploy()
        #slack_notify("✅ Docker App Deployed Successfully")
        #email_notify("Pipeline SUCCESS: App deployed")
    except Exception as e:
        #slack_notify(f"❌ Pipeline Failed: {e}")
        #email_notify(f"Pipeline FAILED: {e}")
        raise
```
 - For better understand, First only create docker image and push by python, so comment slack and email function then run the python file
 - Second crate app password for email and then comment out the email function then run the file
 - Third create a Slack webhook and export it in your local pc if you run and test your file in local pc
      - Create the slack steps I put at bottom 
      - ```export SLACK_WEBHOOK="https://hooks.slack.com/services/*************/*********/**********"```
      - Now run the python file
  

### 🟢 STEP 3: Jenkinsfile (Very Clean)
📄 Jenkinsfile
```sh
pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credential')
        SLACK_WEBHOOK = credentials('SLACK_WEBHOOK')
    }

    stages {
        stage('Checkout') {
            steps {
                //git 'https://github.com/rajivsiddiqui/python-docker-cicd.git'
                git branch: 'main', url: 'https://github.com/rajivsiddiqui/python-docker-cicd'
            }
        }

        stage('Docker Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                //sh 'docker push devopssteps/myapp:latest'
            }
        }

        stage('Run Python CI/CD Script') {
            steps {
                sh 'python3 cicd.py'
            }
        }
    }
}
```

### 🟢 STEP 4: Jenkins Credentials Setup
Docker Hub
 - ID: dockerhub-creds
 - Type: Username + Password

Slack
 - Incoming Webhook URL

Email
 - Use App Password (Gmail)
 - Never hardcode real passwords in production

### Interview Quesion & Answer
1️⃣ Why do we use Python in Jenkins CI/CD pipelines?
Answer:
 - Python is used to automate complex tasks such as:
 - Calling APIs (Slack, GitHub, AWS)
 - Custom deployment logic
 - Docker & Kubernetes automation
 - Sending notifications
Python is more flexible than shell scripts and easier to maintain.


### --------
## 🟢 STEP 1: Create / Open Slack Workspace

Go to your Slack workspace.

Example:

```
https://your-workspace.slack.com
```

---

## 🟢 STEP 2: Create a Slack App

1. Open:
   👉 [https://api.slack.com/apps](https://api.slack.com/apps)
2. Click **Create New App**
3. Select **From scratch**
4. App Name:

```
jenkins-notifier
```

5. Select your **Workspace**
6. Click **Create App**

---

## 🟢 STEP 3: Enable Incoming Webhooks

1. Inside your app dashboard
2. Click **Incoming Webhooks**
3. Turn **ON**:

```
Activate Incoming Webhooks
```

---

## 🟢 STEP 4: Create Webhook URL

1. Click **Add New Webhook to Workspace**
2. Select Slack channel:

```
#jenkins-alerts
```

(you can create this channel first)
3. Click **Allow**

🎉 Slack will generate a webhook URL like:

```
https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXX
```

⚠️ **Keep this URL secret!**

---

## 🟢 STEP 5: Test Webhook from Terminal (Optional but Recommended)

Run this on your local PC or Jenkins server:

```bash
curl -X POST -H 'Content-type: application/json' \
--data '{"text":"Hello from Jenkins Slack Webhook 🚀"}' \
https://hooks.slack.com/services/XXXX/XXXX/XXXX
```

✅ Message should appear in Slack channel.

---

## 🟢 STEP 6: Store Webhook Securely in Jenkins (BEST PRACTICE)

### Jenkins → Manage Jenkins → Credentials

1. Add **New Credentials**
2. Kind: **Secret Text**
3. Secret:

```
https://hooks.slack.com/services/XXXX/XXXX/XXXX
```

4. ID:

```
slack-webhook
```

5. Save

---
