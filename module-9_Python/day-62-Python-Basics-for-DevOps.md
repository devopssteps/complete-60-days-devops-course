# Day-62: Why Python is Important for a DevOps Engineer & Python Basics for DevOps

### In this video we will cover the folloing topic
 - Why Python is Important for a DevOps Engineer
 - Install Python & set up VS Code or PyCharm.
 - Variables, data types, input/output.
 - Operators, strings, lists, dictionaries.
 - Loops (for, while), conditionals (if-else).
 - ✅ Mini Project: Write a script to print all files in a directory & List all .log files

## 🔹 Why Python is Important for a DevOps Engineer

### 1️⃣ Automation (Biggest Reason)

DevOps = **automation everywhere**

Python helps you automate:

* Server health checks
* Log analysis
* Backup scripts
* User management
* Cleanup jobs
* Monitoring tasks

Example:

```python
import os
os.system("df -h")
```

This is much **cleaner and powerful than bash** for complex logic.

---

### 2️⃣ Infrastructure Automation & Cloud

Most DevOps tools are built with Python or have Python SDKs.

Python is used with:

* **AWS (boto3)**
* Azure SDK
* GCP SDK

Example:

```python
import boto3
ec2 = boto3.client('ec2')
ec2.describe_instances()
```

👉 This helps when:

* Terraform is not enough
* You need custom logic
* You build internal automation tools

---

### 3️⃣ CI/CD Pipelines (Jenkins, GitHub Actions)

Python is commonly used in pipelines for:

* Build scripts
* Test automation
* Code quality checks
* Deployment logic

Example in Jenkins:

```bash
python3 deploy.py
```

---

### 4️⃣ Monitoring & Observability

Python is used to:

* Write exporters
* Custom Prometheus metrics
* Log parsers
* Alert automation

Example:

* Parse application logs
* Send alerts to Slack/Email

---

### 5️⃣ Configuration & Tooling Ecosystem

Many DevOps tools **expect Python knowledge**:

* Ansible modules → Python-based
* OpenStack → Python
* Kubernetes operators & scripts
* Helm hooks (sometimes)

Even **Ansible troubleshooting becomes easier** if you know Python basics.

---

## 🔹 Python vs Bash for DevOps

| Task            | Bash | Python |
| --------------- | ---- | ------ |
| Simple commands | ✅    | ❌      |
| Complex logic   | ❌    | ✅      |
| Error handling  | Weak | Strong |
| Cloud SDK       | ❌    | ✅      |
| Large scripts   | Hard | Easy   |

👉 **Best DevOps engineers know both**

---

## 🔹 How Much Python Depth Do You REALLY Need?

### ❌ You DO NOT need:

* Django / Flask (unless DevOps + SRE role)
* Advanced OOP design patterns
* Data science / ML
* Competitive programming

---

### ✅ Python Depth Required for DevOps (Must Know)

#### 🔸 Level 1 – Basics (Mandatory)

* Variables
* Data types
* If-else
* Loops
* Functions
* Input/output

✔ Enough to read & modify scripts

---

#### 🔸 Level 2 – Practical DevOps Python (Very Important)

* Lists, dictionaries
* File handling
* Exceptions (try/except)
* Modules & packages
* Virtual environments (`venv`)
* JSON & YAML parsing
* OS & subprocess modules

Example:

```python
import json
with open("config.json") as f:
    data = json.load(f)
```

---

#### 🔸 Level 3 – DevOps Integrations (Career Boost)

* REST APIs (`requests`)
* Cloud SDKs (boto3)
* Environment variables
* Logging
* CLI tools using `argparse`
* Writing reusable scripts

Example:

```python
import requests
requests.get("https://api.github.com")
```

## 🔹 Python Skill Expectation by DevOps Role

| Role          | Python Level            |
| ------------- | ----------------------- |
| Junior DevOps | Basics + scripting      |
| Mid DevOps    | Automation + APIs       |
| Senior DevOps | Tool building           |

---

## 🔹 Real Career Advantage of Python in DevOps

 - ✔ Higher salary
 - ✔ Easier automation
 - ✔ Better troubleshooting
 - ✔ Stronger CI/CD pipelines
 - ✔ Easier cloud work
 - ✔ Better job opportunities abroad



# Python Basics for DevOps
## Python Hands-On Demo (From Zero)

### Install Python (Ubuntu / WSL / Linux)
```sh
sudo apt update
sudo apt install python3 python3-pip -y
```
Check if Python is already installed
```sh
python3 --version
```
Run Python shell:
```sh
python3
```

### 1️⃣ Variables in Python (Hands-On)
```sh
name = "Rajiv"
age = 30
salary = 50000
```
Print variables:
```sh
print(name)
print(age)
print(salary)
```
✔ Python automatically detects variable type
✔ No need to define data type

