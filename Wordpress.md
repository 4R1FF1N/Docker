<h1>DO NOT USE THIS IN PRODUCTION/EXPOSE TO INTERNET. USE THIS FOR PENTESTING/LEARNING ONLY!</h1>
<br>
<h1>Installing Wordpress in Docker</h1>

1. Install Docker<br>
   `sudo apt install docker`
2. Install Docker-Compose<br>
   `sudo apt install docker-compose`
3. Copy This command<br>
  `sudo mkdir /srv/wordpress && cd /srv/wordpress`
4. Copy script below and create a new file called **docker-compose.yml**

**COPY FROM START TO FINISH!!** (thanks to #Hostinger for providing a good .yml file)
#https://www.hostinger.co.uk/tutorials/run-docker-wordpress
```
version: "3" 
services:
  db:
    image: mysql:latest
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: MySQLRootPassword
      MYSQL_DATABASE: MySQLDatabaseName
      MYSQL_USER: MySQLUsername
      MYSQL_PASSWORD: MySQLUserPassword
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    restart: always
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: MySQLUsername
      WORDPRESS_DB_PASSWORD: MySQLUserPassword
      WORDPRESS_DB_NAME: MySQLDatabaseName
    volumes:
      - "./:/var/www/html"
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - "8080:80"
    environment:
      PMA_HOST: db
      PMA_USER: MySQLUsername
      PMA_PASSWORD: MySQLUserPassword
volumes:
  mysql: {}
```
5. Paste this command while in **/srv/wordpress FOLDER**
   `sudo docker-compose up -d`<br>
   6. Wordpress should be installed within couple minutes<br>
   7. Access your WP Website at `http://localhost / http//[IP]/wp-admin/install.php`
