# Day-03 Networking, SSH, SCP, curl, wget – Explained with Examples

### 🌐 1. Networking Basics (in DevOps Context)
Networking is the backbone of communication between servers, services, and users.

Common Terms:
 - IP Address – Unique identifier for a device on a network.
 - DNS – Translates domain names to IP addresses.
 - Port – Logical endpoint for communication (e.g., port 80 for HTTP).
 - Ping – Tests network connectivity to a host.
 - Traceroute – Shows the path packets take to a host.
    - if pkackage not avaible then install ```sudo apt install traceroute```<br>

✅ Example:
```sh
ping google.com
traceroute www.google.com
```
Output:
```sh
PING google.com (142.250.190.14): 56 data bytes
64 bytes from 142.250.190.14: icmp_seq=0 ttl=117 time=10.2 ms
```
## We can login any server by username and password OR SSH key and SSH key is recommended one
### 🔐 2. SSH (Secure Shell)
### Create public key and private key
```sh
ssh-keygen
```
Create ssh public key name ansible
```sh
ssh-keygen -t rsa -f ~/.ssh/ansible
```
Location and file where the public key is save
```sh
 .ssh/authorized_keys
```
Copy the public key to a remote host
```sh
ssh-copy-id rajiv@192.168.30.11
```
OR
<br>we can copy the public key and pest it to the authorized_keys file .ssh/authorized_keys
<br>Copy the public key which is available is a specific location 
```sh
ssh-copy-id -i /home/thor/.ssh/ansible ansible@web1
```

SSH allows secure remote login and command execution on remote servers.
✅ Syntax:
```sh
ssh user@hostname
```
✅ Example:
```sh
ssh ubuntu@54.210.12.34
```
 - Connects to an EC2 instance or remote server securely.
 - Requires SSH key or password authentication.

### 📂 3. SCP (Secure Copy Protocol)
SCP copies files securely between local and remote systems over SSH.
```sh
scp [source] [destination]
```
✅ Example:
```sh
scp -i my-key d.txt ubuntu@98.84.100.244:/home/ubuntu/
scp myfile.txt ubuntu@54.210.12.34:/home/ubuntu/
```
Copies myfile.txt to a remote EC2 instance.<br>
✅ Reverse:
```sh
scp -i my-key d.txt ubuntu@98.84.100.244:/home/ubuntu/dd.txt .
```
 - Downloads the file from the server to your local machine.

### 🌍 4. curl (Client URL)
✅ Syntax:
```sh
curl [options] [URL]
```
✅ Example:
```sh
curl https://jsonplaceholder.typicode.com/posts/1
```
Output (JSON):
```sh
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere...",
  "body": "quia et suscipit..."
}
```
✅ Upload a file using POST:
```sh
curl -X POST -F "file=@myfile.txt" http://example.com/upload
```
### ⬇️ 5. wget (Web Get)
wget is used to download files from the web.
✅ Syntax:
```sh
wget [URL]
```
✅ Example:
```sh
wget https://example.com/file.zip
wget https://dlcdn.apache.org/maven/maven-3/3.9.11/binaries/apache-maven-3.9.11-bin.tar.gz
```
 - Downloads file.zip from the URL to your current directory.

✅ Download in the background:
```sh
wget -b https://example.com/largefile.iso
```

### 🧠 Summary Table
| Tool       | Purpose                          | Example Command                          |
| ---------- | -------------------------------- | ---------------------------------------- |
| `ping`     | Check network connectivity       | `ping google.com`                        |
| `ssh`      | Connect to a remote server       | `ssh ubuntu@<server-ip>`                 |
| `scp`      | Copy files securely              | `scp file.txt ubuntu@<ip>:/home/ubuntu/` |
| `curl`     | Fetch data from APIs/web servers | `curl https://api.github.com/users`      |
| `wget`     | Download files from the web      | `wget https://example.com/file.zip`      |
| `nslookup` | domin name query                 | `nslookup www.google.com`                |
| `dig`      | domin name query                 | `dig www.google.com`                     |


### ✅ Real-world Use Case (DevOps Scenario)
 - You're deploying an app to an AWS EC2 instance:
   - Use ssh to access the EC2 instance.
   - Use scp to copy your code to the server.
   - Use curl to test the API endpoints after deployment.
   - Use ping or traceroute to troubleshoot if the app isn’t reachable.
   - Use wget to download dependencies or install scripts from external sources.


### commonly used ports in DevOps
### 🔗 Networking & SSH
| Service | Default Port | Protocol | Notes                       |
| ------- | ------------ | -------- | --------------------------- |
| SSH     | 22           | TCP      | Remote shell access         |
| HTTP    | 80           | TCP      | Unencrypted web traffic     |
| HTTPS   | 443          | TCP      | Encrypted web traffic (TLS) |


### 🗄️ Databases
| Database       | Default Port | Protocol | Notes                     |
| -------------- | ------------ | -------- | ------------------------- |
| MySQL/MariaDB  | 3306         | TCP      | Relational database       |
| PostgreSQL     | 5432         | TCP      | Relational database       |
| MongoDB        | 27017        | TCP      | NoSQL database            |
| Redis          | 6379         | TCP      | In-memory key-value store |
| Elasticsearch  | 9200         | TCP      | Search engine HTTP API    |
| Cassandra      | 9042         | TCP      | NoSQL DB using CQL        |
| Kafka (broker) | 9092         | TCP      | Apache Kafka              |


### 🧪 CI/CD Tools
| Tool              | Default Port | Protocol | Notes                       |
| ----------------- | ------------ | -------- | --------------------------- |
| Jenkins           | 8080         | TCP      | CI/CD server UI             |
| GitLab (web)      | 80/443/22    | TCP      | Git over HTTPS/SSH          |
| SonarQube         | 9000         | TCP      | Code quality scanner        |
| Nexus/Artifactory | 8081         | TCP      | Artifact repository manager |

 ### 📈 Monitoring & Logging
 | Tool           | Default Port | Protocol | Notes                    |
| -------------- | ------------ | -------- | ------------------------ |
| Prometheus     | 9090         | TCP      | Metrics collection       |
| Grafana        | 3000         | TCP      | Visualization dashboards |
| Alertmanager   | 9093         | TCP      | Prometheus alerting      |
| Loki (Grafana) | 3100         | TCP      | Logs backend for Grafana |
| Fluentd        | 24224        | TCP      | Log collector            |
| Graylog Web UI | 9000         | TCP      | Logging dashboard        |
| Kibana         | 5601         | TCP      | Elasticsearch dashboard  |
| Logstash       | 5044         | TCP      | Log processing (input)   |
| Zabbix Server  | 10051        | TCP      | Monitoring tool          |


### ☸️ Container & Orchestration
| Service           | Default Port | Protocol | Notes                      |
| ----------------- | ------------ | -------- | -------------------------- |
| Docker Engine API | 2375/2376    | TCP      | Non-TLS/TLS (respectively) |
| Kubernetes API    | 6443         | TCP      | Control plane access       |
| etcd              | 2379–2380    | TCP      | Kubernetes DB backend      |
| kubelet           | 10250        | TCP      | Node agent in Kubernetes   |


### 💻 Dev Tools & Misc
| Service     | Default Port | Protocol | Notes                  |
| ----------- | ------------ | -------- | ---------------------- |
| VNC         | 5900         | TCP      | Remote desktop sharing |
| SMTP (mail) | 25, 465, 587 | TCP      | Email sending          |
| FTP         | 21           | TCP      | File transfer protocol |
| DNS         | 53           | UDP/TCP  | Domain name system     |
| NTP         | 123          | UDP      | Time sync              |



