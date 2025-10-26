# Day-54: Terraform Cloud

# Hands-on Demo: Terraform Cloud 

### Step 1: Create Terraform Cloud Account
 1. Go to https://app.terraform.io
 2. Sign up (free tier is fine)
 3. Create an Organization — e.g., ```devops-steps-org```
 4. Create your first Workspace — e.g., ```terraform-aws-dev```

### Step 2: Configure Terraform CLI Authentication
From your local terminal, run:
```sh
terraform login
```
You’ll be prompted to open a URL in your browser and paste the token into your CLI.

### Step 3: Terraform Configuration (main.tf)
```sh
terraform {
  cloud {
    organization = "devops-steps-org"

    workspaces {
      name = "terraform-aws-dev"
    }
  }

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "demo" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "TerraformCloudDemo"
  }
}

output "public_ip" {
  value = aws_instance.demo.public_ip
}
```

### Step 4: Initialize Terraform Cloud Backend
```sh
terraform init
```
You’ll see:
```sh
Initializing Terraform Cloud...
Connected to organization: devops-steps-org
Workspace: terraform-aws-dev
```

### Step 5: Apply the Infrastructure
```sh
terraform apply -auto-approve
```
 - Terraform will run the plan in Terraform Cloud, not locally.
 - You can log in to Terraform Cloud → Workspace → “terraform-aws-dev” →
 - check the Runs, Plan, and Apply logs.

### Step 6: Create Multiple Workspaces
In Terraform Cloud, click Workspaces → New Workspace
<br>
Create:
 - ```terraform workspace list```
 - ```terraform workspace select prod```
 - ```terraform workspace new stage```
To switch between them in CLI:
```sh
terraform workspace list
terraform workspace select prod
terraform workspace new stage
```
Each workspace will have its own Terraform state, allowing you to manage different environments safely.

### Step 7: Verify Resources
After applying, go to AWS EC2 Console → you’ll see an instance named
 - ✅ TerraformCloudDemo
Switch workspace → reapply → new EC2 instance will appear (separate environment).


### Terraform Cloud Best Practices
 - ✅ Always use Terraform Cloud for remote state & collaboration
 - ✅ Use separate workspaces for dev, stage, and prod
 - ✅ Enable cost estimation & run approvals
 - ✅ Store sensitive variables as environment variables
 - ✅ Integrate with VCS (GitHub) for automated runs

### Summary
 - Terraform Cloud is the future of team-based infrastructure automation —
 - with remote state, workspaces, and collaboration all built-in!”


