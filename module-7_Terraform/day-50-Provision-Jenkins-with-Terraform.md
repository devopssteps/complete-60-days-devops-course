# day 50: Provision Jenkins with Terraform (Hands-on Demo)

### Goal
Use Terraform to automatically provision a Jenkins server (on AWS EC2) — instead of creating it manually.

### 🧩 Project Overview
We’ll create:
 - 1️⃣ VPC + Subnet + Security Group
 - 2️⃣ EC2 instance (Ubuntu)
 - 3️⃣ Install Jenkins using user_data script automatically
 - 4️⃣ Output the Jenkins public URL

### 📁 Project Structure
```sh
jenkins-terraform/
│
├── main.tf
├── variables.tf
├── outputs.tf
└── user_data.sh
```

### 🧠 Step 1: Configure Provider (main.tf)
```sh
provider "aws" {
  region = "us-east-1"
}

resource "aws_key_pair" "jenkins_key" {
  key_name   = "jenkins-key"
  public_key = file("~/.ssh/id_rsa.pub")
}

resource "aws_security_group" "jenkins_sg" {
  name        = "jenkins-sg"
  description = "Allow Jenkins and SSH"

  ingress {
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

### ⚙️ Step 2: Add EC2 Instance
```sh
resource "aws_instance" "jenkins_server" {
  ami           = "ami-0bbdd8c17ed981ef9" # Ubuntu 22.04 (update as per region)
  instance_type = "t2.medium"
  key_name      = aws_key_pair.jenkins_key.key_name
  vpc_security_group_ids = [aws_security_group.jenkins_sg.id]
  user_data = file("user_data.sh")

  tags = {
    Name = "Terraform-Jenkins"
  }
}
```

### 🧩 Step 3: Jenkins Installation Script (user_data.sh)
```sh
#!/bin/bash
sudo apt update -y
sudo apt install -y openjdk-17-jdk
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update -y
sudo apt install -y jenkins
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

### 🧮 Step 4: Add Variables (variables.tf)
```sh
variable "aws_region" {
  default = "us-east-1"
}
```

### 🧾 Step 5: Output Public IP (outputs.tf)
```sh
output "jenkins_url" {
  description = "Access Jenkins at this URL"
  value       = "http://${aws_instance.jenkins_server.public_ip}:8080"
}
```

### 🚀 Step 6: Run the Project
```sh
terraform init
terraform plan
terraform apply -auto-approve
```
After a few minutes, Terraform will create:
 - EC2 instance with Jenkins preinstalled
 - Security group open on port 8080
 - Output Jenkins URL

✅ Open:
```sh
http://<public_ip>:8080
```
and you’ll see the Jenkins setup page.

### 🧹 Step 7: Destroy the Resources (Cleanup)
When done:
```sh
terraform destroy -auto-approve
```

### 🧠 Concepts Covered in the Video
Infrastructure as Code (IaC) using Terraform
 - AWS provider setup
 - Security group management
 - Using user_data for automation
 - Terraform outputs
 - Clean resource teardown


