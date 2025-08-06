### project 1 : Deploy an E-commerce Website with PHP & MySQL Using Shell Script | Step-by-Step DevOps Project

### Deploy a complete php and mysql website by shell script
### Install MariaDB
```sh
sudo apt  install -y mariadb-server
systemctl status mariadb
```
### Configure Database by command
Login to mysql 
```sh
sudo mysql
```
Then run the following command
```sh
CREATE DATABASE ecomdb;
```
```sh
CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
```
```sh
GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'localhost';
```
```sh
 FLUSH PRIVILEGES;
```
```sh
exit
```
## Configure databse by script
```sh
cat > configure-db.sql <<-EOF
CREATE DATABASE ecomdb;
CREATE USER 'ecomuser'@'localhost' IDENTIFIED BY 'ecompassword';
GRANT ALL PRIVILEGES ON *.* TO 'ecomuser'@'localhost';
FLUSH PRIVILEGES;
EOF
```
### Instert this file to mysql
```sh
sudo mysql < configure-db.sql
```

### Create the db-load-script.sql
```sh
cat > db-load-script.sql <<-EOF
USE ecomdb;
CREATE TABLE products (id mediumint(8) unsigned NOT NULL auto_increment,Name varchar(255) default NULL,Price varchar(255) default NULL, ImageUrl varchar(255) default NULL,PRIMARY KEY (id)) AUTO_INCREMENT=1;

INSERT INTO products (Name,Price,ImageUrl) VALUES ("Apple","100","c-1.png"),("Orange","200","c-2.png"),("Lemon","300","c-3.png"),("Potato","50","c-5.png"),("Fish","90","c-6.png"),("Chicken","20","c-7.png"),("Beef","80","c-8.png"),("Apple","150","c-4.png");

EOF
```
### Instert the file to mysql
```sh
sudo mysql < db-load-script.sql
```

### Install required packages 
```sh
sudo apt  install -y apache2 php php-mysql
```
### Delete the /var/www/html folder and clone the code to /var/www//html
```sh
sudo rm -fr /var/www/html/
sudo git clone https://github.com/rajivsiddiqui/project-7-shell-script.git /var/www/html
```
                 
### Browse the website
```sh
curl http://localhost
```





