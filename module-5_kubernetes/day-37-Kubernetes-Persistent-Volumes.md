# Day 37: Kubernetes Persistent Volumes
Kubernetes Persistent Volumes is one of the most important concepts for stateful apps in Kubernetes.

### 📌 Hands-on Demo: Kubernetes Persistent Volumes (PV & PVC)
Step 1: Create a Persistent Volume (PV)
File: pv.yaml
```sh
apiVersion: v1
kind: PersistentVolume
metadata:
  name: demo-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```
👉 Explanation:
 - This PV provides 1Gi storage.
 - It uses hostPath (good for demo in Minikube).
 - In cloud providers, you’d use EBS (AWS), Persistent Disk (GCP), Azure Disk, etc.

### Step 2: Create a Persistent Volume Claim (PVC)
File: pvc.yaml
```sh
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: demo-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```
👉 Explanation:
 - PVC is like a request for storage.
 - Here we ask for 500Mi, which will be bound from our PV (1Gi available).

### Step 3: Use PVC in a Pod
File: pod.yaml
```sh
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pv-pod
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: demo-storage
  volumes:
    - name: demo-storage
      persistentVolumeClaim:
        claimName: demo-pvc
```
👉 Explanation:
 - This pod runs Nginx.
 - The HTML files will be stored in /mnt/data (from PV) but mounted inside container at /usr/share/nginx/html.
 - Even if pod restarts, data stays on the node disk because of PV.

### Step 4: Apply & Test
Run in terminal:
```sh
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl apply -f pod.yaml
```
Check status:
```sh
kubectl get pv
kubectl get pvc
kubectl get pods
```
Test data persistence:
```sh
kubectl exec -it nginx-pv-pod -- bash
echo "Hello Persistent Volume" > /usr/share/nginx/html/index.html
exit
kubectl delete pod nginx-pv-pod
kubectl apply -f pod.yaml
kubectl exec -it nginx-pv-pod -- cat /usr/share/nginx/html/index.html
```
✅ You will still see "Hello Persistent Volume" — proving persistence.
