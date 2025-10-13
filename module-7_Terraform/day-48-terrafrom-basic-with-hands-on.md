# Day-48: What is IaC? Terraform Basics with hands on demo  

### Step 1: What is IaC (Infrastructure as Code)?
 - IaC means managing and provisioning infrastructure using code instead of manual setup.
 - Tools like Terraform, CloudFormation, and Pulumi help automate infrastructure.
 - Benefits: repeatability, scalability, version control, automation.

### Step 2: Install Terraform
```sh
# On Linux/macOS
sudo apt-get update && sudo apt-get install -y wget unzip
wget https://releases.hashicorp.com/terraform/1.9.0/terraform_1.9.0_linux_amd64.zip
unzip terraform_1.9.0_linux_amd64.zip
sudo mv terraform /usr/local/bin/
terraform -version
```

### Step 3: Create a Terraform Project
 1. Create a folder:
```sh
mkdir terraform-basics && cd terraform-basics
```
 2. Create a ```main.tf``` file:
```sh
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "my_ec2" {
  ami           = "ami-0c55b159cbfafe1f0"   # Amazon Linux 2 AMI
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-EC2"
  }
}
```

### Step 4: Initialize Terraform
```sh
terraform init
```

### Step 5: Plan & Apply
```sh
terraform plan
terraform apply -auto-approve
```
✅ This will launch an EC2 instance in AWS using just code.

### Step 6: Destroy (Clean up resources)
```sh
terraform destroy -auto-approve
```
