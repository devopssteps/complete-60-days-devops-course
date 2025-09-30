# Auto Scaling & Load Balancer Setup

“Auto Scaling & Load Balancer Setup” is one of the most searched AWS practicals, because people want to see how to handle traffic spikes automatically.

# 🚀 Hands-on Demo: Auto Scaling & Load Balancer Setup (AWS)
We’ll use a Node.js App in EC2 with an Application Load Balancer (ALB) and an Auto Scaling Group (ASG).

### 📌 Step 1: Simple Node.js App
```server.js```
```sh
const http = require("http");
const os = require("os");

const PORT = process.env.PORT || 3000;

const requestHandler = (req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end(`Hello from Node.js App 🚀\nServed from host: ${os.hostname()}`);
};

const server = http.createServer(requestHandler);

server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```
### 📌 Step 2: Create an AMI
 1. Launch one EC2 instance, install Node.js, and run the app.
 2. Create an AMI from that instance → use this AMI in your Auto Scaling Group.

### 📌 Step 3: Application Load Balancer (ALB)
 1 . Go to EC2 → Load Balancers → Create Load Balancer → Application Load Balancer.
 2. Add a listener on HTTP (80).
 3. Create a Target Group → Register EC2 instances.
 4. Test: http://<ALB-DNS-NAME> should show “Hello from Node.js App 🚀”.

### 📌 Step 4: Auto Scaling Group (ASG)
 1. Go to EC2 → Auto Scaling Groups → Create.
 3. Select Launch Template / Launch Configuration → Choose the AMI created in Step 2.
 3. Attach the Target Group (so new instances auto-register with ALB).
 4. Configure scaling policy:
    - Min: 2 instances
    - Max: 5 instances
    - Desired: 2 instances
    - Scaling Policy: Add when CPU > 60%, Remove when CPU < 30%.

### 📌 Step 5: Testing Auto Scaling
 1. Open ALB DNS in browser → See app working.
 2. Run a load test (e.g., with ApacheBench):
```sh
ab -n 10000 -c 100 http://<ALB-DNS-NAME>/
```
 3. Watch EC2 Dashboard → Auto Scaling Group should launch more instances.
 4. When load decreases, instances terminate automatically.

