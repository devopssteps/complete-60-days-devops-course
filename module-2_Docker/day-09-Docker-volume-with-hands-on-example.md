# Day-09 : Docker volume with hands on
## 🔹 What is a Docker Volume?
A Docker volume is a persistent storage mechanism used to save data generated and used by Docker containers.<br>

Without a volume:
 - When a container stops or is removed, its data is lost.

With a volume:
 - You can persist, share, and reuse data across containers, even after a restart or deletion.

### 🔧 Types of Docker Storage
 - Volumes (Managed by Docker) ✅ Recommended
 - Bind mounts (Host path to container path)
 - tmpfs (In-memory, non-persistent)

### 🛠 Hands-On Docker Volume Demo
Let’s walk through a practical example using the official nginx image.

### 🗂 Step 1: Create a Volume
```sh
docker volume create mydata
```
Verify:
```sh
docker volume ls
```

### 🌐 Step 2: Run Nginx with Volume Mounted
```sh
docker run -d --name nginx-demo -p 8081:80 -v mydata:/usr/share/nginx/html nginx
```
 - -v mydata:/usr/share/nginx/html: Mounts the volume mydata to the Nginx web root.

### 📝 Step 3: Copy a Custom HTML Page into Volume
Create an index.html file locally:
```sh
echo "<h1>Hello from Docker Volume</h1>" > index.html
```
### 🌐 Step 4: Open Your Browser
Visit:
```sh
http://localhost:8080
```
✅ You’ll see: Hello from Docker Volume

### 🧪 Step 5: Stop and Remove the Container
```sh
docker rm -f nginx-demo
```
Now run a new container using the same volume:
```sh
ocker run -d --name nginx-demo2 -p 8081:80 -v mydata:/usr/share/nginx/html nginx
```
🔁 The HTML content persists, even though the old container is gone.

## Bind mount demo
### First create a folder in your local pc
```sh
mkdir data
```
Now create a container with this data folder
```sh
docker container run -d --name b1 --mount type=bind,source=/home/ubuntu/data,destination=/abcd centos:7
```
Now login to the conatiner and craete folder 
```sh
docker exec -it <container-id> /bin/bash
```
Now create a file
```sh
echo "welcome" > /abcd/a.txt
```
Exit from the container and delete 
```sh
docker rm -f <contaienr-id>
```
Now create a file in our local folder abcd
```sh
echo "devops"> /abcd/b.txt
```
Now create anoter container with using same mount folder
```sh
docker container run -d --name b1 --mount type=bind,source=/home/ubuntu/data,destination=/abcd centos:7
```
Now login to container and we see the old and new craeted container both are there
```sh
docker exec -it <container-id> /bin/bash
```

### 🧹 Cleanup (Optional)
```sh
docker rm -f nginx-demo2
docker volume rm mydata
```
### Volume command list
```sh
docker volume create <name>                 # Create volume
docker volume ls                            # List volumes
docker volume inspect <name>                # Inspect volume
docker volume rm <name>                     # Remove volume
docker run -v <volume>:/path <image>        # Use named volume
```

### 💡 Why Use Docker Volumes?
 - Keeps app data safe across restarts
 - Easy backups and restores
 - Data sharing between containers
 - Managed and isolated by Docker (no path conflicts)










