
# day-66-Script-to-stop-all-EC2-instances-at-night-and-start-them-in-the-morning


# 🚀 Auto Stop & Start EC2 Instances (Cost Optimization)

### 🎯 Goal

* Stop **running EC2 instances at night**
* Start them **in the morning**
* Save AWS cost 💰
* Fully automated using Python (Boto3)

---

# 🧱 Prerequisites (Hands-On Setup)

### 1️⃣ Install Required Tools

```bash
sudo apt update
sudo apt install -y python3 python3-pip
pip3 install boto3
```

### 2️⃣ Configure AWS Credentials

```bash
aws configure
```

Provide:

* AWS Access Key
* Secret Key
* Region (e.g. `us-east-1`)
* Output format: `json`

> ✅ **Recommended in production:** Use IAM Role (EC2 / Lambda)

---

# 🔐 Required IAM Permission

Attach this policy to your IAM user/role:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "ec2:StartInstances",
        "ec2:StopInstances"
      ],
      "Resource": "*"
    }
  ]
}
```

---

# 🟢 SCRIPT 1: Stop EC2 Instances at Night

📁 **File:** `stop_ec2.py`

```python
import boto3

ec2 = boto3.client("ec2", region_name="us-east-1")

def stop_instances():
    response = ec2.describe_instances(
        Filters=[
            {"Name": "instance-state-name", "Values": ["running"]}
        ]
    )

    instance_ids = []

    for reservation in response["Reservations"]:
        for instance in reservation["Instances"]:
            instance_ids.append(instance["InstanceId"])

    if instance_ids:
        ec2.stop_instances(InstanceIds=instance_ids)
        print("Stopped instances:", instance_ids)
    else:
        print("No running instances found")

if __name__ == "__main__":
    stop_instances()
```

### ▶ Run

```bash
python3 stop_ec2.py
```

---

# 🟢 SCRIPT 2: Start EC2 Instances in the Morning

📁 **File:** `start_ec2.py`

```python
import boto3

ec2 = boto3.client("ec2", region_name="us-east-1")

def start_instances():
    response = ec2.describe_instances(
        Filters=[
            {"Name": "instance-state-name", "Values": ["stopped"]}
        ]
    )

    instance_ids = []

    for reservation in response["Reservations"]:
        for instance in reservation["Instances"]:
            instance_ids.append(instance["InstanceId"])

    if instance_ids:
        ec2.start_instances(InstanceIds=instance_ids)
        print("Started instances:", instance_ids)
    else:
        print("No stopped instances found")

if __name__ == "__main__":
    start_instances()
```

### ▶ Run

```bash
python3 start_ec2.py
```

---

# ⏰ AUTOMATION: Schedule Using CRON (Hands-On)
### Collect the log of our cron task
Create a folder
```sh
mkdir ~/cron_log
```
Give the file execute permision 
```sh
sudo chmod +x /home/rajiv/youtube/python/day6/stop_ec2.py
sudo chmod +x /home/rajiv/youtube/python/day6/start_ec2.py
```

### 🕘 Stop at **9 PM**

```bash
crontab -e
```

Add:

```bash
0 21 * * * /usr/bin/python3 /home/rajiv/stop_ec2.py >> /home/rajiv/cron_log/ec2_stop.log 2>&1
```

### 🌅 Start at **8 AM**

```bash
0 8 * * * /usr/bin/python3 /home/rajiv/start_ec2.py >> /home/rajiv/cron_log/ec2_start.log 2>&1       
```
check crontab 
```sh
crontab -l
```

# 🧠 BEST PRACTICE (Advanced – Optional)

### ✅ Use Tags (Only Stop Tagged Instances)

Example filter:

```python
Filters=[
  {"Name": "tag:AutoStop", "Values": ["true"]},
  {"Name": "instance-state-name", "Values": ["running"]}
]
```

This avoids stopping **production or critical servers**.

---

# 📌 DevOps Interview Question 

**Q:** Why use Boto3 instead of AWS Console?

**A:**

> Boto3 allows automation, repeatability, cost optimization, and integration with CI/CD pipelines.

---