### 2️⃣ Data Types (Most Important)
```sh
name = "DevOps"       # string
age = 25             # int
price = 99.99        # float
is_active = True     # boolean
```
Check data type:
```sh
print(type(name))
print(type(age))
print(type(price))
print(type(is_active))
```

### 3️⃣ Input & Output (User Interaction)
```sh
username = input("Enter your name: ")
print("Hello", username)
```
Input with number:
```sh
age = int(input("Enter your age: "))
print("Your age is", age)
```
input() always returns string, so convert if needed.

### 4️⃣ Operators (Hands-On)
🔹 Arithmetic Operators
```sh
a = 10
b = 3

print(a + b)   # addition
print(a - b)   # subtraction
print(a * b)   # multiplication
print(a / b)   # division
print(a % b)   # modulus
```
🔹 Comparison Operators
```sh
print(a > b)
print(a < b)
print(a == b)
print(a != b)
```
### 5️⃣ Strings (Very Important)
```sh
message = "Welcome to Python"
print(message)
```
String operations:
```sh
print(message.upper())
print(message.lower())
print(message[0])
print(len(message))
```
String formatting:
```sh
name = "Rajiv"
print(f"Hello {name}, welcome to DevOps")
```

### 6️⃣ Lists (Used A LOT in DevOps)
```sh
tools = ["Docker", "Kubernetes", "Jenkins"]
print(tools)
```
Access list items:
```sh
print(tools[0])
```
Add item:
```sh
tools.append("Terraform")
print(tools)
```
Loop list:
```sh
for tool in tools:
    print(tool)
```
### 7️⃣ Dictionaries (Key–Value Data)
```sh
server = {
    "name": "jenkins-server",
    "ip": "192.168.1.10",
    "status": "running"
}
```
Access value:
```sh
print(server["ip"])
```
Loop dictionary:
```sh
for key, value in server.items():
    print(key, ":", value)
```

### 8️⃣ Conditionals (if-else)
```sh
age = 18

if age >= 18:
    print("You are eligible")
else:
    print("You are not eligible")
```
Multiple conditions:
```sh
marks = 75

if marks >= 80:
    print("Grade A")
elif marks >= 60:
    print("Grade B")
else:
    print("Grade C")
```
### 9️⃣ Loops (for & while)
🔹 for loop
```sh
for i in range(1, 6):
    print(i)
```
Loop list:
```sh
tools = ["Docker", "Kubernetes", "Terraform"]

for tool in tools:
    print("Learning", tool)
```
🔹 while loop
```sh
count = 1

while count <= 5:
    print(count)
    count += 1
```
✅ Simple DevOps Tool Checker
```sh
tools = ["Docker", "Kubernetes", "Jenkins"]

user_tool = input("Enter a DevOps tool name: ")

if user_tool in tools:
    print(user_tool, "is available")
else:
    print(user_tool, "is not available")
```
# Hand-on Demo
### see current directory
```sh
import os

path = "."   # current directory

files = os.listdir(path)

for file in files:
    print(file)
```


### List all .log files (Very Practical)
```sh
import os

path = "/var/log"

for file in os.listdir(path):
    if file.endswith(".log"):
        print(file)
```




# MOST COMMON and IMPORTANT interview questions interviewers ask for a **DevOps Engineer role**
## 1️⃣ Why should a DevOps Engineer know Python?

**Best Interview Answer:**

> Python helps DevOps engineers automate repetitive tasks such as CI/CD pipelines, infrastructure provisioning, cloud operations, monitoring, and notifications. It is easy to learn, has a rich ecosystem of libraries, and integrates well with tools like Jenkins, Docker, Kubernetes, AWS, and APIs.

**Points to mention:**

* Automation & scripting
* Cloud automation (AWS Boto3)
* CI/CD integration
* Monitoring & alerting
* Better than bash for complex logic

---

## 2️⃣ Where have you used Python in real DevOps scenarios?

**Best Interview Answer:**

> I have used Python in Jenkins pipelines to automate Docker builds, deployments, and send Slack or email notifications. I have also used Python for AWS automation using Boto3, such as managing EC2 instances and interacting with APIs. Python is especially useful when shell scripting becomes complex.

**Real-world examples interviewers like:**

* Jenkins + Python scripts
* Slack webhook notifications
* AWS automation with Boto3
* Kubernetes API interaction

---

## 3️⃣ What level of Python knowledge is required for a DevOps Engineer?

**Best Interview Answer:**

> A DevOps engineer does not need to be an expert Python developer. Basic to intermediate knowledge is enough, such as variables, data types, loops, functions, modules, exception handling, working with APIs, and reading environment variables.

**Must-know Python basics for DevOps:**

* Variables & data types
* If-else, loops
* Functions & modules
* File handling
* `requests`, `subprocess`, `boto3`
* Environment variables

---

## 🎯 Bonus One-Line Interview Tip

> “DevOps engineers use Python to glue different tools together and automate workflows.”

---










































































