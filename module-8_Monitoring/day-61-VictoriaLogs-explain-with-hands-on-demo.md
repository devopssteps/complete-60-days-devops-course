# VictoriaLogs explain with hands on demo 

### 🔹 What is VictoriaLogs? (Simple Explanation)
VictoriaLogs is a high-performance, cost-efficient log database created by the makers of VictoriaMetrics.

Why VictoriaLogs?
 - ⚡ Very fast log ingestion & search
 - 💰 Much cheaper storage than ELK
 - 🧩 Simple architecture (single binary)
 - 📦 Easy to run with Docker & Kubernetes
 - 🔁 Loki-like experience but simpler

### 🔹 Architecture (Simple)
```sh
Application Logs
      ↓
VictoriaLogs
      ↓
VictoriaLogs UI / HTTP API
```

### 🔹 Hands-On Demo (Local PC using Docker)
We will:
 - 1️⃣ Run VictoriaLogs
 - 2️⃣ Send logs to VictoriaLogs
 - 3️⃣ Query logs from UI
 - 

### ✅ Step 1: Run VictoriaLogs using Docker
```sh
docker run -d \
  --name victorialogs \
  -p 9428:9428 \
  -v victorialogs-data:/victoria-logs-data \
  victoriametrics/victoria-logs
```
What this does
 - Runs VictoriaLogs server
 - Exposes UI + API on port 9428
 - Persists log data

### ✅ Step 2: Open VictoriaLogs Web UI
Open browser:
```sh
http://127.0.0.1:9428
```
You will see:
 - Log query interface
 - Time range selector
 - Search box
VictoriaLogs is running!

### ✅ Step 3: Push Sample Logs (Hands-On)
VictoriaLogs accepts logs via HTTP API.
Send logs using curl
```sh
curl -X POST http://localhost:9428/insert/jsonline \
  -H "Content-Type: application/json" \
  -d "{\"_time\":\"$(date -Iseconds)\",\"_msg\":\"Application started\",\"level\":\"info\",\"service\":\"node-app\"}"
```
Send another log:
```sh
curl -X POST http://localhost:9428/insert/jsonline   -H "Content-Type: application/json"   -d "{\"_time\":\"$(date -Iseconds)\",\"_msg\":\"Database connection failed\",\"level\":\"error\",\"service\":\"node-app\"}"
```
✅ Logs are now stored in VictoriaLogs.

### ✅ Step 4: Query Logs in VictoriaLogs UI
In the query box, try:
🔹 View all logs
```sh
*
```
🔹 Filter by service
```sh
service:node-app
```
🔹 Filter error logs
```sh
level:error
```
🔹 Search by message
```sh
_msg:"Database"
```
📌 Queries are very fast, even with large data.

### 🔹 Step 5: Generate Logs Automatically (Demo Script)
Example: Bash log generator
```sh
while true; do
  curl -s -X POST http://localhost:9428/insert/jsonline \
    -H "Content-Type: application/json" \
    -d "{\"_time\":\"$(date -Iseconds)\",\"_msg\":\"Heartbeat log\",\"level\":\"info\",\"service\":\"demo\"}"
  sleep 2
done
```
Now refresh UI → logs appear in real time.
### Verify Logs via API (Extra Check)
```sh
curl "http://localhost:9428/select/logsql/query?query=service:node-app"
```

### 🔹 VictoriaLogs Query Language (Basic)
| Query                   | Meaning        |
| ----------------------- | -------------- |
| `*`                     | All logs       |
| `level:error`           | Error logs     |
| `service:backend`       | Service filter |
| `_msg:"timeout"`        | Text search    |
| `_time:5m`              | Last 5 minutes      |
| `_time:1h`              | Last hour      |
| `_time:1d`              | Last day      |


### 🔹 VictoriaLogs vs Loki vs ELK (Quick Comparison)
| Feature        | VictoriaLogs  | Loki     | ELK     |
| -------------- | ------------- | -------- | ------- |
| Setup          | ⭐ Very Easy   | Easy     | Complex |
| Performance    | ⚡ Very High   | High     | Medium  |
| Cost           | 💰 Low        | Medium   | High    |
| Learning Curve | Easy          | Medium   | Hard    |
| Components     | Single binary | Multiple | Many    |


### 🔹 Real-World Use Cases
 - Kubernetes application logs
 - Docker container logs
 - System logs
 - Cost-effective log storage
 - ELK replacement for small & medium setups


# Integrate victoriaLogs with-grafana

### ✅ STEP 1: Run Grafana (Docker)
```sh
docker run -d \
  --name grafana \
  -p 3000:3000 \
  -v grafana-data:/var/lib/grafana \
  grafana/grafana
```
Open Grafana:
```sh
http://127.0.0.1:3000
```
Login:
```sh
admin / admin
```

### ✅ STEP 2: Install VictoriaLogs Data Source Plugin
Install plugin inside Grafana container
```sh
docker exec -it grafana grafana-cli plugins install victoriametrics-logs-datasource
```
Restart Grafana:
```sh
docker restart grafana
```

### ✅ STEP 3: Add VictoriaLogs Data Source in Grafana
 - 1️⃣ Grafana → Settings → Data Sources
 - 2️⃣ Click Add data source
 - 3️⃣ Select VictoriaLogs
Configure:
 - URL:
```sh
http://host.docker.internal:9428
```
(If Linux):
```sh
http://172.17.0.1:9428
```

### ✅ STEP 4: Query Logs in Grafana (Explore Mode)
 - 1️⃣ Click Explore
 - 2️⃣ Select VictoriaLogs data source

Query examples: 
<br>
🔹 View all logs
```sh
*
```
🔹 Filter by service
```sh
service:node-app
```
🔹 Filter error logs
```sh
level:error
```
🔹 Search message text
```sh
_msg:"Grafana"
```
