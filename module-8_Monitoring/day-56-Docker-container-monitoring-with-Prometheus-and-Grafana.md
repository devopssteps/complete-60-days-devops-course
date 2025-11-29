# Day-56: Monitor docker container by prometheus & Grafana - Hands on Lab

### ✅ Working Architecture
```sh
Grafana  ← reads metrics ←  Prometheus  ← scrapes metrics ←  cAdvisor 
                                                                ↑
                                                 Docker containers running on OS
```

### 🟢 STEP-1: Install Docker in WSL Ubuntu
```sh
sudo apt update
sudo apt install docker.io -y
sudo usermod -aG docker $USER
```

### 🟢 STEP-2: Create a project folder
```sh
mkdir docker-monitoring
cd docker-monitoring
```

### 🟢 STEP-3: Create docker-compose.yml (Fully Working in WSL2)
Create the file:
```sh
vi docker-compose.yml
```
Paste:
```sh
version: '3.8'

services:
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:v0.46.0   # WORKING VERSION
    container_name: cadvisor
    ports:
      - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
      - /sys/fs/cgroup:/sys/fs/cgroup:ro
    restart: unless-stopped

  prometheus:
    image: prom/prometheus
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    restart: unless-stopped

  grafana:
    image: grafana/grafana
    container_name: grafana
    ports:
      - "3000:3000"
    restart: unless-stopped
```
 - ✔ cAdvisor v0.46.0 → FULL metrics work
 - ✔ Dashboards show everything

### 🟢 STEP-4: Create prometheus.yml
```sh
vi prometheus.yml
```
Past:
```sh
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['prometheus:9090']

  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']
```

### 🟢 STEP-5: Start the monitoring stack
```sh
docker compose up -d
```
Check containers:
```sh
docker ps
```
✔ Should see: cadvisor, prometheus, grafana

### 🟢 STEP-6: Check Prometheus
Open:
<br>
👉 http://localhost:9090
<br>
Run query:
```sh
container_memory_usage_bytes
```
You should see container metrics.

### 🟢 STEP-7: Login to Grafana
Open:
<br>
👉 http://localhost:3000
<br>
Default:
 - User: admin
 - Pass: admin

### 🟢 STEP-8: Verify cAdvisor UI
Open:
<br>
👉 http://localhost:8080/containers/
<br>
You will see ALL containers & metrics.

### 🟢 STEP-9: Add Prometheus Data Source
 1. Settings → Data Sources
 2. Add → Prometheus
 3. URL:
```sh
http://prometheus:9090
```
 4. Save & Test → ✔ Success should appear.

### 🟢 STEP-10: Import FULL working dashboards
Import this ID list:
| Dashboard Name            | Grafana ID |                                                 |
| ------------------------- | ---------- | ----------------------------------------------- |
| Docker Monitoring         | **893**    | This one detects new containers automatically   |
| Docker Container Overview | **179**    | 
| Docker & System Dashboard | **193**    | Shows per-container CPU/RAM/Network automatically |

Import Steps:
<br>
Grafana → Dashboards → Import → enter ID → Load → Select Prometheus → Import
<br>
✔ All dashboards will show:
 - CPU
 - Memory
 - Disk
 - Swap
 - Filesystem
 - Uptime
 - Container count
 - Network
 - Start time
 - Image info
 - Runtime info


### 🟢 Add a sample docker container to monitor
```sh
docker run -d --name test-nginx -p 8081:80 nginx
```
Now it will appear in:
 - Prometheus query
 - cAdvisor
 - Grafana dashboards








