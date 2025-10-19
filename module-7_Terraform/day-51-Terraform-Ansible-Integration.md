# Day-51: Terraform-Ansible-Integration

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




