# Day-44: CloudWatch Logs + Monitoring

### Hands-on Demo: AWS CloudWatch Logs + Monitoring
### Step 0: Prerequisites
 - AWS account
 - EC2 instance (or ECS/EKS app) already running
 - IAM Role with permissions:
     - CloudWatchAgentServerPolicy
     - CloudWatchLogsFullAccess
  
### Step 1: Install the CloudWatch Agent
 - For Amazon Linux 2, CentOS, or RHEL
```sh
sudo yum install amazon-cloudwatch-agent
```
 - For Ubuntu/Debian:
 ```sh
 sudo apt-get install amazon-cloudwatch-agent
 ```
 - For Windows, download the agent MSI from AWS documentation.
  https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/download-CloudWatch-Agent-on-EC2-Instance-commandline-first.html

### setp 2: Create a CloudWatch Agent Configuration File
Create a file with the foloowing code to this location  ```/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json```
```sh
{
  "metrics": {
    "metrics_collected": {
      "mem": {
        "measurement": [
          "mem_used_percent",
          "mem_available",
          "mem_total"
        ],
        "metrics_collection_interval": 60
      }
    }
  }
}
```

### Step 3: Configure IAM Role for the EC2 Instance
 -  First create a policy with the following permission for ec2
 ```sh
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "cloudwatch:PutMetricData",
        "ssm:ListCommandInvocations",
        "ssm:ListCommands",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:CreateLogGroup"
      ],
      "Resource": "*"
    }
  ]
}
```
 -  Second create a role for ec2 with this policy
 -  Third, attach this role to the ec2

### Step 4: Start the CloudWatch Agent
```sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
-a fetch-config \
-m ec2 \
-c file:/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json \
-s
```

### Step 5:Verify the Metrics in CloudWatch
•	Go to the CloudWatch Console > All metrics > CWAgent > host > 
 	You should see the memory metrics like mem_used_percent, mem_available, and mem_total.


### Stop the CloudWatch Agent
```sh
sudo systemctl stop amazon-cloudwatch-agent
```

# Alerm create in cloud watch

CloudWatch > Alarms > Create Alearm > Click Select metric > Copy your ec2 intance id and search it from alearm > Select Per-Instance Metrics > Select CPUUtilization > 
Click select metric > keep everything same just put 95 in Define the threshold value > Next > in Notification choose your exesting sNS topic >
Click EC2 action > Select Stop this instanc > give a name if alearm > Next > Click Create Alearm 

### Now run the script in cloud shell
```sh
aws cloudwatch set-alarm-state --alarm-name private_ec2_cpu_use_high1 --state-value ALARM --state-reason "Testing"
```
NB: jsut change your alarm name in this script


