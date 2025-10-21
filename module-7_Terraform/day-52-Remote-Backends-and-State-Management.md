# Day-52: Terraform Remote Backends & State Management

# Hands-on Demo: Terraform Remote Backend & State Management

### Step 1: Create S3 Bucket and DynamoDB Table
You can use AWS Console or CLI.<br>
Using CLI:
```sh
aws s3 mb s3://terraform-remote-backend-demo-rajiv
aws dynamodb create-table \
  --table-name terraform-locks \
  --attribute-definitions AttributeName=LockID,AttributeType=S \
  --key-schema AttributeName=LockID,KeyType=HASH \
  --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5
```

### Step 2: Terraform Code (```main.tf```)
```sh
terraform {
  backend "s3" {
    bucket         = "terraform-remote-backend-demo-rajiv"
    key            = "dev/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "RemoteBackendDemo"
  }
}

output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

### Step 3: Initialize Terraform
```sh
terraform init
```
You’ll see:
```sh
Initializing the backend...
Successfully configured the backend "s3"!
```
Terraform now automatically stores the state file in your S3 bucket.

### Step 4: Apply Infrastructure
```sh
terraform apply -auto-approve
```
 - ✅ EC2 instance is created.
 - ✅ Terraform state file is uploaded to your S3 bucket.
 - ✅ DynamoDB table locks the state during execution to prevent conflicts.  

### Step 5: Verify Remote State
 - Go to AWS S3 → terraform-remote-backend-demo-rajiv → dev/terraform.tfstate
 - Open the file and you’ll see the full state JSON
 - Check DynamoDB → terraform-locks to see the state lock entry

### Step 6: Test State Locking
If you try to run another terraform apply from a different terminal at the same time, you’ll see:
```sh
Error: Error acquiring the state lock
```
That means state locking is working perfectly.

### Best Practices for Terraform State Management
 - ✅ Never store terraform.tfstate in GitHub
 - ✅ Always use a remote backend (S3, GCS, Terraform Cloud)
 - ✅ Enable versioning in your S3 bucket
 - ✅ Enable encryption (AES-256 or KMS)
 - ✅ Use DynamoDB for locking in team environments



### Summary :
 - Terraform Remote Backend ensures collaboration, consistency, and security-
 - It’s a must-have setup for every real-world DevOps team.”

