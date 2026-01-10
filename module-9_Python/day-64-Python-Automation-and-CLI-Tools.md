# Day-64: 🐍 Python Automation & CLI Tools (Hands-On for DevOps)

## 🎯 Goal

Automate common **DevOps tasks** using Python scripts and build **CLI tools** like real-world DevOps engineers.

---

## 🔧 Prerequisites

* Linux / Ubuntu / WSL (you already use WSL 👍)
* Python 3 installed
* Basic Linux commands

Check Python:

```bash
python3 --version
```

---

## 🔹 Why Python for DevOps Automation?

Python is used in DevOps for:

* Server health checks
* Log monitoring
* Disk & memory monitoring
* Backup automation
* CI/CD scripting
* Cloud & Kubernetes automation

---
## 🔹 Run a Simple Command (ls -l)
```sh
import subprocess

subprocess.run(["ls", "-l"])
```
## 🔹 Capture Command Output like ```uptime```
```sh
import subprocess

result = subprocess.run(
    ["uptime"],
    capture_output=True,
    text=True
)

print("Command Output:")
print(result.stdout)
```
## Docker Command Automation
See docker container:
```sh
subprocess.run(["docker", "ps"])
```
Check aws s3 bucket:
```sh
subprocess.run(["aws", "s3", "ls"])
```
Same way we can use kubectl relted k8s command

## Combine with CLI Arguments (Real Tool)
```sh
import subprocess
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("--cmd", required=True)
args = parser.parse_args()

subprocess.run(args.cmd.split())
```
Run
```sh
python3 run_cmd.py --cmd "df -h"
```

## 🔹 Simple Automation Script (Disk Usage)

### 📌 Task:

Check disk usage and alert if usage is above 80%.

### 📝 Script: `disk_check.py`

```python
import shutil

total, used, free = shutil.disk_usage("/")

usage_percent = (used / total) * 100

print(f"Disk usage: {usage_percent:.2f}%")

if usage_percent > 80:
    print("⚠️ Warning: Disk usage is above 80%")
else:
    print("✅ Disk usage is under control")
```

Run:

```bash
python3 disk_check.py
```

✅ **Real DevOps use case**: Cron jobs + monitoring alerts

---

## 🔹 Automate Linux Command Execution

### 📌 Task:

Run `uptime` and `df -h` using Python.

### 📝 Script: `system_info.py`

```python
import subprocess

print("System Uptime:")
subprocess.run(["uptime"])

print("\nDisk Info:")
subprocess.run(["df", "-h"])
```

Run:

```bash
python3 system_info.py
```

---

## 🔹Python CLI Tool using argparse

### 📌 Task:

Create a CLI tool like:

```bash
python3 server_check.py --host google.com
```

---

### 📝 Script: `server_check.py`

```python
import argparse
import subprocess

parser = argparse.ArgumentParser(description="Server Reachability Checker")
parser.add_argument("--host", required=True, help="Hostname or IP address")

args = parser.parse_args()

result = subprocess.run(
    ["ping", "-c", "2", args.host],
    stdout=subprocess.DEVNULL
)

if result.returncode == 0:
    print(f"✅ {args.host} is reachable")
else:
    print(f"❌ {args.host} is NOT reachable")
```

Run:

```bash
python3 server_check.py --host google.com
```

✅ **This is a real DevOps-style CLI tool**

---

## 🔹Automate Log Monitoring

### 📌 Task:

Check if the word "ERROR" exists in a log file.

### 📝 Script: `log_monitor.py`

```python
log_file = "/var/log/syslog"

try:
    with open(log_file, "r") as f:
        for line in f:
            if "ERROR" in line:
                print("⚠️ ERROR found in log!")
                break
except FileNotFoundError:
    print("Log file not found")
```

---

## 🔹Make Script Executable (Linux Style)

```bash
chmod +x server_check.py
```

Add shebang at top:

```python
#!/usr/bin/env python3
```

Run like a CLI tool:

```bash
./server_check.py --host 8.8.8.8
```
---
# Handling environment variables  
Environment variables in Python** is *core DevOps knowledge* (used in Docker, Kubernetes, CI/CD, AWS, Jenkins, etc.).

## 🔹 What are Environment Variables? (DevOps View)

Environment variables are used to store:

* Secrets (tokens, passwords)
* Configuration (env, region, ports)
* Runtime values (PATH, HOME)

Examples:

```bash
ENV=prod
DB_HOST=localhost
AWS_REGION=us-east-1
```

---
## 🔹 View Environment Variables (Linux)

```bash
echo $HOME
echo $PATH
```
## 🔹 Read Environment Variables in Python

### 📝 Script: `read_env.py`

```python
import os

home = os.environ.get("HOME")
path = os.environ.get("PATH")

print("HOME:", home)
print("PATH:", path)
```

Run:

```bash
python3 read_env.py
```
---
## 🔹Set Environment Variables Temporarily (Shell)

```bash
export APP_ENV=dev
export APP_PORT=8080
```
## Access Custom Environment Variables in Python

### 📝 Script: `app_config.py`

```python
import os

env = os.environ.get("APP_ENV")
port = os.environ.get("APP_PORT")

print(f"Running in {env} mode on port {port}")
```
---
## 🔹 Set Environment Variables Inside Python (Runtime)

⚠️ Valid only for current process:

```python
import os

os.environ["DEBUG"] = "true"
print(os.environ["DEBUG"])
```
---
## 🔹 Environment Variables with `.env` File (Very Important)

### 📌 Step 1: Create `.env` file

```
APP_ENV=production
DB_USER=admin
DB_PASS=secret123
```

### 📌 Step 2: Install dependency

```bash
pip install python-dotenv
```

### 📌 Step 3: Load `.env` in Python

```python
from dotenv import load_dotenv
import os

load_dotenv()

print(os.environ.get("DB_USER"))
```

✅ Used in **Docker, Kubernetes, CI/CD pipelines**.
---
## 🔹 DevOps Use Case – Jenkins Example

```python
import os

build_id = os.environ.get("BUILD_ID")
job_name = os.environ.get("JOB_NAME")

print(f"Job: {job_name}, Build: {build_id}")
```

---
## 🎤 Interview Question

> How do you run Linux commands in Python?

**Answer:**

> Using the subprocess module, mainly `subprocess.run()` with return codes and output handling.

---
> How do you manage secrets in Python applications?

**Answer:**

> By using environment variables and .env files instead of hardcoding.
