<h1>DO NOT USE THIS IN PRODUCTION/EXPOSE TO INTERNET. USE THIS FOR PENTESTING/LEARNING ONLY!</h1>
<br>
<h1>Installing Wordpress in Docker</h1>

1. Install Docker<br>
   `sudo apt install docker`
2. Install Docker-Compose<br>
   `sudo apt install docker-compose`
3. Copy This command<br>
  `sudo mkdir /var/www/wordpress && cd /var/www/wordpress`
4. Copy script below and create a new file called **docker-compose.yml**

**COPY FROM START TO FINISH!!** 
**DON'T FORGET TO CHANGE ALL THE SENSITIVE INFO**
```
version: "3"
services:
  db:
    image: mysql:latest
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: MySQLRootPassword <--- CHANGE!!
      MYSQL_DATABASE: MySQLDatabaseName <--- CHANGE!!
      MYSQL_USER: MySQLUsername <--- CHANGE!!
      MYSQL_PASSWORD: MySQLUserPassword <--- CHANGE!!
    volumes:
      - mysql:/var/lib/mysql

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    restart: always
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: MySQLUsername <--- CHANGE!! -- TO MYSQL FROM ABOVE
      WORDPRESS_DB_PASSWORD: MySQLUserPassword <--- CHANGE!! -- TO MYSQL FROM ABOVE
      WORDPRESS_DB_NAME: MySQLDatabaseName <--- CHANGE!! -- TO MYSQL FROM ABOVE
    volumes:
      - "./:/var/www/html"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - "127.0.0.1:8080:80" # Bind to 127.0.0.1 on host, map to port 80 in container
    environment:
      PMA_HOST: db
      PMA_USER: MySQLUsername <--- CHANGE!! -- TO MYSQL FROM ABOVE
      PMA_PASSWORD: MySQLUserPassword <--- CHANGE!! -- TO MYSQL FROM ABOVE
    depends_on:
      - db

volumes:
  mysql: {}
```
5. Paste this command while in **/vaar/www/wordpress FOLDER -- You basically need to have the file in the same folder as the directory you are in**
   `sudo docker-compose up -d`<br>
   6. Wordpress should be installed within couple minutes<br>
   7. Access your WP Website at `http://localhost:80 / http//[IP]/wp-admin/install.php`
