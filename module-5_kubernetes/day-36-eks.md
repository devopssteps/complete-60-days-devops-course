# Day-36: Professonal kubernetes cluster create by EKS
### 🔹 1. Intro (Explain the Concept)
   - EKS = Amazon’s managed Kubernetes service.
   - Why "Professional"? → Because EKS handles control plane, security, scalability, and integrates with AWS IAM, VPC, Load Balancers, CloudWatch.
   - Ideal for production workloads.

### 🔹 2. Hands-On Example (Step by Step)
### Step 1: Prerequisites
 - Install AWS CLI:
```sh
aws configure
```
(Enter Access Key, Secret Key, Region, Output format)
 - Install eksctl:
```sh
curl -sL https://github.com/eksctl-io/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz | tar -xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

 - Install kubectl:
```sh
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

## Step 2: Create an EKS Cluster
```sh
eksctl create cluster \
  --name devops-cluster \
  --version 1.29 \
  --region us-east-1 \
  --nodegroup-name linux-nodes \
  --node-type t3.medium \
  --nodes 2 \
  --nodes-min 2 \
  --nodes-max 4 \
  --managed
```

👉 This will:
 - Create a cluster named devops-cluster
 - Kubernetes version 1.29
 - Managed node group with t3.medium instances (auto scales 2–4 nodes).

### Step 3: Verify Cluster
```sh
kubectl get nodes
kubectl get pods -A
```
### Step 4: Deploy a Sample App
```sh
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --type=LoadBalancer --port=80
```
Check service:
```sh
kubectl get svc
```
👉 Copy the EXTERNAL-IP and open in a browser — you’ll see the Nginx welcome page.

### Step 5: Clean Up
When finished:
```sh
eksctl delete cluster --name devops-cluster --region us-east-1
```
