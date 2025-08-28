# Day-28 : Kubernetes Architecture 

### 📌 1. What is Kubernetes?
Kubernetes, often abbreviated as K8s, is an open-source system designed to automate the deployment, scaling, and management of containerized applications. It acts as a platform for orchestrating containers, which are lightweight, isolated environments that package an application and its dependencies. 
 -	Developed by Google, now maintained by CNCF
 -	Manages containerized apps at scale
 -	Automates deployment, scaling, and healing

### 📌 2. Kubernetes Architecture Overview 
### 🔷 Control Plane Components (Master Node):
  - API Server: Main entry point
  - Scheduler: Decides where to run Pods
  -	Controller Manager: Maintains desired state, detect cluster state chnage
  -	etcd: Distributed key-value store (reource avaiblec, cluster helathy, cluster state chage)
### 🔷 Node Components (Worker Node):
  - kubelet: Talks to API server, runs pods
  -	kube-proxy: Handles networking
  - Container runtime (Docker or containerd)
📊 See the diagrams 

![Diagram](https://github.com/rajivsiddiqui/image-use/blob/main/k8s-architecture.png) 

### 📌 3. Hands-On Demo with Minikube 
Step 1: Start Minikube
```sh
minikube start
```
Step 2: Check Cluster Info

```sh
kubectl cluster-info
kubectl get nodes
```
Step 3: Deploy a Sample App
```sh
kubectl create deployment myapp --image=httpd
kubectl expose deployment myapp --type=NodePort --port=80
minikube service myapp
```
Step 4: Check Kubernetes Components
```sh
kubectl get pods -A
kubectl get svc
kubectl describe pod <pod-name>
```
