# Day-11: Real-Time Docker Troubleshooting & most common docker command we need know

### 🛠️ Problem 1: Container Exits Immediately 
🗨️ “You run a container and it exits immediately.”

```sh
docker run ubuntu
```
🔍 Troubleshoot:
```sh
docker ps -a    # See exited container
docker logs <container_id>
```
✅ Fix:
```sh
docker run -it ubuntu bash
```
🧠 Explain: Ubuntu has no default process to keep it running; using -it starts an interactive shell.

### 🐛 3. Problem 2: Port Not Accessible 
🗨️ “App runs, but can’t access it from browser.”
```sh
docker run -d --name web -p 80:81 nginx
```
But visiting localhost doesn't work? Check:
🔍 Troubleshoot:
```sh
docker ps
docker inspect web | grep IPAddress
docker image history  nginx           # we can see contaiern is expose 80 port 
```
✅ Fix: Ensure:
 - App inside container is listening on correct port
 - so chage the port from 81 to 80 then we can see we browse the website
 - Use curl localhost:80 to test
 - Try docker exec -it web bash to check if service is running
    - service nginx status

### 🧱 4. Problem 3: Container Crash Loop
🗨️ “Container starts and keeps restarting.”
```sh
docker run --name app myapp:latest
```
🔍 Troubleshoot:
```sh
docker logs app
```
✅ Fix:
 - Check error logs
 - Validate entrypoint or CMD in Dockerfile
 - Use docker inspect app to analyze config
 - docker exec -it if it stays up momentarily


###  5. Problem 4: Image Build Fails 
🗨️ “Dockerfile build fails with errors.”
```sh
docker build -t myapp .
```
🔍 Common Issues:
  - Bad COPY paths
  - Missing dependencies
  - Wrong syntax in Dockerfile

✅ Fix:
  - Use .dockerignore to speed up
  - Check context path
  - Add RUN echo or ls for debugging during build


🧹 6. Bonus: Clean Up Dangling Images & Containers
```sh
docker system df
docker system prune
docker container prune
docker image prune
```
### 📌 7. Pro Tips: Docker Troubleshooting Cheatsheet 
```sh
# List all containers (including exited)
docker ps -a

# See real-time logs
docker logs -f <container>

# Dive into container
docker exec -it <container> bash

# Inspect networking, volumes, mounts
docker inspect <container>

# View running processes
docker top <container>
```


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

## 📂 Volumes
```sh
docker volume create <name>                 # Create volume
docker volume ls                            # List volumes
docker volume inspect <name>                # Inspect volume
docker volume rm <name>                     # Remove volume
docker run -v <volume>:/path <image>        # Use named volume
```

## 🌐 Networks
```sh
docker network ls                           # List Docker networks
docker network create <network-name>        # Create network
docker network inspect <name>               # Inspect network
docker network connect <net> <container>    # Connect container to network
docker network rm <name>                    # Remove network
```

## 🛠️ Dockerfile / Image Build
```sh
docker build -t <image-name> .              # Build Docker image from Dockerfile
docker build -f <Dockerfile> -t <name> .    # Build using a specific Dockerfile
docker image prune                          # Remove unused images
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

## 🧬 Docker Compose
```sh
docker-compose --version                    # Check Docker Compose version
docker-compose up                           # Start services (foreground)
docker-compose up -d                        # Start in detached mode
docker-compose down                         # Stop and remove services
docker-compose build                        # Build or rebuild services
docker-compose ps                           # List running services
docker-compose logs                         # Show service logs
docker-compose exec <service> /bin/bash     # Open shell in service container
```

## 🧠 Useful One-Liners
```sh
# Run Nginx on port 8080
docker run -d -p 8080:80 nginx

# Run MySQL with environment variables
docker run -d -e MYSQL_ROOT_PASSWORD=root -p 3306:3306 mysql

# Enter container bash
docker exec -it <container_id> /bin/bash
```

## 🎯 Best Practices
```sh
| Task                  | Command                                                 |
| --------------------- | ------------------------------------------------------- |
| Avoid dangling images | `docker image prune -f`                                 |
| Avoid leaking secrets | Use `.env` files and `--env` flags carefully            |
| Automate builds       | Use `Dockerfile` + `docker-compose`                     |
| Tag all images        | Always version your images (e.g., `v1.0.0`, `build-45`) |
| Use `.dockerignore`   | Avoid copying unnecessary files into the image          |
```
