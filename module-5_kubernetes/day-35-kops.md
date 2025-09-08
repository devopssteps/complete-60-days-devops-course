# Day-35: Professonal kubernetes cluster create by KOPS

AWS KOPS (Kubernetes Operations) is one of the classic tools to create, manage, and upgrade Kubernetes clusters on AWS

### What is KOPS?
KOPS = Kubernetes Operations
 - It helps you create, upgrade, and manage production-grade Kubernetes clusters on AWS (and other cloud providers).
 - Think of it as kubectl but for managing entire clusters.
 - It automates EC2, VPC, Route53, IAM roles, Security Groups, Autoscaling Groups for your Kubernetes cluster.

### 🛠 Hands-On Demo: Setup Kubernetes Cluster on AWS using KOPS
### Step 1: Prerequisites
 - An AWS account (IAM user with ```AdministratorAccess```).
 - A domain or subdomain hosted in Route53 (KOPS uses DNS for cluster discovery). Example: ```devopssteps.com```.
 - Create a ec2 and install the following command to this ec2
 - Install AWS cli 
  ```sh
   sudo apt update
   sudo apt install unzip
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install
   # chceck the version of AWS Cli
   aws --version
 ```
  - Confiure AWS cresedtails in ec2
 ```sh
 aws configure
 ```
  - Create a ssh key in ec2
```sh
 ssh-keygen
```

Install tools:
# Install KOPS
```sh
curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
chmod +x kops
sudo mv kops /usr/local/bin/kops
```
# Install kubectl
URL: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux
```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
### Step 2: Create an S3 bucket for KOPS state ( KOPS stores cluster configuration in an S3 bucket).
- We can create the s3 bucket from web console
- OR, we can do from command line as follows
```sh
aws s3api create-bucket \
  --bucket my-kops-state-store \
  --region us-east-1

export KOPS_STATE_STORE=s3://my-kops-state-store
```
### Step 3: Create the cluster config
```sh
kops create cluster --name=<your-domain-name> --state=s3://<your-s3-bucket-name> --zones=us-east-1a,us-east-1b --node-count=2 --node-size=t3.small --control-plane-size=t3.medium --dns-zone=<your-domain-name> --node-volume-size=12 --control-plane-volume-size=12 --ssh-public-key ~/.ssh/<your ssh public key in ec2 which is .pub>
```
Example:
```sh
kops create cluster --name=devopssteps.com --state=s3://kops-install-bucket --zones=us-east-1a,us-east-1b --node-count=2 --node-size=t3.small --control-plane-size=t3.medium --dns-zone=devopssteps.com --node-volume-size=12 --control-plane-volume-size=12 --ssh-public-key ~/.ssh/id_ed25519.pub
```
OR, the following way if you create s3 from command line
```sh
kops create cluster \
  --name=k8s.example.com \
  --state=${KOPS_STATE_STORE} \
  --zones=us-east-1a,us-east-1b \
  --node-count=2 \
  --node-size=t3.medium \
  --master-size=t3.medium \
  --dns-zone=k8s.example.com
```
This generates the cluster config.

### Step 4: Update and validate the cluster
Update the cluster
```sh
kops update cluster --name=<your-domain-name> --state=s3://<your-bucket-name> --yes –admin
```
example:
```sh
kops update cluster --name=devopssteps.com --state=s3://kops-install-bucket --yes --admin
```
Validate the cluster
```sh
kops validate cluster --name=devopssteps.com --state=s3://kops-install-bucket 
```
Delete the cluster 
```sh
kops delete cluster --name=devopssteps.com --state=s3://kops-install-bucket --yes 
```
Edit the cluster 
```sh
kops edit cluster --name=devopssteps.com
```
The update command provisions EC2 instances, VPC, IAM roles, Route53 DNS entries, and Kubernetes control plane.

### Step 5: Verify Cluster is working properly 
After 5–10 minutes:
```sh
kubectl get nodes
kubectl get pods --all-namespaces
```
You should see your Kubernetes nodes.

### Step 6: Deploy a Sample App
```sh
kubectl create deployment nginx --image=nginx
kubectl expose deployment nginx --port=80 --type=LoadBalancer
kubectl get svc
```
Copy the LoadBalancer DNS and test in your browser.

### Step 7: Delete Cluster
When done:
```sh
kops delete cluster --name=k8s.example.com --yes
```

