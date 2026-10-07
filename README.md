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


# STEP 1 - INSTALLING THE NGINX WEB SERVER

Nginx was installed as the web server component of the LEMP stack.

First, the server's package index was updated.

    sudo apt update

Nginx was then installed using:

    sudo apt install nginx

After installation, the Nginx service was checked to verify that it was running.

    sudo systemctl status nginx

The Nginx service was confirmed to be active and running.

Nginx was also tested from the browser using the public IP address of the EC2 instance.

The default Nginx welcome page was successfully displayed, confirming that the web server was installed and working.

![Nginx Service Running](NGINX_STATUS.png)

![Nginx Welcome Page](NGINX_BROWSER.png)


# STEP 2 - INSTALLING MYSQL

MySQL was installed as the database component of the LEMP stack.

The installation was performed using:

    sudo apt install mysql-server

The installation confirmed that MySQL Server 8.0.46 was installed on the Ubuntu server.

![MySQL Installation](MYSQL_INSTALL.png)

MySQL was then accessed through the MySQL console to verify that the database server was working correctly.

The MySQL server was successfully installed and configured for the project.


# STEP 3 - INSTALLING PHP

PHP was installed to provide server-side processing and allow PHP applications to communicate with MySQL.

The required PHP packages were installed using:

    sudo apt install php-fpm php-mysql

The PHP-FPM service was checked to confirm that the PHP FastCGI Process Manager was running.

    sudo systemctl status php8.1-fpm

The PHP 8.1 FastCGI Process Manager was confirmed to be active and running.

![PHP-FPM Service](PHP_FPM.png)


# STEP 4 - CONFIGURING NGINX
## TO USE PHP PROCESSOR

A dedicated web root directory was created for the Project LEMP website.

    sudo mkdir /var/www/projectLEMP

Ownership of the directory was assigned to the current user.

    sudo chown -R $USER:$USER /var/www/projectLEMP

Nginx was configured to use the Project LEMP directory as the web root.

The Nginx configuration was also configured to process PHP files through PHP-FPM.

The PHP-FPM socket used by the configuration was:

    /var/run/php/php8.1-fpm.sock

The Nginx configuration was tested for syntax errors using:

    sudo nginx -t

The configuration test returned:

    syntax is ok
    test is successful

Nginx was then reloaded to apply the configuration.

    sudo systemctl reload nginx

The Nginx configuration was successfully applied.

![Nginx Configuration Test](NGINX_CONFIG.png)

A test page was also accessed through the browser to verify that the configured Nginx web root was being served.

![LEMP Test Page](LEMP_TEST.png)


# STEP 5 - TESTING PHP WITH NGINX

A PHP test file was created in the Project LEMP web root.

The file was:

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

The `todo_list` table was created inside the database.

The table contained the following fields:

    item_id
    content

The database contents were checked using:

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
