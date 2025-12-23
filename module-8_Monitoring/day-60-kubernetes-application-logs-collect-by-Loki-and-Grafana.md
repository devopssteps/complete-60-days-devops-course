# Day 60: hands-on-demo-collect-kubernetes-logs-by-loki+grafana

Complete hands-on demo to:
 - ✅ Run a Node.js application in Kubernetes
 - ✅ Collect application logs in Loki
 - ✅ View logs in Grafana

### 🔹 Architecture (Simple)
```sh
Node.js App (K8s Pod)
        ↓ logs
      Promtail 
        ↓
      Loki
        ↓
     Grafana
```

### 🔹 Prerequisites
 - Kubernetes cluster (Minikube / Kind / EKS)
 - kubectl
 - Helm
 - Docker

Here I used Minikube for demo.

### 🔹 Step 1: Create a Simple Node.js App
app.js
```sh
const express = require("express");
const app = express();

setInterval(() => {
  console.log("INFO: App is running at " + new Date().toISOString());
}, 3000);

app.get("/", (req, res) => {
  console.log("INFO: Home API called");
  res.send("Hello from Node.js running in Kubernetes!");
});

app.listen(3000, () => {
  console.log("Server started on port 3000");
});
```

package.json
```sh
{
  "name": "node-log-demo",
  "version": "1.0.0",
  "main": "app.js",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

### 🔹 Step 2: Dockerize Node.js App
Dockerfile
```sh
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY app.js .
EXPOSE 3000
CMD ["node", "app.js"]
```
Build & Push (example)
```sh
docker build -t devopssteps/node-log-demo:v1 .
docker push devopssteps/node-log-demo:v1
```

### 🔹 Step 3: Deploy Node.js App to Kubernetes
node-deployment.yaml
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
        - name: node-app
          image: devopssteps/node-log-demo:v1
          ports:
            - containerPort: 3000
```
node-service.yaml
```sh
apiVersion: v1
kind: Service
metadata:
  name: node-service
spec:
  type: NodePort
  selector:
    app: node-app
  ports:
    - port: 3000
      targetPort: 3000
```
Deploy the k8s 
```sh
kubectl apply -f node-deployment.yaml
kubectl apply -f node-service.yaml
```
Check logs:
```sh
kubectl logs -l app=node-app
```

### 🔹 Step 4: Install Loki + Promtail using Helm
Add Helm Repo
```sh
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
Install Loki Stack
```sh
helm install loki grafana/loki-stack \
  --namespace monitoring \
  --create-namespace \
  --set grafana.enabled=true \
  --set promtail.enabled=true
```
This installs:
 - Loki
 - Promtail (DaemonSet)
 - Grafana

### 🔹 Step 5: Verify everything is up 
```sh
kubectl get pods -n monitoring
kubectl get all -n monitoring
```

### 🔹 Step 6: Access Grafana
```sh
kubectl port-forward svc/loki-grafana 3000:80 -n monitoring
```
Open browser:
```sh
http://127.0.0.1:3000
```
Get the password
First get the secret
```sh
kubectl describe secret loki-grafana -n monitoring
```
Now get decode the secret
```
kubectl get secret loki-grafana -o jsonpath="{.data.admin-password}" -n monitoring | base64 --decode
```
Login
```sh
Username: admin
Password: Password we decode from the previos step.
```

### 🔹 Step 7: Configure Loki Data Source (If not auto-added)
 - Settings → Data Sources → Add data source
 - Select Loki
 - URL:
```sh
http://loki:3100
```
 - Save & Test

### 🔹 Step 8: View Node.js Logs in Grafana
Go to Explore → Select Loki
<br>
Query: Select ```app = node-app``` from Label filters
```sh
{app="node-app"}
```
or
```sh
{namespace="default"}
```
You will see:
```sh
INFO: App is running at 2025-xx-xxTxx
INFO: Home API called
Server started on port 3000
```
Node.js logs are now visible in Grafana via Loki and these logs from stdout are collected automatically ✅

### 🔹 Step 9: Generate Logs (Live Demo)
```sh
kubectl port-forward svc/node-service 3001:3000
```
Open browser:
```sh
http://localhost:3001
```
Each refresh generates logs → Instantly visible in Grafana.

### 🔹 Filter Logs by Level
```sh
{app="node-app"} |= "INFO"
```

### 🔹 Summary
 - ✔ Node.js logs written to stdout
 - ✔ Promtail collects logs from Kubernetes
 - ✔ Loki stores logs
 - ✔ Grafana visualizes logs

### Delete everything 
```sh
helm list -n monitoring                       # check the helm we created
helm uninstall loki -n monitoring             # delete the loki, grafana,promtail which we creted by helm
kubectl delete deployment.apps/node-app       # delete the deployment
kubectl delete service/node-service           # delete the service
kubectl get all                               # check everything is deleted
```


