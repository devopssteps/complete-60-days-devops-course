## day 08: Docker installation,Docker Registry, Docker Images, Containers & most commonlly docker command we use 

### Install Docker 
```sh
sudo apt update
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```
Another way to install docker by shell script
```sh
#!/bin/bash
# Add Docker's official GPG key:
sudo apt-get update -y
sudo apt-get install ca-certificates curl -y
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update -y
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo usermod -aG docker ubuntu	
newgrp docker
```
### Docker hub account create 
URL: https://hub.docker.com/

### Docker command 
## 🧪 Docker Setup & Version Check
```sh
docker --version              # Check Docker version
docker info                   # View Docker system info
docker help                   # Help for Docker CLI
```

## 📦 Images
```sh
docker pull <image>                      # Download image from Docker Hub
docker images                            # List downloaded images
docker rmi <image>                       # Remove image
docker tag <image> <new-name:tag>        # Tag image
docker build -t <name:tag> .             # Build from Dockerfile
docker inspect <image>                   # Get image details
docker history <image>                   # Show image layer history
```

## 🚀 Containers
```sh
docker run <image>                           # Run container from image
docker run -it <image> /bin/bash             # Interactive terminal session
docker run -d <image>                        # Run in detached mode
docker run --name <name> <image>             # Assign container name
docker run -p 8080:80 <image>                # Map port 8080 (host) to 80 (container)
docker run -v /host:/container <image>       # Mount volume
docker run --env KEY=value <image>           # Set environment variable

docker ps                                    # List running containers
docker ps -a                                 # List all containers
docker stop <container>                      # Stop running container
docker start <container>                     # Start a stopped container
docker restart <container>                   # Restart container
docker rm <container>                        # Remove stopped container
docker exec -it <container> /bin/bash        # Open shell in container
docker logs <container>                      # View logs of container
docker inspect <container>                   # View container details
```

## 🧹 Clean Up
```sh
docker stop $(docker ps -q)                 # Stop all running containers
docker rm $(docker ps -a -q)                # Remove all containers
docker rmi $(docker images -q)              # Remove all images
docker system prune                         # Remove unused data (images, containers, volumes)
```

## 📄 Save & Load Images
```sh
docker save -o myimage.tar <image-name>     # Save image to tar file
docker load -i myimage.tar                  # Load image from tar file
```

## 🔐 Docker Login & Push (Docker Hub)
```sh
docker login                                # Login to Docker Hub
docker tag <image> username/repo:tag        # Tag image for Docker Hub
docker push username/repo:tag               # Push image to Docker Hub
docker pull username/repo:tag               # Pull image from Docker Hub
```

