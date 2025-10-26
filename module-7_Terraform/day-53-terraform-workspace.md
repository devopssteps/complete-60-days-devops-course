# Day 53 : Terraform Workspaces Explained (with Hands-On Demo)

You’ll learn how to:
 - Use Terraform Workspaces to manage multiple environments (like dev, stage, prod)
 - Keep configurations clean and reusable
 - Avoid overwriting state files between environments

### 🧩 1️⃣ What is a Terraform Workspace?
By default, Terraform has one workspace — default.
A workspace is basically a separate state file — meaning each workspace can manage its own infrastructure independently using the same code.

For example:
 - workspace: dev → deploys EC2 instance dev-web
 - workspace: prod → deploys EC2 instance prod-web
All with the same Terraform code — only variables or workspace names change.

### 🧱 2️⃣ Hands-on Demo
🪜 Step 1: Create a simple Terraform project <br>
File: main.tf
```sh
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }

  required_version = ">= 1.6.0"
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "${terraform.workspace}-web"
  }
}
```
👉 Notice this part:
```sh
Name = "${terraform.workspace}-web"
```
That will dynamically name the instance based on your active workspace.

### 🪜 Step 2: Initialize Terraform
```sh
terraform init
```

### 🪜 Step 3: Check your current workspace
```sh
terraform workspace show
```
Output:
```default ```

### 🪜 Step 4: Create new workspaces
```sh
terraform workspace new dev
terraform workspace new prod
```
You can list all workspaces:
```terraform workspace list```
<br>
Output:
```sh
  default
* dev
  prod
```
(* shows the active one)

### 🪜 Step 5: Apply for dev environment
```sh
terraform apply -auto-approve
```
This will create an instance tagged as:
```sh
Name = dev-web
```

### 🪜 Step 6: Switch to another workspace
```sh
terraform workspace select prod
terraform apply -auto-approve
```
Now it will create a new EC2 instance tagged as:
```sh
Name = prod-web
```
✅ Both instances are separate — because each workspace has its own state file.
### 🧠 7️⃣ Where Terraform Stores Workspace States

Each workspace creates its own state file under:
```sh
terraform.tfstate.d/<workspace_name>/terraform.tfstate
```
So:
```sh
terraform.tfstate.d/dev/terraform.tfstate
terraform.tfstate.d/prod/terraform.tfstate
```
This is how Terraform keeps environments isolated but using the same code.

### 🧹 8️⃣ Clean Up
To destroy infrastructure for a specific environment:
```sh
terraform workspace select dev
terraform destroy -auto-approve

terraform workspace select prod
terraform destroy -auto-approve
```























