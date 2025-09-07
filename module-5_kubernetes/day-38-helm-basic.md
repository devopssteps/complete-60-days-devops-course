# Day-38: Helm basic

Helm is one of the most searched Kubernetes topics, so making a clear, hands-on Helm Basics video is a great idea for your channel. Let me break it down for you:


## 🎬 Helm Basics – Hands-On Example for Your Video
### 🔹 1. What is Helm?
 - Helm = “Package Manager for Kubernetes”
 - Instead of writing long YAML files, you package apps into Charts and deploy them with one command.
 - Think of it like apt/yum but for Kubernetes.

### 🔹 2. Install Helm
```sh
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```
Verify:
```sh
helm version
```
### 🔹 3. Add a Helm Repository
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

### 🔹 4. Install an Application (Example: Nginx)
```sh
helm install my-nginx bitnami/nginx
```
👉 This will create all Kubernetes objects (Deployment, Service, ConfigMaps) in one go.
<br>
Check pods:
```sh
kubectl get pods
```
### 🔹 5. View Installed Releases
```sh
helm list
```

### 🔹 6. Upgrade the Release
```sh
helm upgrade my-nginx bitnami/nginx --set service.type=NodePort
```
👉 Changes the Service type without editing YAML manually.

### 🔹 7. Uninstall the Release
```sh
helm uninstall my-nginx
```

### 🔹 8. Create Your Own Chart (Basic Hands-On)
```sh
helm create mychart
```
 - This creates a template folder (charts/, values.yaml, templates/)
 - You can customize values.yaml and deploy your own app.
### Some Command which I used in the demo

```sh
helm repo list    #show repo 
helm repo remove hashicorp	  # reove the repo from your local pc
helm repo update    # update the repo
helm search repo MySQL  # search mysq in your repo
helm install mysql-2025 bitnami/MariaDB    # creaet mysql chart 

#Get the database password
kubectl get secret --namespace default mysql-1-mariadb -o jsonpath="{.data.mariadb-root-password}" | base64 -d

kubectl get all
kubectl exec -it mysql-1-mariadb-0 /bin/bash

helm list
helm uninstall mysql-1
```
# -----------------------------------------------
```sh

<br>
Deploy your chart:
```sh
 helm install myapp ./mychart
```
