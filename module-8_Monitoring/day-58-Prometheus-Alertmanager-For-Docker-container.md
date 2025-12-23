# Day-58-Prometheus-Alertmanager-For-Docker-container
### Step 1: Create ```docker-compose.yml```  file
```sh
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - ./alert.rules.yml:/etc/prometheus/alert.rules.yml:ro
      - prometheus_data:/prometheus
    ports:
      - "9090:9090"
    restart: unless-stopped
    networks:
      - monitoring

  alertmanager:
    image: prom/alertmanager:latest
    container_name: alertmanager
    volumes:
      - ./alertmanager.yml:/etc/alertmanager/alertmanager.yml:ro
    ports:
      - "9093:9093"
    restart: unless-stopped
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:9.0.0
    container_name: grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin   # change if desired
    volumes:
      - grafana_data:/var/lib/grafana
    ports:
      - "3000:3000"
    restart: unless-stopped
    networks:
      - monitoring

  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    pid: "host"
      #network_mode: bridge
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    ports:
      - "9100:9100"
    command:
      - --path.procfs=/host/proc
      - --path.sysfs=/host/sys
      - --collector.filesystem.ignored-mount-points="^/(sys|proc|dev|host|etc)($$|/)"
    restart: unless-stopped
    networks:
      - monitoring

  cadvisor:
    image: gcr.io/cadvisor/cadvisor:v0.46.0
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
    networks:
      - monitoring
volumes:
  prometheus_data:
  grafana_data:

networks:
  monitoring:
    driver: bridge
```

### Step 2: Create ```prometheus.yml```
```sh
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - "/etc/prometheus/alert.rules.yml"

alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093']

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['prometheus:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']

  - job_name: 'cadvisor'
    metrics_path: /metrics
    static_configs:
      - targets: ['cadvisor:8080']
```

### Step 3: Create ```alertmanager.yml```
```sh
global:
  resolve_timeout: 5m
  smtp_smarthost: 'smtp.gmail.com:465'
  smtp_from: 'rajiv19831@gmail.com'
  smtp_auth_username: 'rajiv19831@gmail.com'
  smtp_auth_password: 'uluhawdgripsmpzf'
  smtp_require_tls: false

route:
  receiver: 'email-alerts'
  group_wait: 10s
  group_interval: 1m
  repeat_interval: 2h

receivers:
  - name: 'email-alerts'
    email_configs:
      - to: 'rajiv19831@gmail.com'
        from: 'rajiv19831@gmail.com'
        auth_username: 'rajiv19831@gmail.com'
        auth_password: 'uluhawdgripsmpzf'
        smarthost: 'smtp.gmail.com:465'
        require_tls: false
```

### Step 4: Create ```alert.rules.yml```
```sh
groups:
  - name: node-alerts
    rules:
      - alert: HighNodeCPU
        expr: (1 - avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[2m]))) * 100 > 60
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "High CPU usage on {{ $labels.instance }}"
          description: "CPU usage is above 60% for more than 2 minutes."

  - name: container-alerts
    rules:
      - alert: ContainerDown
        expr: up{job="cadvisor"} == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "cAdvisor not reachable"
          description: "Prometheus cannot scrape cAdvisor, container metrics missing."
```

### Up all container 
```sh
docker-compose up -d
```

### Verify 
down the cadvisor container and we can see we get an email
