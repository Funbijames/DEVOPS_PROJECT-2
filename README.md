# DEVOPS_PROJECT-2
DevOps Project 2: Web Stack Implementation using Nginx, MySQL and PHP on an AWS EC2 Ubuntu server.
# DEVOPS PROJECT - PROJECT 2

# WEB STACK IMPLEMENTATION
## (LEMP STACK)

# STEP 1 - INSTALLING THE NGINX WEB SERVER

Nginx was installed as the web server component of the LEMP stack.

First, the server's package index was updated.

    sudo apt update

Nginx was then installed using:

    sudo apt install nginx

After installation, the Nginx service was checked to verify that it was running.

    sudo systemctl status nginx

The Nginx server was also tested locally using:

    curl http://localhost

The successful response confirmed that Nginx was installed and serving web pages correctly.

The Nginx web server was then accessed through the public IP address of the EC2 instance using a web browser.

![Nginx Web Server](NGINX_SCREENSHOT.png)


# STEP 2 - INSTALLING MYSQL

MySQL was installed as the database component of the LEMP stack.

The installation was performed using:

    sudo apt install mysql-server

After installation, the MySQL service was checked to confirm that it was running correctly.

    sudo systemctl status mysql

MySQL was accessed through the MySQL console.

    sudo mysql

MySQL was successfully installed and the database server was operational.

![MySQL Server Running](MYSQL_SCREENSHOT.png)


# STEP 3 - INSTALLING PHP

PHP was installed to provide server-side processing and allow PHP applications to communicate with the MySQL database.

The required PHP packages were installed using:

    sudo apt install php-fpm php-mysql

The installed PHP version was verified using:

    php -v

The PHP-FPM service was configured as the PHP processor for the Nginx web server.

The PHP components were successfully installed.

![PHP Installation](PHP_SCREENSHOT.png)


# STEP 4 - CONFIGURING NGINX
## TO USE PHP PROCESSOR

A dedicated web root directory was created for the Project LEMP website.

    sudo mkdir /var/www/projectLEMP

Ownership of the directory was assigned to the current user.

    sudo chown -R $USER:$USER /var/www/projectLEMP

An Nginx server block was configured for the Project LEMP website.

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

The configuration was successfully validated.

Nginx was then reloaded to apply the configuration.

    sudo systemctl reload nginx

The Nginx configuration was successfully applied and connected to PHP-FPM.

![Nginx PHP Configuration](NGINX_CONFIG_SCREENSHOT.png)


# STEP 5 - TESTING PHP WITH NGINX

A PHP test file was created in the Project LEMP web root.

The test file was:

    /var/www/projectLEMP/info.php

The PHP test file contained:

    <?php
    phpinfo();
    ?>

The PHP page was accessed through the web browser using the EC2 public IP address followed by `/info.php`.

The PHP information page confirmed that Nginx was correctly processing PHP through PHP-FPM.

After testing, the PHP information file was removed because it contains detailed information about the PHP environment and server configuration.

    sudo rm /var/www/projectLEMP/info.php

PHP was successfully tested with Nginx.

![PHP Test](PHP_TEST_SCREENSHOT.png)


# STEP 6 - RETRIEVING DATA FROM MYSQL
## WITH PHP

A MySQL database was created for the Project LEMP application.

The database was named:

    example_database

A database user was created:

    example_user

The user was granted privileges on the project database.

The database was then accessed and a table was created for the TODO list.

The table structure was:

    CREATE TABLE todo_list (
        item_id INT AUTO_INCREMENT,
        content VARCHAR(255),
        PRIMARY KEY(item_id)
    );

An item was inserted into the TODO list.

The database was then queried to verify that the data had been stored successfully.

The PHP application was configured to connect to the MySQL database using the `example_user` account.

A PHP file was created at:

    /var/www/projectLEMP/todo_list.php

The PHP script was configured to retrieve the contents of the `todo_list` table from `example_database`.

The PHP application used the following database configuration:

    $user = "example_user";
    $password = "MyNewPassword123!";
    $database = "example_database";
    $table = "todo_list";

The PHP script established a connection to MySQL using PDO and retrieved the records from the database.

The application was first tested locally using:

    curl http://localhost/todo_list.php

The successful response displayed the TODO information retrieved from MySQL.

The application was then tested through the web browser using the EC2 public IP address:

    http://44.198.52.20/todo_list.php

The browser successfully displayed:

    TODO
    1. Updated important item

This confirmed that PHP was successfully communicating with MySQL and retrieving data from the database through the Nginx web server.

![TODO List PHP MySQL Test](TODO_LIST_SCREENSHOT.png)


# CONCLUSION

The LEMP stack was successfully implemented on an AWS EC2 Ubuntu server.

The following components were installed and configured:

* Linux: Ubuntu Server
* Nginx: Web server
* MySQL: Database server
* PHP: Server-side scripting language
* PHP-FPM: PHP processor

Nginx was configured with a dedicated project web root:

    /var/www/projectLEMP

Nginx was successfully configured to process PHP files through PHP-FPM.

PHP was tested successfully through Nginx.

A MySQL database named `example_database` was created together with the `example_user` database account.

The `todo_list` table was created and populated with data.

The PHP application successfully connected to MySQL and retrieved the stored TODO item.

The final browser test successfully displayed the database information:

    TODO
    1. Updated important item

This confirmed that the complete Nginx, PHP, and MySQL stack was operational.

The Project 2 LEMP web stack implementation was therefore successfully completed and verified.
