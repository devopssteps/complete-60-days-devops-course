# Kubernetes Health Checks: Liveness & Readiness Probes

## 📌 Explanation + Hands-on Demo
### 🔹 What are Health Checks in Kubernetes?
 - Liveness Probe – checks if the container is alive.
   - If it fails → K8s kills & restarts the container.
   - Used to detect when apps are stuck or crashed.
 - Readiness Probe – checks if the container is ready to serve traffic.
   - If it fails → Pod is removed from Service endpoints until healthy again.
   - Used to prevent sending requests before the app is ready.
 - StartupProbe – determine if an application within a container has successfully started.
   - It is particularly useful for applications that have a long initialization time.

## Hands-on Demo: Example App with Health Checks
### Step 1: Create a simple app (Node.js)
app.js
```sh
const express = require('express');
const app = express();
let healthy = true;

app.get('/healthz', (req, res) => {
  if (healthy) {
    res.status(200).send("OK - Liveness");
  } else {
    res.status(500).send("Not OK - Liveness");
  }
});

app.get('/readyz', (req, res) => {
  res.status(200).send("OK - Readiness");
});

app.listen(3000, () => {
  console.log("App running on port 3000");
});
```
Dockerfile
```sh
FROM node:16
WORKDIR /usr/src/app
COPY app.js .
RUN npm init -y && npm install express
EXPOSE 3000
CMD ["node", "app.js"]
```
Build & push:
```sh
docker build -t devopssteps/health-demo .
docker push devopssteps/health-demo
```

### Step 2: Kubernetes Deployment with Liveness & Readiness
deployment.yaml
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: health-demo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: health-demo
  template:
    metadata:
      labels:
        app: health-demo
    spec:
      containers:
      - name: health-demo
        image: <your-dockerhub-username>/health-demo
        ports:
        - containerPort: 3000
        livenessProbe:
          httpGet:
            path: /healthz
            port: 3000
          initialDelaySeconds: 10
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: /readyz
            port: 3000
          initialDelaySeconds: 5
          periodSeconds: 5
        startupProbe:
          httpGet:
            path: /healthz
            port: 3000
          failureThreshold: 60
          periodSeconds: 10 

---
apiVersion: v1
kind: Service
metadata:
  name: health-demo
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 3000 
  selector:
    app: health-demo
```
Apply:
```sh
kubectl apply -f deployment.yaml
kubectl get pods
```

By this command we can ontiniously monitor the k8s pods
```sh
watch -x kubectl get all
```
Running a loop browse the page url
```sh
while true; do curl http://127.0.0.1:37611/healthz ; echo; done
```
