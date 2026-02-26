# day-69: Final python module project
### 📌 Project Goal

Use **one Python automation script** to:

1. Deploy a Docker application to **AWS EKS**
2. Update & verify **LoadBalancer service**
3. Collect application logs from Kubernetes
4. Upload logs to **AWS S3**
5. Slack notification 

---

# 🧱 TECHNOLOGY STACK (Interview Gold)

| Tool                     | Purpose                    |
| ------------------------ | -------------------------- |
| Python                   | Automation language        |
| Boto3                    | AWS SDK (S3, EKS metadata) |
| Kubernetes Python Client | K8s automation             |
| Docker                   | Containerized app          |
| AWS EKS                  | Kubernetes cluster         |
| AWS S3                   | Log storage                |
| Slack Webhook            | Notification               |
| Jenkins (optional)       | CI/CD trigger              |

---
# 🗂 PROJECT STRUCTURE

```
cloud-automation-python/
│
├── web/
│   ├── Dockerfile
│   └── app/server.js
│
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
│
├── scripts/
│   ├── deploy_to_eks.py
│   ├── update_lb.py
│   ├── collect_logs.py
│   ├── upload_logs_s3.py
│   
│
├── main.py
└── README.md
```

---

# 🛠 STEP 0 – PREREQUISITES 

* EKS cluster running
* `kubectl` configured
* IAM role with:

  * `AmazonEKSClusterPolicy`
  * `AmazonS3FullAccess`
* Slack Incoming Webhook
* Python 3.8+

# Python Dependencies installed if not

```bash
pip install boto3 kubernetes requests
```

---
# 🧩 STEP 1 – Create k8s deployment 

k8s/deployment.yaml
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-demo-deployment
  labels:
    app: node-demo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-demo
  template:
    metadata:
      labels:
        app: node-demo
    spec:
      containers:
        - name: node-demo-container
          image: devopssteps/node-demo:latest
          imagePullPolicy: Always
          ports:
            - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: node-demo-service
spec:
  type: NodePort # use LoadBalancer when use EKS LB
  selector:
    app: node-demo
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
      nodePort: 30007    # Comment this line when use EKS LB
```
 - kubectl apply -f node-demo.yaml
 - minikube service node-demo-service --url
 - Now we get an ip, browse that ip and we can see that website is running

---

# 🧩 STEP 2 – Deploy Docker App to EKS (Python)

### `deploy_to_eks.py`

```python
from kubernetes import client, config, utils

config.load_kube_config()

k8s_client = client.ApiClient()

utils.create_from_yaml(
    k8s_client,
    "k8s/deployment.yaml",
    namespace="default"
)

print("Deployment created successfully!")
```
📌 Python replaces manual `kubectl apply`.

---

# 🧩 STEP 3 – Update & Verify LoadBalancer

### `update_lb.py`

```python
from kubernetes import client, config
import time

config.load_kube_config()
v1 = client.CoreV1Api()

svc = v1.read_namespaced_service("node-demo-service", "default")

while not svc.status.load_balancer.ingress:
    time.sleep(5)
    svc = v1.read_namespaced_service("node-demo-service", "default")

lb_ip = svc.status.load_balancer.ingress[0].hostname
print(f"🌍 LoadBalancer URL: {lb_ip}")
```

📌 Python waits until AWS assigns ELB.

---

# 🧩 STEP 4 – Collect Application Logs from Kubernetes

### `collect_logs.py`

```python
from kubernetes import client, config

config.load_kube_config()
v1 = client.CoreV1Api()

pods = v1.list_namespaced_pod("default", label_selector="app=node-app")

logs = ""
for pod in pods.items:
    logs += v1.read_namespaced_pod_log(pod.metadata.name, "default")

with open("/tmp/app.log", "w") as f:
    f.write(logs)

print("📄 Logs collected from pods")
```

---

# 🧩 STEP 5 – Upload Logs to AWS S3

### `upload_logs_s3.py`

```python
import boto3

s3 = boto3.client("s3")

s3.upload_file(
    "/tmp/app.log",
    "devops-logs-bucket",
    "eks/app.log"
)

print("☁ Logs uploaded to S3")
```

---
# 🧩 STEP 6 – Send Slack Notification
Beofore run the notify_slack.py make sure you set the salck webhook in your environment variable
```sh
export SLACK_WEBHOOK="https://hooks.slack.com/services/XXXX/XXXX/XXXX"
```
### `notify_slack.py`

```python
import requests
import os
import sys

SLACK_WEBHOOK = os.getenv("SLACK_WEBHOOK")

if not SLACK_WEBHOOK:
    print("❌ SLACK_WEBHOOK environment variable not set!")
    sys.exit(1)

msg = {
    "text": "🚀 EKS Deployment Successful\n"
            "✔ App Deployed\n"
            "✔ LoadBalancer Active\n"
            "✔ Logs Uploaded to S3"
}

response = requests.post(SLACK_WEBHOOK, json=msg)

if response.status_code == 200:
    print("✅ Slack notification sent successfully!")
else:
    print("❌ Failed to send Slack notification:", response.text)
```

---
# 🧠 STEP 7 – Master Script (One Command Automation)

### `main.py`

```python
import os

os.system("python scripts/deploy_to_eks.py")
os.system("python scripts/update_lb.py")
os.system("python scripts/collect_logs.py")
os.system("python scripts/upload_logs_s3.py")


print("🎯 Cloud Automation Completed Successfully")
```

---

# 🎬 After run the main.py file 

1. we can see EKS cluster is empty
2. Run `python main.py`
3. We can see:

   * Pods running
   * LoadBalancer created
   * Logs in S3
   
---

# The following command i used when do the video 

### Check Current kubectl Context
```sh
kubectl config current-context
```
Check Available Contexts
```sh
kubectl config get-contexts
```

Update kubeconfig for EKS
```sh
aws eks update-kubeconfig --region <region> --name <cluster-name>
```
Switch to Minikube Context
```sh
kubectl config use-context minikube
```
Now verify:
```sh
kubectl config current-context
```




