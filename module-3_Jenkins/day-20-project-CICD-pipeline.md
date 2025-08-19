### day 20 - Project: CI/CD pipeline for Docker app using Jenkins + GitHub
Project: CI/CD pipeline for Docker app using Jenkins + GitHub"

### 🔹 Step 1: Project Overview
 - What CI/CD is.
 - Why Jenkins + GitHub + Docker is a powerful combo.
 - The goal: Automate build, test, and deployment of a Dockerized application from GitHub using Jenkins.

### 🔹 Step 2: Prerequisites
 - Installed: Docker, Jenkins, GitHub repo with your app.
 - Jenkins plugins:
   - Git
   -  Pipeline
   - Docker Pipeline
  
### 🔹 Step 3: GitHub Repo Setup
 - Create a public or private GitHub repository with your Dockerized app (e.g., Node.js, Python, or PHP).
 - Include:
    - Dockerfile
    - Application source code
    - Jenkinsfile

### 🔹 Step 4: Jenkins Setup
 - Install Jenkins on your server/VM (Docker or local machine).
 - Configure Jenkins credentials for GitHub.
 - Install Docker inside Jenkins environment (so Jenkins can build and push Docker images).

### 🔹 Step 5: Pipeline Script (Jenkinsfile)
Example Jenkinsfile for Docker CI/CD:
```sh
pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-user-pass')
        IMAGE_NAME = "yourdockerhubusername/myapp"
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/yourrepo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:latest .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    sh 'docker push $IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container...'
                // You can add Kubernetes or Docker run command here
            }
        }
    }
}
```

