# day-14: Jenkins Jobs, Pipelines, Plugins

### 🧠 Todays class Objective:
By the end of this video, viewers will:
 - Understand what Jenkins jobs and pipelines are
 - Learn the difference between freestyle jobs and pipelines
 - Install and use essential Jenkins plugins
 - Build and run a basic CI/CD pipeline with a real example

## 🛠️ Hands-On Jenkins Video Outline

### ✅ 1. Intro 
“Welcome to Day X of our DevOps series! In this video, we’ll dive deep into Jenkins — how to create jobs, write pipelines, and use plugins — all with hands-on examples. If you're learning CI/CD or preparing for interviews, this is a must-watch!”

### 📢 Call to action:
“Don’t forget to subscribe and turn on the bell so you don’t miss the rest of the series!”

### 📦 2. What is Jenkins? 
 - Open-source automation server
 - Core tool in CI/CD pipelines
 - Supports building, testing, and deploying code automatically

### 🎯 Real-world Example:
Jenkins pulls code from GitHub, runs tests, and deploys to a server.

### ⚙️ 3. Jenkins Installation Quick Recap (Optional) 
(If not already installed, add this: otherwise, skip.)

```sh
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts
```
🔐 Access Jenkins: http://localhost:8080


### 🧱 4. Jenkins Freestyle Job 
 - Go to “New Item” → Freestyle Project
 - Configure:
   - GitHub repo URL
   - Add shell build step: echo Hello from Jenkins
 - Save and run job
🎥 Demo what happens in the console output

### 🧪 5. Jenkins Pipeline Job 
 - Go to “New Item” → Pipeline
 - Add this script in Pipeline section:
```sh
pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/YOUR_REPO_HERE.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the app...'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
            }
        }
    }
}
```
🎥 Run it and explain the logs step-by-step

### 🔌 6. Jenkins Plugins 
 - Go to Manage Jenkins → Plugins
 - Install:
   - Git plugin
   - Pipeline plugin
   - Blue Ocean (for better UI)
   - Docker plugin (if needed later)
 - Explain how plugins add functionality


### 💡 7. Best Practices & Tips 
 - Always use pipeline as code (in Jenkinsfile)
 - Use credentials manager for secrets
 - Keep Jenkins up-to-date
 - Backup Jenkins config using thin backup plugin
