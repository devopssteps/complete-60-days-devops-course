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

