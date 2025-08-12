### day-12 Project: Explain docker compose and Dockerize a simple PHP/NodeJS app + DB using Docker Compose  

### ✅ What is Docker Compose?
Docker Compose is a tool that allows you to define and run multi-container Docker applications using a single YAML file (docker-compose.yml). Instead of running multiple docker run commands, you can use Compose to bring everything up with a single command:
```sh
docker-compose up
```
Install docker compose latest version in ubuntu
```sh
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```
Check the version 
```sh
docker-compose version
```


### 🛠️ Hands-On Docker Compose Example: PHP + MySQL App
✅ Step 1: Project Structure
```sh
lemp-project/
│
├── docker-compose.yml
│
├── nginx/
│    └── default.conf
├── php/
│    └── Dockerfile
├── www/
│    └── index.php
```

### 📄 1. docker-compose.yml
```sh
version: '3.9'

services:
  nginx:
    image: nginx:latest
    ports:
      - "8081:80"
    volumes:
      - ./www:/var/www/html
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php
    networks:
      - lempnet

  php:
    build: ./php
    volumes:
      - ./www:/var/www/html
    networks:
      - lempnet

  mysql:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: exampledb
      MYSQL_USER: user
      MYSQL_PASSWORD: userpassword
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - lempnet

volumes:
  db_data:

networks:
  lempnet:
```

### 📄 2. nginx/default.conf
```sh
server {
    listen 80;
    server_name localhost;

    root /var/www/html;
    index index.php index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include fastcgi_params;
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

### 📄 3. php/Dockerfile
```sh
FROM php:8.2-fpm

# Optional: install extensions (e.g., mysqli for MySQL)
RUN docker-php-ext-install mysqli pdo pdo_mysql

WORKDIR /var/www/html
```

### 📄 4. www/index.php
```sh
<?php
echo "<h1>LEMP Stack is Working!</h1>";

$host = 'mysql';
$db   = 'exampledb';
$user = 'user';
$pass = 'userpassword';

$conn = new mysqli($host, $user, $pass, $db);

if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
echo "<p>Connected to MySQL successfully!</p>";

$conn->close();
?>
```

### ✅ How to Run
docker compose up -d
<br>
Then visit:
👉 http://localhost:8081
You should see:
  - A "LEMP Stack is Working!" message
  - A successful MySQL connection message

## 🧬 Docker Compose Command list
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
