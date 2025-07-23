## ✅ Day 01: What is DevOps? SDLC vs DevOps, CI/CD Concepts

## 🔹 1. What is DevOps?
DevOps is a combination of:
 - Dev = Development (coding, building features)
 - Ops = Operations (deploying, monitoring, infrastructure)

Definition: DevOps is a cultural and technical approach that aims to integrate software development (Dev) and IT operations (Ops) to shorten the software development life cycle and provide continuous delivery with high quality.

### 🔸 Real-World Example:
Imagine you're building a web app:
 - Developer writes code.
 - Ops team deploys the app to a server.

In traditional setup, there’s delay, miscommunication. In DevOps:
 - Developer and Ops work together
 - Use automation tools (like Jenkins, Docker, GitHub Actions)
 - Deploy in minutes instead of days.


## 🔹 2. SDLC vs DevOps
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


### 🔹 3. What is CI/CD?
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

### 🔹 CI/CD Tools Examples
| Task             | Tool Example                  |
| ---------------- | ----------------------------- |
| Code repo        | GitHub, GitLab                |
| CI/CD pipeline   | Jenkins, GitHub Actions       |
| Containerization | Docker                        |
| Orchestration    | Kubernetes                    |
| Infrastructure   | Terraform, AWS CloudFormation |







