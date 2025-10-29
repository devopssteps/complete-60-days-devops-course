# Day-54: Terraform Cloud Explained with Hands-On Demo

### 🧠 1️⃣ What is Terraform Cloud?
Terraform Cloud is a SaaS (by HashiCorp) that:
 - Stores your Terraform state securely in the cloud (instead of local)
 - Runs Terraform remotely (no need to install Terraform locally)
 - Enables collaboration (teams, workspaces, version control)
 - Integrates directly with GitHub, GitLab, or Bitbucket
Essentially — it’s a central control hub for Terraform automation and state management.

### 🧩 2️⃣ Hands-on Demo: Setup Terraform Cloud with AWS Example
🪜 Step 1: Sign up for Terraform Cloud
<br>
Go to https://app.terraform.io <br>
 → Create a free account. <br>
Then:
   1. Create an Organization
   2. Create a Workspace
       - Choose Version Control Workflow
       - Connect to your GitHub Repo
     
### 🪜 Step 2: Prepare your local Terraform project
Create a folder:
```sh
mkdir terraform-cloud-demo && cd terraform-cloud-demo
```
Create these files:

File: ```main.tf```
```sh
terraform {
  cloud {
    organization = "your-org-name"

    workspaces {
      name = "aws-demo"
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

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "terraform-cloud-demo"
  }
}
```

### 🪜 Step 3: Initialize Terraform
```sh
terraform init
```
✅ Output:
```sh
Initializing Terraform Cloud...
```
Terraform automatically connects to your Terraform Cloud workspace.

### 🪜 Step 4: Set up AWS credentials
Go to: <br>
👉 Terraform Cloud → Workspace → Variables → Add
| Key                   | Value           | Category             |
| --------------------- | --------------- | -------------------- |
| AWS_ACCESS_KEY_ID     | your-access-key | Environment Variable |
| AWS_SECRET_ACCESS_KEY | your-secret-key | Environment Variable |

### 🪜 Step 5: Push Code to GitHub
```sh
git init
git add .
git commit -m "Terraform Cloud demo"
git branch -M main
git remote add origin https://github.com/<yourname>/terraform-cloud-demo.git
git push -u origin main
```
Terraform Cloud will automatically detect the new commit and start a plan and apply run in the UI.

### 🪜 Step 6: Monitor the Run
In Terraform Cloud Dashboard:
 - You’ll see the run status (Plan → Apply)
 - You can view logs, approve runs, and see outputs directly.

### 🪜 Step 7: Verify in AWS
Go to AWS Console → EC2 → Instances → You’ll see:
```sh
Name: terraform-cloud-demo
```

### 🧹 Step 8: Clean up
From Terraform Cloud dashboard:
<br>
Click Actions → Queue destroy plan → Confirm & Apply

### 🧠 3️⃣ Key Advantages of Terraform Cloud
| Feature              | Benefit                                 |
| -------------------- | --------------------------------------- |
| Remote state storage | Secure and versioned state management   |
| Remote execution     | No need to install Terraform locally    |
| Team collaboration   | Access control and approvals            |
| VCS integration      | Auto runs on commits                    |
| Cost estimation      | Built-in Terraform plan cost estimation |


