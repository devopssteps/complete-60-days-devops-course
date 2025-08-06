
## Day 07 - Docker Concepts With Hands On Example 

### 🧠 Basic Concepts You Should Explain:
What is Docker?
 - A platform for developing, shipping, and running applications inside lightweight containers.

Image vs Container
 - Image: Blueprint
 - Container: Running instance of an image

Dockerfile
 - Instructions to build an image

Docker Hub
 - Public registry for storing and sharing Docker images

Docker Compose
 - Tool for defining and running multi-container Docker apps

Volumes
 - For persistent storage

Networking
 - Bridge, host, none, and custom networks

## 🛠 Hands-On Demo Structure (Beginner Friendly)


### ✅ 1. Run Your First Container
```sh
docker run -d -p 8082:80 httpd
```

### ✅ 2. Pull and Run a Web Server (e.g., Nginx)
```sh
docker pull nginx
docker run -d -p 8080:80 nginx
```

### ✅ 3. Create a Dockerfile and Build an Image
```sh
FROM ubuntu:latest

RUN apt-get update && apt-get install -y nginx
WORKDIR /var/www/html/
COPY index.html .
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```
Create the image
```sh
docker build -t d3 .
```
Run the file
```sh
docker run -d -p 8081:80 d3
```

### ✅ 4. Docker Compose Example
docker-compose.yml
```sh
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
```
Run:
```sh
docker-compose up
```

### Another docker file example
Create Dockerfile
```sh
# Use base image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy app files
COPY app.py .

# Run app
CMD ["python", "app.py"]
```

Create app.py file
```sh
print("Hello from inside the container!")
```
Build & Run
```sh
docker build -t my-python-app .
docker run my-python-app
```
