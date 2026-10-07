# DEVOPS PROJECT - PROJECT 2

# WEB STACK IMPLEMENTATION
## (LEMP STACK)

## PREPARING PREREQUISITES

For this project, an AWS EC2 instance was created using Ubuntu Server 22.04 LTS.

The EC2 instance was used as the server environment for implementing the LEMP stack.

The LEMP stack consists of:

- Linux
- Nginx
- MySQL
- PHP

The Ubuntu server was accessed through AWS EC2 Instance Connect.

![AWS EC2 Ubuntu Server](AWS_EC2.png)


# STEP 1 - INSTALLING THE NGINX WEB SERVER

Nginx was installed as the web server component of the LEMP stack.

First, the server's package index was updated.

    sudo apt update

Nginx was then installed using:

    sudo apt install nginx

After installation, the Nginx service was checked to verify that it was running.

    sudo systemctl status nginx

The Nginx configuration was also tested using:

    sudo nginx -t

The configuration test returned:

    nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
    nginx: configuration file /etc/nginx/nginx.conf test is successful

Nginx was successfully installed and confirmed to be running.

![Nginx Installation](NGINX_INSTALL.png)

![Nginx Service Running](NGINX_STATUS.png)

The Nginx server was then accessed through the public IP address of the EC2 instance.

The default Nginx welcome page was successfully displayed in the browser.

![Nginx Welcome Page](NGINX_BROWSER.png)


# STEP 2 - INSTALLING MYSQL

MySQL was installed as the database component of the LEMP stack.

The installation was performed using:

    sudo apt install mysql-server

The installation confirmed that MySQL Server 8.0.46 was installed on the Ubuntu server.

![MySQL Installation](MYSQL_INSTALL.png)

The MySQL service was then checked to verify that it was running correctly.

    sudo systemctl status mysql

MySQL was successfully installed and configured as the database server for the project.


# STEP 3 - INSTALLING PHP

PHP was installed to provide server-side processing and allow PHP applications to communicate with MySQL.

The required PHP packages were installed using:

    sudo apt install php-fpm php-mysql

The PHP version was verified using:

    php -v

PHP-FPM was also checked to confirm that the PHP FastCGI Process Manager was running.

    sudo systemctl status php8.1-fpm

The PHP-FPM service was confirmed to be active and running.

![PHP-FPM Service](PHP_FPM.png)

PHP was successfully installed and configured as the server-side scripting component of the LEMP stack.


# STEP 4 - CONFIGURING NGINX
## TO USE PHP PROCESSOR

A dedicated web root directory was created for the Project LEMP website.

    sudo mkdir /var/www/projectLEMP

Ownership of the directory was assigned to the current user.

    sudo chown -R $USER:$USER /var/www/projectLEMP

An Nginx server block was created for the Project LEMP website.

The configuration used the following structure:

    server {
        listen 80;
        server_name projectLEMP www.projectLEMP;

        root /var/www/projectLEMP;
        index index.html index.htm index.php;

        location / {
            try_files $uri $uri/ =404;
        }

        location ~ \.php$ {
            include snippets/fastcgi-php.conf;
            fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        }

        location ~ /\.ht {
            deny all;
        }
    }

The Nginx configuration was tested for syntax errors using:

    sudo nginx -t

The configuration test returned:

    syntax is ok
    test is successful

Nginx was then reloaded to apply the configuration.

    sudo systemctl reload nginx

The Nginx configuration was successfully applied and connected to PHP-FPM.

![Nginx Configuration Test](NGINX_CONFIG.png)


# STEP 5 - TESTING PHP WITH NGINX

A PHP test file was created in the Project LEMP web root.

The file was created at:

    /var/www/projectLEMP/info.php

The PHP test file contained:

    <?php
    phpinfo();
    ?>

The PHP page was accessed through the web browser using the EC2 public IP address followed by:

    /info.php

The PHP information page displayed the installed PHP environment.

The page confirmed that PHP 8.1.2 was running with:

    Server API: FPM/FastCGI

This confirmed that Nginx was successfully processing PHP through PHP-FPM.

![PHP Information Page](PHP_INFO.png)

After testing, the PHP information file was removed because it contains detailed information about the PHP environment and server configuration.

    sudo rm /var/www/projectLEMP/info.php


# STEP 6 - RETRIEVING DATA FROM MYSQL
## WITH PHP

A MySQL database was created for the Project LEMP application.

The database was named:

    example_database

A database user named `example_user` was created and granted access to the project database.

The database was accessed and the `todo_list` table was created.

The table used the following structure:

    CREATE TABLE todo_list (
        item_id INT AUTO_INCREMENT,
        content VARCHAR(255),
        PRIMARY KEY(item_id)
    );

An item was inserted into the TODO list.

The database contents were then checked using:

    SELECT * FROM todo_list;

The TODO list initially contained:

    My first important item

The item was then updated using:

    UPDATE todo_list
    SET content = 'Updated important item'
    WHERE item_id = 1;

The updated database contents were verified using:

    SELECT * FROM todo_list;

The final database result showed:

    Updated important item

![MySQL Todo List](MYSQL_TODO.png)


## TESTING DATABASE ACCESS WITH THE PROJECT USER

The `example_user` account was tested to confirm that it could access the project database.

The following command was used:

    mysql -u example_user -p -h 127.0.0.1 -e "USE example_database; SHOW TABLES;"

The command successfully displayed:

    todo_list

This confirmed that `example_user` could access the `example_database` database and its `todo_list` table.

![Database Access Test](DATABASE_ACCESS.png)


## CREATING THE PHP TODO APPLICATION

A PHP file was created in the Project LEMP web root.

The file was:

    /var/www/projectLEMP/todo_list.php

The PHP application was configured to connect to MySQL using the project database and database user.

The application retrieved the contents of the `todo_list` table and displayed the records as an HTML TODO list.

The PHP application was tested locally using:

    curl http://localhost/todo_list.php

The request successfully returned the TODO information from MySQL.

![PHP MySQL Test](PHP_MYSQL_TEST.png)


## FINAL BROWSER TEST

The completed PHP application was accessed through the EC2 public IP address using:

    http://44.198.52.20/todo_list.php

The browser successfully displayed:

    TODO

    1. Updated important item

This confirmed that Nginx successfully processed the PHP application, PHP successfully connected to MySQL, and the data was successfully retrieved from the MySQL database.

![Final TODO List](TODO_FINAL.png)


# CONCLUSION

The LEMP stack was successfully implemented on an AWS EC2 Ubuntu server.

The following components were installed and configured:

* Linux: Ubuntu Server 22.04 LTS
* Nginx: Web server
* MySQL: Database server
* PHP: Server-side scripting language
* PHP-FPM: PHP processor

Nginx was successfully installed and verified as running.

A dedicated project web root was created at:

    /var/www/projectLEMP

Nginx was configured to process PHP files through PHP-FPM.

PHP was successfully tested through Nginx using the PHP information page.

A MySQL database named `example_database` was created together with the `example_user` database account.

The `todo_list` table was created and populated with data.

The PHP application successfully connected to MySQL and retrieved the stored TODO item.

The final browser test successfully displayed:

    TODO

    1. Updated important item

This confirmed that the complete Nginx, PHP, and MySQL stack was operational.

The Project 2 LEMP web stack implementation was therefore successfully completed and verified.
