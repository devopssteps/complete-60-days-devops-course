# Day-45: Route53, ELB, SSL

### Hands-on Demo: AWS Route 53 + ELB + SSL
Architecture Overview
 - Deploy an EC2 (or ECS service) with a sample web app.
 - Put the app behind an Application Load Balancer (ALB).
 - Use Route 53 to map a custom domain to the ALB.
 - Enable SSL certificate (HTTPS) using AWS Certificate Manager (ACM).

### Step 1: Create a Sample Web App on EC2
```sh
sudo yum install -y httpd
echo "Hello from AWS with HTTPS" | sudo tee /var/www/html/index.html
sudo systemctl start httpd
sudo systemctl enable httpd
```
✅ App runs on port 80.

### Step 2: Create Application Load Balancer (ALB)
  1. Go to EC2 → Load Balancers → Create Load Balancer → Application Load Balancer.
  2. Choose Internet-facing, HTTP/HTTPS.
  3. Add your EC2 instance(s) to a Target Group.
  4. Confirm: Access ALB DNS (e.g., http://my-alb-123456.elb.amazonaws.com).

### Step 3: Get a Free SSL Certificate
  1. Go to AWS Certificate Manager (ACM).
  2. Request a certificate for your domain (e.g., example.com, www.example.com).
  3. Validate domain ownership via DNS (Route 53 makes it 1-click).

### Step 4: Attach SSL to ALB
  1. Edit ALB Listeners → Add HTTPS (port 443).
  2. Select the SSL certificate from ACM.
  3. Forward traffic to your Target Group (EC2 app).
✅ Now your app works on https://my-alb-123456.elb.amazonaws.com

### Step 5: Configure Route 53
  1. Buy or transfer a domain to Route 53 (e.g., myawsdemo.com).
  2. Create a Hosted Zone.
  3. Create a Record Set:
     - Type: A (Alias)
     - Alias Target: Your ALB DNS name
     ✅ Now your domain (https://myawsdemo.com) points to ALB with SSL.


### Step 6: Test
 - Open browser → https://myawsdemo.com
 - Confirm 🔒 Secure (SSL) and load balancing works.
