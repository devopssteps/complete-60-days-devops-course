# Day-44: Launch Docker App on ECS Fargate

### Hands-on Demo: Launch Docker App on ECS Fargate
### Step 1: Prerequisites
 - AWS account
 - Docker installed locally
 - AWS CLI installed and configured (aws configure)
 - A simple Docker app (e.g., Node.js, Python, or Nginx)

### Step 2: Create and Push Image to ECR
 1. Create ECR repo
```sh
aws ecr create-repository --repository-name my-docker-app
```
 2. Authenticate Docker to ECR
```sh
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_Account_ID>.dkr.ecr.us-east-1.amazonaws.com
```
 3. Build & Push Image
```sh
aws ecs create-cluster --cluster-name my-fargate-cluster
```

### Step 4: Create Task Definition
```ecs-task-def.json```
```sh
{
  "family": "my-docker-task",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512",
  "containerDefinitions": [
    {
      "name": "myapp",
      "image": "<AWS_Account_ID>.dkr.ecr.us-east-1.amazonaws.com/my-docker-app:latest",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp"
        }
      ],
      "essential": true
    }
  ]
}
```
Register task:
```sh
aws ecs register-task-definition --cli-input-json file://ecs-task-def.json
```

### Step 5: Create Security Group & VPC (if needed)
```sh
aws ec2 create-security-group --group-name myapp-sg --description "Allow HTTP"
aws ec2 authorize-security-group-ingress --group-name myapp-sg --protocol tcp --port 80 --cidr 0.0.0.0/0
```
### Step 6: Run Service on ECS Fargate
```sh
aws ecs create-service \
  --cluster my-fargate-cluster \
  --service-name myapp-service \
  --task-definition my-docker-task \
  --desired-count 1 \
  --launch-type FARGATE \
  --network-configuration "awsvpcConfiguration={subnets=[subnet-xxxxxx],securityGroups=[sg-xxxxxx],assignPublicIp=ENABLED}"
```

### Step 7: Access Your App
 - Get Public IP from ECS Service → Tasks → ENI details
 - Open browser:
```sh
http://<Public_IP>
```
✅ Now your Docker app is live on ECS Fargate
