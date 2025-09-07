### day 32: explain k8s Services & Ingress with hands on example"

Service & Ingerss is one of the most important Kubernetes topic, and I’ll break it down into:
 - Content structure for your video (step-by-step hands-on demo).
 - Sample Kubernetes YAML files for Services & Ingress.


### Kubernetes Service

 - 🔹 Concept: A Service exposes a Pod (or Deployment) to the network inside or outside the cluster.
 - 🔹 Demo:
Deployment + Service Example (nginx-deployment.yaml)
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP

```
👉 Run:
```sh
kubectl apply -f nginx-deployment.yaml
kubectl get svc
```

### Ingress

 - 🔹 Concept: Ingress allows external HTTP/HTTPS traffic into Services with routing rules.
 - 🔹 Demo:
Ingress Example (nginx-ingress.yaml)
```sh
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: myapp.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
```
👉 Run:
```sh
kubectl apply -f nginx-ingress.yaml
kubectl get ingress
```
👉 Test:
```sh
curl -H "Host: myapp.local" http://<INGRESS_CONTROLLER_IP>
```
###  Wrap-up
 - Services expose applications.
 - Ingress provides external access with routing.

