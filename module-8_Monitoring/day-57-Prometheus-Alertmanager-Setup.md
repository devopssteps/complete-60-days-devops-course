# Day 57: Prometheus Alertmanager Hands on Demo

Do the following tasks:
 - ✔️ Server #1 (Apllication Server) → Runs Nginx + Node Exporter
 - ✔️ Server #2 (Monitoring Server) → Runs Prometheus + Alertmanager
 - ✔️ If Server #1 goes down → send email alert
 - ✔️ If CPU usage high → send another alert

### Step 1 — Install Node Exporter on Nginx EC2-1
SSH into your Nginx EC2 and install node exporter:
```sh
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.1/node_exporter-1.8.1.linux-amd64.tar.gz
tar -xvf node_exporter-1.8.1.linux-amd64.tar.gz
sudo mv node_exporter-1.8.1.linux-amd64/node_exporter /usr/local/bin/
```
Create systemd service:
```sh
sudo vi /etc/systemd/system/node_exporter.service
```
Paste:
```sh
[Unit]
Description=Node Exporter
After=network.target

[Service]
ExecStart=/usr/local/bin/node_exporter
User=nobody

[Install]
WantedBy=default.target
```
Start and enable:
```sh
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
```
Check
```sh
curl http://localhost:9100/metrics
```

### Step 2 — Configure Prometheus (on second EC2)
SSH into Prometheus server and edit:
```sh
ssh -i key.pem ubuntu@<PROMETHEUS_EC2_PUBLIC_IP>
```
Edit Prometheus config:
```sh
sudo vi /etc/prometheus/prometheus.yml
```
Add your Nginx server (replace IP):
```sh
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "nginx-ec2"
    static_configs:
      - targets: ["<NGINX_EC2_PRIVATE_IP>:9100"]  # Node Exporter
```
Save and reload:
```sh
sudo systemctl restart prometheus
```

### Step 3 — Install & Configure Alertmanager Email
Install Alertmanager
```sh
cd /tmp
curl -LO https://github.com/prometheus/alertmanager/releases/download/v0.27.0/alertmanager-0.27.0.linux-amd64.tar.gz
tar -xvf alertmanager-0.27.0.linux-amd64.tar.gz
cd alertmanager-0.27.0.linux-amd64
sudo cp alertmanager amtool /usr/local/bin/
sudo mkdir /etc/alertmanager /var/lib/alertmanager
sudo cp alertmanager.yml /etc/alertmanager/
```
Configure Alertmanager config:
```sh
sudo vi /etc/alertmanager/alertmanager.yml
```
Example Gmail SMTP setup (use App Password if Gmail):
⚠️ If using Gmail, create an App Password under Google Account → Security → App passwords (2FA must be enabled).
```sh
 global:
   resolve_timeout: 5m
   smtp_smarthost: 'smtp.gmail.com:465'
   smtp_from: 'yourgmail@gmail.com'
   smtp_auth_username: 'yourgmail@gmail.com'
   smtp_auth_password: 'YOUR_APP_PASSWORD'
   smtp_require_tls: false
 
 route:
   receiver: 'email-alerts'
   group_wait: 10s
   group_interval: 1m
   repeat_interval: 2h
 
 receivers:
   - name: 'email-alerts'
     email_configs:
       - to: 'yourgmail@gmail.com'
         from: 'yourgmail@gmail.com'
         auth_username: 'yourgmail@gmail.com'
         auth_password: 'YOUR_APP_PASSWORD'
         smarthost: 'smtp.gmail.com:465'
         require_tls: false
```
Create systemd service for Alertmanager
```sh
sudo vi /etc/systemd/system/alertmanager.service
```
Paste this:
```sh
[Unit]
Description=Alertmanager
After=network.target

[Service]
ExecStart=/usr/local/bin/alertmanager \
  --config.file=/etc/alertmanager/alertmanager.yml \
  --storage.path=/var/lib/alertmanager/
Restart=always

[Install]
WantedBy=multi-user.target
```
Then run:
```sh
sudo systemctl daemon-reload
sudo systemctl enable alertmanager
sudo systemctl start alertmanager
```
Check: <br>
👉 http://localhost:9093
<br>
You should see the Alertmanager dashboard.

Integrate Alertmanager with Prometheus
Edit Prometheus config:
```sh
sudo vi /etc/prometheus/prometheus.yml
```
Add at the top level:
```sh
alerting:
  alertmanagers:
    - static_configs:
        - targets: ["localhost:9093"]
```
For Restarting Alertmanager:
```sh
sudo systemctl restart alertmanager
```

### Step 4 — Create Alert Rules
Create alert file:
```sh
sudo vi /etc/prometheus/alert-rules.yml
```
Paste alerts:
```sh
groups:
- name: EC2 Alerts
  rules:

  # Alert 1: Node down
  - alert: NginxServerDown
    expr: up{job="nginx-ec2"} == 0
    for: 2m
    labels:
      severity: critical
    annotations:
      summary: "Nginx EC2 Down"
      description: "The EC2 running Nginx is unreachable."

  # Alert 2: High CPU usage
  - alert: HighCPUUsage
    expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle",job="nginx-ec2"}[2m])) * 100) > 80
    for: 1m
    labels:
      severity: warning
    annotations:
      summary: "High CPU Usage Detected"
      description: "CPU usage on Nginx EC2 server is above 80%."
```
Edit Prometheus config to load alert rules:
```sh
sudo vi /etc/prometheus/prometheus.yml
```
Add:
```sh
rule_files:
  - "/etc/prometheus/alert-rules.yml"
```
Restart Prometheus:
```sh
sudo systemctl restart prometheus
```

### Step 5 — Test Alerts
🔥 Test EC2 Down alert
<br>
Stop node exporter:
```sh
sudo systemctl stop node_exporter
```
Wait ~2–3 minutes → You should get an email:
<br>
ALERT: NginxServerDown

🔥 Test High CPU alert
Run CPU stress:
```sh
sudo apt install stress -y
stress --cpu 4 --timeout 120
```
Within 1–2 minutes → You get:
<br>
ALERT: HighCPUUsage

### Verify in Prometheus UI
Open browser:
```sh
http://<PROMETHEUS_EC2_PUBLIC_IP>:9090/alerts
```
You should see alerts change from Pending → Firing.
