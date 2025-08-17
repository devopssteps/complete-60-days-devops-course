# Day 16: CI/CD for Docker Project 

## ✅ CI/CD for Docker Project – Hands-On Demo
We'll use:
 - GitHub: For source code hosting
 - Jenkins: For CI/CD automation
 - Docker: To build and run the app
 - A sample Python web app as the project

## 🔧 Step-by-Step CI/CD Pipeline
### 🧱 Step 1: Sample Docker Project (Python App)
Project Structure:
```sh
docker-cicd-demo/
├── app.py
├── requirements.txt
├── Dockerfile
```
app.py
```sh
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Deployed via CI/CD Pipeline with Docker!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```
requirements.txt
```sh
flask
```
Dockerfile
```sh
FROM python:3.9-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```
Test locally:
```sh
docker build -t myapp .
docker run -p 5000:5000 myapp
```

### 🔁 Step 2: Push Project to GitHub
```sh
git init
git remote add origin <your-repo-url>
git add .
git commit -m "initial commit"
git push origin main
```

### ⚙️ Step 3: Set Up Jenkins Job for CI/CD
 1. Install Jenkins with Docker (or native)
 2. Install these Jenkins plugins:
    - Git
    - Docker Pipeline
    - Pipeline

### 📜 Step 4: Jenkinsfile for CI/CD Pipeline
Create a file named Jenkinsfile in your project root:
```sh
pipeline {
    agent any

    stages {
        
        stage('Build Docker Image') {
            steps {
                script {
                    //dockerImage = docker.build("myapp:${env.BUILD_NUMBER}")
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ." 
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    //dockerImage.run("-p 5000:5000")
                    sh "docker run -d -p 5000:5000 --name demo-container ${IMAGE_NAME}:${IMAGE_TAG}" 

                }
            }
        }
    }
}
```

### 🚀 Step 5: Create Jenkins Pipeline Job
  - New Item > Pipeline
  - Set your GitHub repo
  - Point to Jenkinsfile
  - Click Build Now

Jenkins will:
 - ✅ Clone the repo
 - ✅ Build Docker image
 - ✅ Run container

### 🧼 Step 6: Clean Up (Optional)
```sh
docker ps -a
docker stop <container_id>
docker rm <container_id>
docker rmi myapp:<build_number>
```
