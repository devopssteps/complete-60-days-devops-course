# day-67- Kubernetes + Python
### 🎯 Goal: Automate Kubernetes tasks.
 - Install kubernetes Python client.
 - Connect to your cluster (~/.kube/config).
 - List pods, services, deployments. 

**“Kubernetes + Python automation”** is **very powerful for DevOps engineers** and highly demanded in real jobs.

---

# 🚀 Kubernetes + Python (Hands-On Automation)

## 🎯 Goal

Automate Kubernetes tasks using **Python** instead of kubectl:

* List pods
* Create a deployment
* Scale deployment
* Delete resources

---

# 🧱 Prerequisites

### ✔ Tools required

```bash
# Python
python3 --version

# Kubernetes
kubectl version --client
minikube status   # or EKS

# Python Kubernetes client
pip3 install kubernetes
```

### ✔ kubeconfig must exist

```bash
ls ~/.kube/config
```

---

# 🧠 How Python Talks to Kubernetes

```
Python Script
   ↓
Kubernetes Python Client
   ↓
Kubeconfig / ServiceAccount
   ↓
Kubernetes API Server
```

---

# 🟢 DEMO 1: List All Pods Using Python

📄 **File:** `list_pods.py`

```python
from kubernetes import client, config

# Load kubeconfig (~/.kube/config)
config.load_kube_config()

v1 = client.CoreV1Api()

pods = v1.list_pod_for_all_namespaces()

for pod in pods.items:
    print(f"Namespace: {pod.metadata.namespace} | Pod: {pod.metadata.name}")
```

▶ Run:

```bash
python3 list_pods.py
```

---

# 🟢 DEMO 2: Create a Deployment Using Python

📄 **File:** `create_deployment.py`

```python
from kubernetes import client, config

config.load_kube_config()

apps_v1 = client.AppsV1Api()

deployment = client.V1Deployment(
    metadata=client.V1ObjectMeta(name="python-nginx"),
    spec=client.V1DeploymentSpec(
        replicas=2,
        selector=client.V1LabelSelector(
            match_labels={"app": "python-nginx"}
        ),
        template=client.V1PodTemplateSpec(
            metadata=client.V1ObjectMeta(labels={"app": "python-nginx"}),
            spec=client.V1PodSpec(
                containers=[
                    client.V1Container(
                        name="nginx",
                        image="nginx:latest",
                        ports=[client.V1ContainerPort(container_port=80)]
                    )
                ]
            )
        )
    )
)

apps_v1.create_namespaced_deployment(
    namespace="default",
    body=deployment
)

print("Deployment created successfully")
```

▶ Run:

```bash
python3 create_deployment.py
```

Verify:

```bash
kubectl get deploy
kubectl get pods
```

---

# 🟢 DEMO 3: Scale Deployment Using Python

📄 **File:** `scale_deployment.py`

```python
from kubernetes import client, config

config.load_kube_config()
apps_v1 = client.AppsV1Api()

apps_v1.patch_namespaced_deployment_scale(
    name="python-nginx",
    namespace="default",
    body={"spec": {"replicas": 5}}
)

print("Deployment scaled to 5 replicas")
```

▶ Run:

```bash
python3 scale_deployment.py
```

---

# 🟢 DEMO 4: Delete Deployment Using Python

📄 **File:** `delete_deployment.py`

```python
from kubernetes import client, config

config.load_kube_config()
apps_v1 = client.AppsV1Api()

apps_v1.delete_namespaced_deployment(
    name="python-nginx",
    namespace="default"
)

print("Deployment deleted")
```

▶ Run:

```bash
python3 delete_deployment.py
```

---

# 🔥 REAL DEVOPS AUTOMATION USE CASES

| Use Case                 | Python + Kubernetes |
| ------------------------ | ------------------- |
| Auto scale workloads     | ✔                   |
| Cleanup failed pods      | ✔                   |
| CI/CD deployment         | ✔                   |
| Health checks            | ✔                   |
| Multi-cluster automation | ✔                   |

---

# 🧪 MINI PROJECT (Perfect for Students)

### 🎯 Project: Auto Cleanup CrashLoopBackOff Pods

```python
from kubernetes import client, config

config.load_kube_config()
v1 = client.CoreV1Api()

pods = v1.list_pod_for_all_namespaces()

for pod in pods.items:
    if pod.status.container_statuses:
        for cs in pod.status.container_statuses:
            if cs.state.waiting and cs.state.waiting.reason == "CrashLoopBackOff":
                print("Deleting pod:", pod.metadata.name)
                v1.delete_namespaced_pod(
                    name=pod.metadata.name,
                    namespace=pod.metadata.namespace
                )
```

---

# 🧠 Interview Question (Very Important)

**Q:** Why use Python instead of kubectl?
**A:**

> Python enables automation, logic, loops, error handling, and integration with CI/CD tools.

