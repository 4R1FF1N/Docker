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
version: '3.9'

services:
  wordpress:
    image: wordpress:6.6.2-php8.2-apache
    container_name: wordpress
    restart: unless-stopped
    ports:
      - "80:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: ${DB_PASSWORD:-securepassword}
      WORDPRESS_TABLE_PREFIX: wp_
    volumes:
      - wordpress_data:/var/www/html
      - ./wp-content:/var/www/html/wp-content
    depends_on:
      - db
    networks:
      - wp-network

  db:
    image: mysql:8.0
    container_name: wordpress_db
    restart: unless-stopped
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: ${DB_PASSWORD:-securepassword}
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD:-rootsecurepassword}
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - wp-network

  phpmyadmin:
    image: phpmyadmin:5.2
    container_name: phpmyadmin
    restart: unless-stopped
    ports:
      - "127.0.0.1:8081:80"
    environment:
      PMA_HOST: db
      PMA_USER: root
      PMA_PASSWORD: ${DB_ROOT_PASSWORD:-rootsecurepassword}
      UPLOAD_LIMIT: 64M
    depends_on:
      - db
    networks:
      - wp-network

volumes:
  wordpress_data:
  db_data:

networks:
  wp-network:
    driver: bridge
```

5. Create an .env file and paste this:
   ```
   DB_PASSWORD=your_secure_password
   DB_ROOT_PASSWORD=your_secure_root_password
   ```
5. Paste this command while in **/var/www/wordpress FOLDER -- You basically need to have the file in the same folder as the directory you are in**
   `sudo docker-compose up -d`<br>
   6. Wordpress should be installed within couple minutes<br>
   7. Access your WP Website at `http://localhost:80 or http://[IP]:80
   8. Access your PHPMyAdmin at 'http://localhost:8081'
