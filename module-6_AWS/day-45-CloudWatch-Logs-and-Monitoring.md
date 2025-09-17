# Day-45: CloudWatch Logs + Monitoring

### Hands-on Demo: AWS CloudWatch Logs + Monitoring
### Step 1: Prerequisites
 - AWS account
 - EC2 instance (or ECS/EKS app) already running
 - IAM Role with permissions:
     - CloudWatchAgentServerPolicy
     - CloudWatchLogsFullAccess
  
### Step 2: Install & Configure CloudWatch Agent on EC2
 1. Connect to EC2:
```sh
ssh -i mykey.pem ec2-user@<EC2_IP>
```
 2. Install CloudWatch agent (Amazon Linux 2):
```sh
sudo yum install -y amazon-cloudwatch-agent
``` 
 3. Create CloudWatch config file:
  ```/opt/aws/amazon-cloudwatch-agent/bin/config.json```
 ```sh
 {
  "logs": {
    "logs_collected": {
      "files": {
        "collect_list": [
          {
            "file_path": "/var/log/messages",
            "log_group_name": "my-ec2-logs",
            "log_stream_name": "{instance_id}"
          }
        ]
      }
    }
  }
}
```
 4. Start the agent:
```sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
    -a fetch-config \
    -m ec2 \
    -c file:/opt/aws/amazon-cloudwatch-agent/bin/config.json \
    -s
```
✅ Now logs are streaming to CloudWatch. 

### Step 3: View Logs in CloudWatch
 - Go to AWS Console → CloudWatch → Log Groups → my-ec2-logs
 - Select Log Stream and check real-time logs

### Step 4: Create Metrics from Logs (Custom Monitoring)
 1. In Log Group → Create Metric Filter
 - Example: detect “ERROR” keyword
 - Pattern:
```sh
?ERROR
```
 - Metric Name: ```ErrorCount```
 2. This creates a Custom Metric in CloudWatch. 

### Step 5: Create Alarms for Monitoring
 1. Go to CloudWatch → Alarms → Create Alarm
 2. Select custom metric ```ErrorCount```
 3. Set threshold (e.g., > 5 errors in 5 minutes)
 4. Connect SNS topic → send email notification
✅ Now you’ll get alerts when logs contain errors!

### Step 6: Dashboard for Monitoring
 - Create a CloudWatch Dashboard
 - Add CPU Utilization, Memory, and ErrorCount
 - Monitor everything in one place

