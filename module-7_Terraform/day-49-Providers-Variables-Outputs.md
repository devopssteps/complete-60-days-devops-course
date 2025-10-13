# Day-49: Providers, Variables, Outputs

## 🔹 Hands-on Example: Providers, Variables, Outputs
### Step 1: What are Providers, Variables, and Outputs?
 - Providers: Plugins Terraform uses to talk to cloud platforms (AWS, Azure, GCP, etc).
 - Variables: Let you make configurations reusable & dynamic.
 - Outputs: Help you print or share useful information after deployment (like IPs, DNS names).

### Step 2: Project Setup
Create a folder:
```sh
mkdir terraform-providers-vars-outputs && cd terraform-providers-vars-outputs
```

### Step 3: Define Provider (```provider.tf```)
```sh
provider "aws" {
  region = var.aws_region
}
```

### Step 4: Define Variables (variables.tf)
```sh
variable "aws_region" {
  description = "The AWS region to deploy resources"
  default     = "us-east-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  default     = "t2.micro"
}

variable "ec2_name" {
  description = "Tag for EC2 instance"
  default     = "MyTerraformEC2"
}
```

### Step 5: Define Resource (```main.tf```)
```sh
resource "aws_instance" "my_ec2" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type

  tags = {
    Name = var.ec2_name
  }
}
```

### Step 6: Define Outputs (```outputs.tf```)
```sh
output "ec2_instance_id" {
  description = "The ID of the EC2 instance"
  value       = aws_instance.my_ec2.id
}

output "ec2_public_ip" {
  description = "Public IP of the EC2 instance"
  value       = aws_instance.my_ec2.public_ip
}
```

### Step 7: Run Terraform
```sh
terraform init
terraform plan
terraform apply -auto-approve
```
✅ After apply, Terraform will print the EC2 instance ID and Public IP from outputs.tf.

### Step 8: Destroy (Clean-up)
```sh
terraform destroy -auto-approve
```
