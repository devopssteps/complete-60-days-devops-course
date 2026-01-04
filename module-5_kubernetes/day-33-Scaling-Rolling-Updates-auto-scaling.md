### 🔧 1. Why is Scaling? 
  - Why scaling is important (high traffic, redundancy)
  - We can scale in two ways: manually or automatically.

### 📌 Explanation of Kubernetes scaling & Auto Scaling
Kubernetes supports three main types of scaling:
 1. Horizontal Pod Autoscaler (HPA)
    - Scales pods in/out based on CPU/Memory usage or custom metrics.
 2. Vertical Pod Autoscaler (VPA)
    - Adjusts container resource requests/limits automatically.
 3. Cluster Autoscaler (CA)
    - Scales the number of worker nodes in the cluster when pods can’t be scheduled due to insufficient resources.
      
### 🔨 Hands-On: Scale App Up & Down 
```sh
kubectl create deployment myapp --image=httpd
kubectl expose deployment myapp --type=NodePort --port=80

# Scale up to 3 replicas
kubectl scale deployment myapp --replicas=3

kubectl get pods
```

### 🔁 2. What is Rolling Update? 
 - Gradual replacement of old pods with new ones
 - Ensures zero-downtime deployments

### 🔨 Hands-On: Rolling Update Demo 
Before update the Image we need to know the container name by using the following command. <br>
First, check the Deployment spec:
```sh
kubectl describe deployment myapp | grep Image:
kubectl get deployment myapp -o yaml | grep name:
```
You’ll see something like:
```sh
containers:
- image: httpd
  name: httpd   👈 container name
```
when we update, we must use that container name (httpd), not myapp deployment name.
Now use the command to Edit/Update the image
```sh
kubectl set image deployment/myapp httpd=rajivsiddiqui/web2023
```
🔎 Now we check the rollout status, history:
```sh
kubectl rollout undo deployment/myapp
kubectl rollout status deployment/myapp
kubectl rollout history deployment/myapp
```
### 📌 Explanation of Kubernetes Auto Scaling
Kubernetes supports three main types of scaling:
 1. Horizontal Pod Autoscaler (HPA)
    - Scales pods in/out based on CPU/Memory usage or custom metrics.
 2. Vertical Pod Autoscaler (VPA)
    - Adjusts container resource requests/limits automatically.
 3. Cluster Autoscaler (CA)
    - Scales the number of worker nodes in the cluster when pods can’t be scheduled due to insufficient resources.

### 📌 Hands-on Demo: Horizontal Pod Autoscaler
### Step 1: Enable Metrics Server
HPA requires metrics.
```sh
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
kubectl get deployment metrics-server -n kube-system
```

### Step 2: Create a Sample App
app.js (simple CPU stress app in Node.js):
```sh
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send("Hello from Kubernetes Auto Scaling Demo 🚀");
});

// Endpoint to simulate CPU load
app.get('/load', (req, res) => {
  let end = Date.now() + 10000; // 10 sec CPU work
  while (Date.now() < end) { Math.random() * Math.random(); }
  res.send("CPU Load simulated!");
});

app.listen(8080, () => console.log("App running on port 8080"));
```
### Dockerfile
```sh
FROM node:16
WORKDIR /usr/src/app
COPY app.js .
RUN npm init -y && npm install express
EXPOSE 8080
CMD ["node", "app.js"]
```
Build & push:
```sh
docker build -t <your-dockerhub-username>/autoscale-demo .
docker push <your-dockerhub-username>/autoscale-demo
```
### Step 3: Deploy App in Kubernetes
```deployment.yaml```
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: autoscale-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: autoscale-demo
  template:
    metadata:
      labels:
        app: autoscale-demo
    spec:
      containers:
      - name: autoscale-demo
        image: <your-dockerhub-username>/autoscale-demo
        ports:
        - containerPort: 8080
        resources:
          requests:
            cpu: 100m
          limits:
            cpu: 200m
---
apiVersion: v1
kind: Service
metadata:
  name: autoscale-demo
spec:
  selector:
    app: autoscale-demo
  ports:
  - port: 80
    targetPort: 8080
  type: ClusterIP
```
Apply:
```sh
kubectl apply -f deployment.yaml
kubectl get pods
```
### Step 4: Create Horizontal Pod Autoscaler
```sh
kubectl autoscale deployment autoscale-demo --cpu-percent=50 --min=1 --max=5
kubectl get hpa
```

### Step 5: Generate Load
Run a load test inside the cluster:
```sh
kubectl run -i --tty load-generator --image=busybox /bin/sh
# Inside pod run:
while true; do wget -q -O- http://autoscale-demo; done
```

### Step 6: Observe Auto Scaling
```sh
kubectl get hpa --watch
kubectl get pods
```
  - You’ll see replicas increase from 1 → up to 5 when load is high.
  - When load decreases, replicas scale back down automatically.
