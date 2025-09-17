# Day-49: EC2-VPC-Security-Groups
### Hands-on Demo Plan (EC2, VPC, Security Groups)
### Step 1: Create a VPC
 - Go to VPC → Create VPC → VPC Only.
 - Name: ```devops-vpc```, CIDR: ```10.0.0.0/16```.

### Step 2: Create Subnet
 - Inside your VPC, create a subnet: ```10.0.1.0/24```.
 - Select AZ: ```ap-south-1a``` (or your preferred).

### Step 3: Create Internet Gateway
 - Go to Internet Gateway → Create → Attach to ```devops-vpc```.

### Step 4: Create Route Table
 - Create a route table for public subnet.
 - Add route: ```0.0.0.0/0``` → Internet Gateway.
 - Associate with subnet.

### Step 5: Create Security Group
 - Create new SG → Name: devops-sg.
 - Inbound rules:
    - SSH → My IP
    - HTTP (80) → 0.0.0.0/0
    - HTTPS (443) → 0.0.0.0/0

### Step 6: Launch EC2 Instance
 - Go to EC2 → Launch Instance.
 - Name: devops-ec2.
 - AMI: Amazon Linux 2.
 - Instance type: t2.micro (free tier).
 - VPC: devops-vpc.
 - Subnet: devops-subnet.
 - Security group: devops-sg.
 - Key pair: new or existing.

### Step 7: Connect & Test
 - SSH into instance.
 - Install Apache:
 ```sh
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
echo "Hello from DevOps EC2 in VPC" | sudo tee /var/www/html/index.html
```
 - Copy public IP → open in browser → you’ll see your message 🎉.
