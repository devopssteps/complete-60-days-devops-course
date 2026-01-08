# day-63:Functions & Modules
### In this video we will learn
🎯 Goal: Reusable Python code.
 - Functions (def, parameters, return values).
 - Importing & using Python modules (os, sys).
 - Exception handling (try/except).
 - ✅ Mini Project: Script that checks if a server is reachable (ping).
Below is a **complete, step-by-step, hands-on demo** you can **teach live** for **Python Functions & Modules**, followed by a **real DevOps-style mini project**.

---

# 🐍 Python Hands-On Demo: Functions & Modules

## 1️⃣ What is a Function? (Simple Explanation)

A **function** is a reusable block of code that performs a specific task.

👉 Instead of repeating code, **write once, use many times**.

---

## 2️⃣ Create Your First Function (Hands-On)

```python
def greet():
    print("Hello DevOps Engineers 👋")

greet()
```

---

## 3️⃣ Function with Parameters

```python
def greet_user(name):
    print(f"Hello {name}, welcome to DevOps")

greet_user("Rajiv")
greet_user("Student")
```

---

## 4️⃣ Function with Return Value

```python
def add(a, b):
    return a + b

result = add(10, 5)
print("Result:", result)
```

---

## 5️⃣ Function with Default Argument

```python
def server_status(status="running"):
    print("Server status is:", status)

server_status()
server_status("stopped")
```

---

## 6️⃣ Built-in vs User-Defined Functions

### Built-in:

```python
print("Hello")
len("DevOps")
type(10)
```

### User-defined:

```python
def deploy():
    print("Deploying application...")
```

---

# 📦 Python Modules (Hands-On)

## 7️⃣ What is a Module?

A **module** is a Python file that contains:

* functions
* variables
* classes

👉 Helps organize large codebases.

---

## 8️⃣ Using Built-in Modules

### Example: `os` module

```python
import os

print(os.getcwd())
```

### Example: `time` module

```python
import time

print("Waiting...")
time.sleep(2)
print("Done")
```

---

## 9️⃣ Import Specific Function

```python
from datetime import datetime

print(datetime.now())
```

---

## 🔟 Create Your Own Module (Hands-On)

### Step 1: Create `utils.py`

```python
def welcome(name):
    print(f"Welcome {name} to DevOps Steps")

def add(a, b):
    return a + b
```

### Step 2: Use it in `main.py`

```python
import utils

utils.welcome("Rajiv")
print(utils.add(5, 7))
```

---

# 🚀 MINI PROJECT: Server Reachability Checker (PING)

## 🎯 Project Goal

Write a Python script to **check if a server is reachable**, just like DevOps health checks.

---

## 🧩 Project Logic

1. Take server IP or hostname as input
2. Ping the server
3. Show reachable / not reachable

---

## 🟢 Version 1: Simple Ping Script (Linux / WSL)

### 📄 `ping_check.py`

```python
import os

server = input("Enter server IP or hostname: ")

response = os.system(f"ping -c 1 {server} > /dev/null 2>&1")

if response == 0:
    print(f"{server} is reachable ✅")
else:
    print(f"{server} is NOT reachable ❌")
```

### ▶ Run:

```bash
python3 ping_check.py
```

---

## 🟢 Version 2: Using Function (Best Practice)

```python
import os

def check_server(server):
    response = os.system(f"ping -c 1 {server} > /dev/null 2>&1")
    return response == 0

server = input("Enter server IP or hostname: ")

if check_server(server):
    print("Server is UP ✅")
else:
    print("Server is DOWN ❌")
```

---

## 🟢 Version 3: Check Multiple Servers (DevOps Style)

```python
import os

servers = ["8.8.8.8", "google.com", "192.168.1.100"]

def check_server(server):
    return os.system(f"ping -c 1 {server} > /dev/null 2>&1") == 0

for server in servers:
    status = "UP ✅" if check_server(server) else "DOWN ❌"
    print(f"{server} is {status}")
```
“This is how real DevOps monitoring scripts start — simple, but powerful.”
---

## 🧠 Interview Question (Very Common)

❓ **How do you check server health using Python?**

✅ Answer:

> By using functions and system commands like `ping` via the `os` or `subprocess` module.




