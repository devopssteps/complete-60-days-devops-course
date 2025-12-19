# Day-59: Complete real-world demo collect Nodejs applicaltion log and send to loki :

 - ✅ Installing Loki -local install 
 - ✅ Running a Node.js app on an AWS EC2 instance - promtail install
 - ✅ Grafana on a local Ubuntu PC - grafana installing by docker compose
 - ✅ Sending EC2 application logs to Loki using Promtail
 - ✅ Viewing logs in Grafana

### 🏗️ Architecture (Simple & Practical)
```sh
EC2 (Node.js App + Promtail)
        |
        |  (HTTP push logs)
        v
EC2 (Loki)
        |
        v
EC2 (Grafana) OR Local Ubuntu (Grafana)
```

### Step-1: install loki in ec2-1
### install loki
```sh
cd /tmp
wget https://github.com/grafana/loki/releases/download/v3.5.1/loki-linux-amd64.zip
sudo apt install unzip
unzip loki-linux-amd64.zip
wget https://raw.githubusercontent.com/grafana/loki/main/cmd/loki/loki-local-config.yaml
```
Start loki
```sh
./loki-linux-amd64 -config.file=loki-local-config.yaml
```
Check loki is working 
```sh
http://100.31.213.83:3100/metrics
```
Stop loki just type 
```ctrl+c```

### Step-2: Install promtail
```sh
cd /tmp/
wget https://github.com/grafana/loki/releases/download/v3.6.3/promtail-linux-amd64.zip
sudo apt install unzip
wget https://raw.githubusercontent.com/grafana/loki/main/clients/cmd/promtail/promtail-local-config.yaml
```
Now open the file and edit the loki ip only
```sh
vi promtail-local-config.yaml
```
Edit
```sh
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://100.31.213.83:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log
      stream: stdout
```
Now Run 
```sh
./promtail-linux-amd64 -config.file=promtail-local-config.yaml
```
For check any command 
```sh
./promtail-linux-amd64 --help
```
Browse promtail:
```sh
http://98.93.133.145:9080/targets
```

### Step 3: Grafana already installed
Grafana → http://localhost:3000
 - user: admin
 - pass: admin

### Step 4: Add Loki in Grafana
Grafana → Settings → Data Sources → Loki
```sh
URL: http://our-loki-ip:3100
```
If your loki and grafana in same server then just use
```sh
URL: http://localhost:3100
```
Click Save & Test ✅

### Step-5: View Logs in Grafana
Query Logs
 - Grafana → Explore → Loki

### Step-6: Now install a nodejs app and View Logs in Grafana by loki
ssh to ec2 where we install the promtail
<br>
Install Node.js on EC2
```sh
sudo apt update -y
sudo apt install nodejs npm -y
```
Create Node.js App
```sh
mkdir node-app
cd node-app
npm init -y
npm install express winston
```
create app.js
```sh
cat > app.js
```
Paste:
```sh
const express = require("express");
const winston = require("winston");

const app = express();

const logger = winston.createLogger({
  transports: [
    new winston.transports.File({
      filename: "/var/log/node-app/app.log"
    })
  ]
});

app.get("/", (req, res) => {
  logger.info("Home page visited");
  res.send("Node.js app running!");
});

app.listen(3000, () => {
  logger.info("Server started on port 3000");
});
```
Create log directory
```sh
sudo mkdir -p /var/log/node-app
sudo chown -R ubuntu:ubuntu /var/log/node-app
node app.js
```
Logs will be generated here:
```sh
/var/log/node-app/app.log
```

Promtail config (promtail-config.yml)
<br>
Replace LOCAL_IP with your Ubuntu PC IP.
```sh
- job_name: nodejs-app
  static_configs:
   - targets:
       - localhost
     labels:
        job: nodejs
        host: ec2
        __path__: /var/log/node-app/*.log
```
Complete Promtail config file
```sh
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://100.31.213.83:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      __path__: /var/log/*log
      stream: stdout

- job_name: nodejs-app
  static_configs:
   - targets:
       - localhost
     labels:
        job: nodejs
        __path__: /var/log/node-app/*.log
```
Run promtail
```sh
./promtail-linux-amd64 -config.file=promtail-local-config.yaml
```
Now check the log in Grafana
```sh
Grafana → Explore → Loki
```
We should see 
<br>
```sh
{"level":"info","message":"Server started on port 3000"}
```
Done the lab

### 🔐 Security Notes (Important)
✅ Best practice:
 - Use VPN / SSH tunnel instead of exposing port 3100
 - Or deploy Loki in cloud and push logs privately
