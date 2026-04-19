# Day 13: Jenkins Installation (Local + Docker) – Hands-On Example

We'll cover both methods:

### 🖥️ Part 1: Jenkins Installation (Local - Native)
✅ Step-by-step for Ubuntu/Debian (adjust as needed for other OS)
```sh
sudo apt update
sudo apt install openjdk-11-jdk -y
java -version
```

✅ Add Jenkins Repo & Install
```sh
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'

sudo apt update
sudo apt install jenkins -y
```

✅ Access Jenkins
Visit:
```sh
http://localhost:8080
```
✅ Unlock Jenkins
```sh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
Paste this into the web UI to complete the setup.

## 🐳 Part 2: Jenkins Installation (Using Docker)
### Install jenkins by docker compose
✅ Step 1: First make sure docker and docker compose is inatalled
```sh
docker --version
docker-compose version
```
✅ Step 2: Create a folder where jenkis data will be stored
```sh
mkdir jenkins_home
```
✅ Step 3: Create the docker-compose.yaml file 
```sh
version: '3'
services:
  jenkins:
    container_name: jenkins
    image: jenkins/jenkins
    ports:
      - "8080:8080"
    volumes:
      - $PWD/jenkins_home:/var/jenkins_home
    networks:
      - net

networks:
  net:                                                                              
```
✅ Step 4: Run the file
```sh
docker-compose up -d
```
✅ Step 5: Browse the ip
```sh
http://localhost:8080
```
✅ Step 5: Get the password in 2 ways
 - get the location of the password when open jenkins in browser
   ```sh
   docker exec jenkins cat /var/lib/jenkins/secrets/initialAdminPassword
   OR
   docker exec -it 0e70fabd682c /bin/bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
 - use this caommnd ```docker logs -f jenkins``` and scroll up then you see the password

### Install jenkins by docker command
✅ Step 1: Pull Jenkins Image
```sh
docker pull jenkins/jenkins:lts
```
✅ Step 2: Run Jenkins Container
```sh
docker run -d \
  --name jenkins-docker \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```
 - -v jenkins_home:/var/jenkins_home: Creates a volume for persistent Jenkins data
 - -p 8080:8080: Jenkins Web UI
 - -p 50000:50000: For Jenkins agents/slaves

✅ Step 3: Get Admin Password
```sh
docker exec jenkins-docker cat /var/jenkins_home/secrets/initialAdminPassword
```
Access via:
```sh
http://localhost:8080
```

### 🧼 Optional: Stop and Remove Jenkins
```sh
# Docker version
docker rm -f jenkins-docker

# Native version
sudo systemctl stop jenkins
sudo apt remove --purge jenkins -y
```
