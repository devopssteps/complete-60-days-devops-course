## ✅ Day 01: Devops fundamental
Todays video with cover the following things:
 - What is DevOps?
 - SDLC vs DevOps,
 - CI/CD Concepts
 - DevOps Tools Overview
 - Real-Time Workflow Example when we use DevOps


## 🔹 What is DevOps?
DevOps is a combination of:
 - Dev = Development (coding, building features)
 - Ops = Operations (deploying, monitoring, infrastructure)

Definition: DevOps is a cultural and technical approach that aims to integrate software development (Dev) and IT operations (Ops) to shorten the software development life cycle and provide continuous delivery with high quality. Key Concepts:
 - Continuous Integration (CI)
 - Continuous Delivery/Deployment (CD)
 - Infrastructure as Code (IaC)
 - Monitoring & Feedback

### 🔸 Real-World Example:
Imagine you're building a web app:
 - Developer writes code.
 - Ops team deploys the app to a server.

In traditional setup, there’s delay, miscommunication. <br>
In DevOps:
 - Developer and Ops work together
 - Use automation tools (like Jenkins, Docker, GitHub Actions)
 - Deploy in minutes instead of days.


## 🔹 SDLC vs DevOps
| Feature            | Traditional SDLC             | DevOps                               |
| ------------------ | ---------------------------- | ------------------------------------ |
| Process            | Sequential (Waterfall/Agile) | Continuous (CI/CD pipeline)          |
| Team Collaboration | Dev and Ops are **separate** | Dev and Ops work **together**        |
| Delivery           | Slow and manual              | Fast and automated                   |
| Feedback           | Late                         | Continuous and real-time             |
| Tools              | Manual testing, scripting    | CI/CD, Docker, Kubernetes, Terraform |

### 🔸 Diagram Explanation:
🟡 Traditional SDLC (Waterfall):
```sh
Planning → Development → Testing → Deployment → Maintenance
(Each phase done one by one)
```
🟢 DevOps Cycle:
```sh
PLAN → DEVELOP → BUILD → TEST → RELEASE → DEPLOY → OPERATE → MONITOR
    ^                                                           |
    └─────────────────── Continuous Feedback ───────────────────┘
```

### 🔹 What is CI/CD?
| Term   | Full Form                        | Description                                      |
| ------ | -------------------------------- | ------------------------------------------------ |
| **CI** | Continuous Integration           | Automatically testing and merging code           |
| **CD** | Continuous Delivery / Deployment | Automatically delivering or deploying to servers |

### 🔸 Example Workflow:
 1. Developer pushes code to GitHub.
 2. Jenkins triggers a pipeline:
    - Builds the app
    - Runs tests
    - If passed, deploys to a server
 ```sh
# Step 1: Developer writes code
git commit -m "Add new feature"

# Step 2: Push to GitHub
git push origin main

# Step 3: Jenkins auto-builds and deploys
```

### 🧠 Pro Tips to Remember
 - DevOps is not a tool, it’s a culture + automation
 - CI = testing + building on each commit
 - CD = deploying automatically or semi-automatically
 - Aim = faster releases, fewer bugs, happy users

### ✅ DevOps Tools Overview
| Stage        | Tool                     | Purpose                                   |
| ------------ | ------------------------ | ----------------------------------------- |
| Plan         | Jira, Trello             | Project and issue tracking                |
| Develop      | Git, GitHub, GitLab      | Source code management                    |
| Build        | Maven, Gradle, npm       | Build automation                          |
| CI/CD        | Jenkins, GitHub Actions  | Automate testing, integration, deployment |
| Test         | Selenium, JUnit, PHPUnit | Automated testing                         |
| Deploy       | Ansible, Terraform       | Deploy infrastructure                     |
| Containerize | Docker                   | Containerize applications                 |
| Orchestrate  | Kubernetes (EKS/GKE/AKS) | Manage containers at scale                |
| Monitor      | Prometheus, Grafana, ELK | Log and performance monitoring            |
| Alert        | Alertmanager, PagerDuty  | Incident management                       |


### ✅ Real-Time Workflow Example
Scenario: Deploy a Node.js App using DevOps
 - 👨‍💻 Developer pushes code to GitHub
 - ⚙️ Jenkins detects the push via webhook, runs pipeline:
    - Pulls code
    - Runs unit tests (with Mocha or Jest)
    - Builds Docker image
    - Pushes image to Docker Hub
- 🌐 Ansible connects to EC2 or prepares Kubernetes manifests
- 🌐 Terraform connects to craete EC2 by code
- 🐳 Docker runs containers
- ☸️ Kubernetes (EKS) deploys updated containers
- 📊 Prometheus + Grafana monitor CPU, memory, and response time
- 🔔 Alertmanager sends Slack alert if issues detected




