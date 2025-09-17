# Day-43: ECR, ECS Basics

### Hands-on Demo: AWS ECR & ECS Basics
### 1. Create an ECR Repository
 - Console → ECR → Create Repository → Name: my-app-repo.
 - Or via CLI:
```sh
aws ecr create-repository --repository-name my-app-repo
```
Note the repository URI (e.g., ```123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app-repo```).

### 2. Authenticate Docker with ECR
```sh
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
```

### 3. Build and Push Image to ECR
```sh
docker build -t my-app .
docker tag my-app:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app-repo:latest
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-app-repo:latest
```

### 4. Create ECS Cluster
 - Console → ECS → Create Cluster → Select Fargate.
 - Name: my-ecs-cluster.

### 5. Define Task Definition
 - Task Definition → Create → Launch type: Fargate
 - Container: Use the ECR image URI pushed earlier.
 - CPU/Memory: Choose defaults.

### 6. Run Service on ECS
 - Create Service → Cluster: my-ecs-cluster → Task definition.
 - Configure desired tasks (e.g., 1).
 - Attach to a VPC + Security Group (allow HTTP 80).
 - Now your app is running in ECS from an image stored in ECR 🚀.
