# day-39: Project github URL
```sh
https://github.com/rajivsiddiqui/helm-jenkins-k8s-nodejs
```
### Step 1: Create a Sample App (Node.js Example)
```
git clone https://github.com/devopssteps/nodejs-app
```
This is nodejs simple webiste it has following file structure
  
### Step 2: to run the file locally
 - first clone the code https://github.com/devopssteps/nodejs-app.git
 ### - Run locally
```sh
npm install
npm run dev
# open http://localhost:3000
```
### - Run by docker 
  - build
     ```sh
     docker build -t node-docker-starter:1.0 .
     ```
  - Copy the .env.example to .env
    ```sh
    cp .env.example .env
    ``` 
  - If not createt the .env file
    ```sh
    PORT=3000
    APP_NAME="Node Docker Starter"
    LOG_LEVEL=dev
    ```
 - Run:
   ```sh
   # Ensure you have an .env file (see .env.example)
    docker run -p 3000:3000 --env-file .env node-docker-starter:1.0
   ```
- Now broowse the page
   ```sh
   http://127.0.0.1:3000/
   ```
### - Run by docker-compose
```sh
docker-compose up -d
```
Browse the page
```sh
 http://127.0.0.1:3000/
```
### Step 3: Run by Kubernetes - Create the helm chart

 - build docker image
 ```sh
 docker build -t devopssteps/node-demo-1:1.0 .
```
 - Push image
```sh
docker push devopssteps/node-demo-1:1.0
```
 - Create heml chart  
```sh
helm create nodejs-app
```
 - Edit values.yaml to use your Docker image:
```sh
replicaCount: 2

image:
  repository: devopssteps/node-demo-1
  tag: "1.0"
  pullPolicy: Always # using Always, if any chnage in docker image eventhough tag is same but it will force to deploy pods with new image #IfNotPresent

service:
  type: NodePort
  port: 3000

resources: {}
```
 - Edit Chart file
  ```sh
  apiVersion: v2
  name: nodejs-app
  description: A Helm chart for Node.js App
  type: application
  version: 0.1.0
  appVersion: "1.0"
  ```
 - Delete all .yaml and NOTE.txt file under template folder
  ```sh
  rm -f *.yaml
  rm -f NOTES.txt
 ```
 - Create the deployment.yaml
 ```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-deployment
  labels:
    app: {{ .Chart.Name }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Chart.Name }}
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
    spec:
      containers:
        - name: nodejs-app
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - containerPort: 3000

```
 - Create service.yaml
 ```sh
  apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-service
  labels:
    app: {{ .Chart.Name }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: 3000
      protocol: TCP
  selector:
    app: {{ .Chart.Name }}
```      
 - Test by helm now:
```sh
helm install helm-app ./nodejs-app
kubectl get pods
minikube service nodejs-app
```
 - Browse the page
 
### Step 4: Setup Jenkins Pipeline
Runing on minikube we need to the folowing steps
 - Export Minikube config properly
 ```sh
 kubectl config view --minify --flatten > /tmp/minikube-kubeconfig
 ```
 - Add it to Jenkins
 ```sh
 Go to Manage Jenkins → Credentials → Add Credentials
 Kind: Secret file
 Upload /tmp/minikube-kubeconfig
 ID: minikube-kubeconfig
 ```
 - In Jenkins, create a new Pipeline job.
Add a ```Jenkinsfile``` to your GitHub repo:
```sh
pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'docker-hub-credential'
        DOCKERHUB_USER = 'devopssteps'
        APP_NAME = 'nodejs-app'
        APP_VERSION = '1.0'
        //APP_VERSION = "build-${env.BUILD_NUMBER}"
        KUBE_CONTEXT = 'minikube'
        HELM_RELEASE = 'nodejs-app'
        //KUBECONFIG = "/var/lib/jenkins/kube-minikube/config"
        //MINIKUBE_HOME = "/var/lib/jenkins/kube-minikube/.minikube"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rajivsiddiqui/helm-jenkins-k8s-nodejs.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKERHUB_USER/$APP_NAME:$APP_VERSION ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKERHUB_USER/$APP_NAME:$APP_VERSION
                    """
                }
            }
        }

        stage('Deploy to Kubernetes via Helm') {
            steps {
                script {
                    withCredentials([file(credentialsId: 'minikube-kubeconfig', variable: 'KUBECONFIG')]) {
                        sh """
                            kubectl config use-context minikube
                            helm upgrade --install $HELM_RELEASE ./nodejs-app \
                            --set image.repository=$DOCKERHUB_USER/$APP_NAME \
                            --set image.tag=$APP_VERSION
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful 🎉'
        }
        failure {
            echo '❌ Deployment Failed'
        }
    }
}
```
### Step 5: Verify Deployment
```sh
kubectl get pods
kubectl get svc
minikube service nodejs-app
```
👉 Open browser → App should show:
<br>
Hello from Helm + Jenkins + Minikube!

Now edit the app.js file 
 - push git gub
 - jenkins build
 - check the pods by kubectl get all
 - Now wait few min and when new pods created we can see the new chnage is showing in browser
 


