# Day-51: Terraform-Ansible-Integration

# The English version of the video documentation is given below, and you will also find the Bangla version afterward.

### Step 1: Terraform Code (```main.tf```)
```sh
provider "aws" {
  region = "us-east-1"
}

# Create key pair using your existing public key
resource "aws_key_pair" "deployer" {
  key_name   = "deployer-key"
  public_key = file("~/.ssh/id_rsa.pub")
}

# Create a security group to allow SSH & HTTP
resource "aws_security_group" "web_sg" {
  name        = "web_sg"
  description = "Allow SSH and HTTP"

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
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

# Launch EC2 instance
resource "aws_instance" "web" {
  ami                    = "ami-0018b373aba829819"  # Amazon Linux 2 AMI (example)
  instance_type          = "t2.micro"
  key_name               = aws_key_pair.deployer.key_name
  vpc_security_group_ids = [aws_security_group.web_sg.id]

  tags = {
    Name = "Terraform-Ansible-Demo"
  }

  # Wait and then call Ansible playbook
  provisioner "local-exec" {
    command = <<EOT
      echo "Waiting for instance to initialize..."
      sleep 60
      ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i '${self.public_ip},' --private-key ~/.ssh/id_rsa ansible/playbook.yml
    EOT
  }
}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

### Step 2: Ansible Playbook (```ansible/playbook.yml```)
If OS = Amazon linux 2
```sh
---
- name: Configure Apache on EC2
  hosts: all
  become: yes
  remote_user: ec2-user  # Amazon Linux default user (use ubuntu for Ubuntu AMIs)
  tasks:
    - name: Install Apache
      yum:
        name: httpd
        state: present

    - name: Start and enable Apache
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create simple index.html
      copy:
        dest: /var/www/html/index.html
        content: "<h1>Hello from Terraform + Ansible!</h1>"
```
If OS = Ubuntu
```sh
---
- hosts: all
  become: yes
  tasks:
    - name: Update apt packages
      apt:
        update_cache: yes

    - name: Install Apache
      apt:
        name: apache2
        state: present

    - name: Start Apache service
      service:
        name: apache2
        state: started
        enabled: yes

    - name: Create index.html
      copy:
        content: "<h1>Hello from Terraform + Ansible Integration!</h1>"
        dest: /var/www/html/index.html
```

### Step 3: Run Commands
```sh
terraform init
terraform apply -auto-approve
```
 - ✅ Terraform provisions the EC2 instance
 - ✅ Automatically triggers Ansible to configure it
 - ✅ Apache installed and web page available at:
http://<EC2_PUBLIC_IP>

### Step 4: Verify
Open your EC2 public IP in browser — you’ll see: <br>
👉 Hello from Terraform + Ansible Integration!

Summary of this demo: <br>
“Terraform handles what to create, and Ansible handles how to configure. <br>
Combine them to achieve full Infrastructure Automation!”

# Day-51-Bangla-version video documentaion I used.

### 📁 Directory Structure:
```sh
terrafrom-ansible-demo/
├── terraform/
│   ├── main.tf       
│   ├── variables.tf        
│   ├── outputs.tf         
├── ansible/
│   ├── hosts
│   ├── playbook.yml
└──deploy.sh
```
### Make sure installed and congured AWS credentails in your local pc
 - connected aws-vault
 - terraform
 - ansible

### Steps 1: create  a key pair
```sh
ssh=kegen -f ~/.ssh/devops
chmod 400 ~/.ssh/devops
```

### Steps 2: create main.tf 
$mkdir terraform && vi terraform/main.tf
terraform/main.tf
```sh
provider "aws" {
  region = "us-east-1"
}

resource "aws_key_pair" "deployer" {
  key_name   = "deployer-key"
  public_key = file("~/.ssh/devops.pub")
}

resource "aws_instance" "web" {
  ami           = "ami-0bbdd8c17ed981ef9" # Ubuntu
  instance_type = "t2.micro"
  key_name      = aws_key_pair.deployer.key_name

  tags = {
    Name = "Terraform-Ansible-Demo"
  }
}

output "public_ip" {
  value = aws_instance.web.public_ip
}
```

### Step 3: terraform/variables.tf (optional)
```sh
variable "region" {
  default = "us-east-1"
}
```

### Step 4: create ansible/hosts
$mkdir ansible && vi ansible/hosts
```sh
[web]
<public_ip> ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/devops
```
Replace <public_ip> with the output from Terraform.

### Step 5: create ansible/playbook.yml
```sh
- name: Configure web server
  hosts: web
  gather_facts: false
  become: true
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Ensure Apache is running
      service:
        name: apache2
        state: started
        enabled: yes

    - name: copy the file
      copy:
        src: ../index.html
        dest: /var/www/html/index.htm
```

### Step 6: create this index.html file
$echo welcome > index.html

### Step 7: create deploy.sh
```sh
#!/bin/bash

cd terraform
terraform init
terraform apply -auto-approve

PUBLIC_IP=$(terraform output -raw public_ip)

cd ../ansible
echo "[web]" > hosts
echo "$PUBLIC_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/devops" >> hosts
sleep 45
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i hosts playbook.yml
```

### Step 8: create destroy.sh
```sh
#!/bin/bash
cd terraform
terraform destroy -auto-approve
```




