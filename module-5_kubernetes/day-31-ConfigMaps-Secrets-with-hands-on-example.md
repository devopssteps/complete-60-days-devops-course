### day 31: explain k8s ConfigMaps, Secrets with hands on example"

### ConfigMap
 - 🔹 Concept: ConfigMaps store non-sensitive configuration values.
 - 🔹 Demo:
ConfigMap (app-config.yaml)
```sh
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  APP_DEBUG: "false"
```
Use in Deployment
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: app
        image: nginx:latest
        envFrom:
        - configMapRef:
            name: app-config
```
👉 Run:
```sh
kubectl apply -f app-config.yaml
kubectl apply -f app-deployment.yaml
kubectl exec -it <pod-name> -- printenv | grep APP_
```
### Secret
 - 🔹 Concept: Secrets store sensitive data like passwords, API keys.
 - 🔹 Demo:
Secret (db-secret.yaml)
```sh
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_USER: YWRtaW4=   # base64(admin)
  DB_PASS: cGFzc3dvcmQ= # base64(password)
```
Use in Deployment
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: db-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: dbapp
  template:
    metadata:
      labels:
        app: dbapp
    spec:
      containers:
      - name: dbapp
        image: mysql:5.7
        env:
        - name: MYSQL_USER
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_USER
        - name: MYSQL_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: DB_PASS
```
👉 Run:
```sh
kubectl apply -f db-secret.yaml
kubectl apply -f db-deployment.yaml
```
### Wrap-up
 - ConfigMaps manage non-sensitive configs.
 - Secrets handle sensitive credentials.
 - Services expose applications.
 - Ingress provides external access with routing.
 - These are must-know concepts for DevOps & Cloud Engineers.

