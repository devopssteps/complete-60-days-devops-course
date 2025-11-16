# Day-55: Monitoring with Prometheus + Grafana

### Hands-on Demo: Monitoring with Prometheus + Grafana

### Overview
You’ll install:
 - Prometheus → for collecting and storing metrics.
 - Node Exporter → to expose system metrics (CPU, memory, disk, etc.).
 - Grafana → to visualize metrics from Prometheus.

### Step 1: Update your system
```sh
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Prometheus
```sh
sudo useradd --no-create-home --shell /bin/false prometheus
```
1️⃣ Create a Prometheus user
```sh
sudo useradd --no-create-home --shell /bin/false prometheus
```
2️⃣ Create directories
```sh
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```
3️⃣ Download Prometheus
Visit https://prometheus.io/download/
 to get the latest version, or use this command (example for v2.54.1):
```sh
cd /tmp
wget https://github.com/prometheus/prometheus/releases/download/v2.54.1/prometheus-2.54.1.linux-amd64.tar.gz
tar xvf prometheus-2.54.1.linux-amd64.tar.gz
cd prometheus-2.54.1.linux-amd64
```
4️⃣ Move binaries
```sh
sudo cp prometheus /usr/local/bin/
sudo cp promtool /usr/local/bin/
```
5️⃣ Set ownership
```sh
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool
```
6️⃣ Move configuration files
```sh
sudo cp -r consoles /etc/prometheus
sudo cp -r console_libraries /etc/prometheus
sudo cp prometheus.yml /etc/prometheus/
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
```

### 🧾 Step 3: Configure Prometheus
Edit the config file:
```sh
sudo vi /etc/prometheus/prometheus.yml
```
Example content:
```sh
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    static_configs:
      - targets: ["localhost:9100"]
```
Save and exit.

### ⚙️ Step 4: Create a Prometheus systemd service
```sh
sudo vi /etc/systemd/system/prometheus.service
```
Paste:
```sh
[Unit]
Description=Prometheus Monitoring
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```
Save and exit.

### ▶️ Step 5: Start and enable Prometheus
```sh
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus
```
Prometheus should now be running at:
<br>
👉 http://your-server-ip:9090

### Step 6: Install Node Exporter (for system metrics)
```sh
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
tar xvf node_exporter-1.8.2.linux-amd64.tar.gz
cd node_exporter-1.8.2.linux-amd64
sudo cp node_exporter /usr/local/bin/
sudo useradd --no-create-home --shell /bin/false nodeusr
sudo chown nodeusr:nodeusr /usr/local/bin/node_exporter
```
Create a service:
```sh
sudo vi /etc/systemd/system/node_exporter.service
```
Paste:
```sh
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=nodeusr
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=default.target
```
Start and enable:
```sh
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter
```
Now check in your browser:
<br>
👉 http://your-server-ip:9100/metrics
<br>
Prometheus should automatically collect from Node Exporter.

### Step 7: Install Grafana
Add the official Grafana repository:
```sh
sudo apt install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo apt update
sudo apt install grafana -y

```
Start and enable Grafana:
```sh
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
sudo systemctl status grafana-server
```
Open Grafana in your browser:<br>
👉 http://your-server-ip:3000 
<br>
(Default username: admin, password: admin)


### 🔗 Step 8: Connect Grafana to Prometheus
 1. Go to Grafana → Connections → Data sources → Add data source.
 2. Choose Prometheus.
 3. In the URL, enter:
```sh
http://localhost:9090
```
 4. Click Save & Test.


### Step 9: Import Grafana Dashboards

Go to: <br>
👉 https://grafana.com/grafana/dashboards
<br>
Search for:
 - Node Exporter Full (ID: 1860)
 - Prometheus 2.0 Overview (ID: 3662)
Import them into Grafana for beautiful visual metrics.

### Done!
 - ✅ Prometheus running on port 9090
 - ✅ Node Exporter on 9100
 - ✅ Grafana dashboard on 3000

