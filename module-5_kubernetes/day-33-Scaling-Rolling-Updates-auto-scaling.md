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
