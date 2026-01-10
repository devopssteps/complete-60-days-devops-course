# day-65-AWS-with-Python-Boto3-Basics

# ☁️ AWS with Python (Boto3 Basics) – Hands-On DevOps Demo

## 🎯 Goal

Learn how to:

* Use **Python + Boto3** to interact with AWS
* Configure AWS credentials
* Work with **EC2 & S3**
* Automate AWS tasks using Python

---

## 🔧 Prerequisites

* AWS Account
* IAM User with programmatic access
* Python 3 installed
* Internet access

---

## 🔹 What is Boto3?

**Boto3** is the **official AWS SDK for Python**.

DevOps use cases:

* Create EC2 instances
* Manage S3 buckets
* Automate AWS resources
* Use in Jenkins pipelines
* Infrastructure automation (alternative to CLI)

---

## 🔹 Install Boto3

```bash
pip install boto3
```

Verify:

```bash
python3 -c "import boto3; print('Boto3 installed')"
```

---

## 🔹 Configure AWS Credentials (IMPORTANT)

### Option 1: AWS CLI (Recommended)

```bash
aws configure
```

Enter:

```
AWS Access Key ID
AWS Secret Access Key
Region (e.g. us-east-1)
Output format (json)
```

Credentials stored in:

```
~/.aws/credentials
```

---

### Option 2: Environment Variables

```bash
export AWS_ACCESS_KEY_ID=xxxx
export AWS_SECRET_ACCESS_KEY=yyyy
export AWS_DEFAULT_REGION=us-east-1
```

---

## 🔹 Demo 1: First Boto3 Script – List S3 Buckets

### 📄 `list_s3.py`

```python
import boto3

s3 = boto3.client("s3")

response = s3.list_buckets()

print("S3 Buckets:")
for bucket in response["Buckets"]:
    print("-", bucket["Name"])
```

Run:

```bash
python3 list_s3.py
```

---

## 🔹 Demo 2: Create an S3 Bucket (Automation Example)

### 📄 `create_bucket.py`

```python
import boto3

region = "us-east-1"
bucket_name = "my-devops-demo-bucket-12345"

s3 = boto3.client("s3", region_name=region)

s3.create_bucket(Bucket=bucket_name)

print("Bucket created:", bucket_name)
```

⚠️ Bucket name must be **globally unique**.

---

## 🔹 Demo 3: Upload a File to S3

### 📄 `upload_file.py`

```python
import boto3

s3 = boto3.client("s3")

s3.upload_file(
    Filename="test.txt",
    Bucket="my-devops-demo-bucket-12345",
    Key="test.txt"
)

print("File uploaded to S3")
```

---

## 🔹 Demo 4: EC2 Automation – List EC2 Instances

### 📄 `list_ec2.py`

```python
import boto3

ec2 = boto3.client("ec2")

response = ec2.describe_instances()

for reservation in response["Reservations"]:
    for instance in reservation["Instances"]:
        print("Instance ID:", instance["InstanceId"],
              "| State:", instance["State"]["Name"])
```

---

## 🔹 Demo 5: Create EC2 Instance (Demo Only)

⚠️ **This may incur cost** – explain clearly in video.

```python
import boto3

ec2 = boto3.resource("ec2")

instance = ec2.create_instances(
    ImageId="ami-0abcdef1234567890",  # Amazon Linux AMI
    MinCount=1,
    MaxCount=1,
    InstanceType="t2.micro",
    KeyName="my-keypair"
)

print("EC2 instance created:", instance[0].id)
```

---

## 🔹 Demo 6: Error Handling (Important)

```python
import boto3
from botocore.exceptions import ClientError, NoCredentialsError

try:
    # Create S3 client FIRST
    s3 = boto3.client("s3")

    # Then call AWS API
    response = s3.list_buckets()

    print("S3 Buckets:")
    for bucket in response["Buckets"]:
        print("-", bucket["Name"])

except NoCredentialsError:
    print("AWS credentials not found. Run aws configure")

except ClientError as e:
    print("AWS Client Error:", e)

except Exception as e:
    print("Unknown Error:", e)

```

---

## 🔹 DevOps Best Practices

Explain clearly:

* ❌ Never hardcode AWS keys
* ✅ Use IAM roles in EC2/EKS
* ✅ Use environment variables
* ✅ Least privilege IAM policies

---

## 🚀 Mini Project: AWS Resource Info Tool

### 🎯 Features

* List S3 buckets
* List EC2 instances
* Show region info

### 📄 `aws_info.py`

```python
import boto3

print("AWS Region:", boto3.Session().region_name)

s3 = boto3.client("s3")
ec2 = boto3.client("ec2")

print("\nS3 Buckets:")
for b in s3.list_buckets()["Buckets"]:
    print("-", b["Name"])

print("\nEC2 Instances:")
for r in ec2.describe_instances()["Reservations"]:
    for i in r["Instances"]:
        print(i["InstanceId"], i["State"]["Name"])
```

---

## Interview Question 

**Q:** How do you automate AWS using Python?
**A:**

> Using Boto3 SDK with proper IAM permissions and credentials.

---
