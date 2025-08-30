# Day 29: Minikube Installation & kubectl

Below, I’ll give you a hands-on explanation with steps to installing minikube & Kubectl. 

# 📌 Hands-On Explanation

### 1. Install Minikube
👉 Install dependencies:
<br>
Ubuntu/Debian
```sh
sudo apt-get update -y
sudo apt-get install -y curl wget apt-transport-https
```
👉 Install Minikube:
```sh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
👉 Verify:
```sh
minikube version
```
### 2. Start Minikube Cluster
```sh
minikube start --driver=docker
```
👉 This will create a single-node Kubernetes cluster locally.
<br>
Check status:
```sh
minikube status
```
### 3. Install kubectl
👉 Download kubectl:
```sh
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
👉 Verify:
```sh
kubectl version --client
```

### 4. Test kubectl with Minikube
👉 Check nodes:
```sh
kubectl get nodes
```
You should see minikube node in Ready state. ✅
<br>
👉 Create a test Pod:
```sh
kubectl run hello-minikube --image=nginx --port=80
```
👉 Check Pods:
```sh
kubectl get pods
```
👉 Expose Pod:
```sh
kubectl expose pod hello-minikube --type=NodePort --port=80
```
👉 Access app in browser:
```sh
minikube service hello-minikube
```
