# Project 2

**Step 1** - Installing the Nginx Web Server
---

The below cmdlet installs the nginx server

`$ sudo apt update`

`$ sudo apt install nginx`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/sudo-apt1.JPG)

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/sudo-apt-install.JPG)

We used the below cmdlet to confirm that Nginx server is active

`$ sudo systemctl status nginx`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/systemctl-status.JPG)

Enabled the TCP Port 22 for the EC2 instance

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/inbound-rule.JPG)

We just used it to confirm if we can access the nginx server locally 

`$ curl http://localhost:80`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/curl-localhost.JPG)

`$ curl http://3.138.202.128:80`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/curl-publicip.JPG)

We tried to access the public Ip over the browser and below is the output

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/browser-nginx.JPG)




**Step 2** - Installing MySQL

The below cmdlet installs the mysql in the server

`$ sudo apt install mysql-server`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/install-mysql.JPG)

The below cmdlet runs a security script that removes insecure default settings and lock down access to the database system

`$ sudo mysql_secure_installation`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/secure-sql.JPG)


This code verifies that we can log in to the MySQL server

`$ sudo mysql`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/test-mysql.JPG)



**Step 3** - Installing PHP

The below cmdlet installs *php-fpm* (PHP fastCGI process manager) and *php-msql* (PHP module to communicate with MySQL-based databases)

`$ sudo apt install php-fpm php-mysql`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/php-install.JPG)



**Step 4** - Configuring Nginx to Use PHP Processor


Created a directory for projectLEMP using ‘mkdir’ command

`$ sudo mkdir /var/www/projectLEMP`

Assigned ownership to the directory with this variable $USER which still referenced the system user

`$ sudo chown -R $USER:$USER /var/www/projectLEMP`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/mk-directory.JPG)

Created and opened a new configuration file in Nginx’s sites-available directory using "nano" command-line editor

`$ sudo nano /etc/nginx/sites-available/projectLEMP`

Pasted the below bare-bones configuration and used Ctrl + X to save it

```xml

#/etc/nginx/sites-available/projectLEMP

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
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}

```
![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/nano-insert.JPG)
![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/nano-insert2.JPG)
![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/nano-insert3.JPG)


We used the below to activate the configuration by linking to the config file from Nginx's *sites-enabled* directory

`$ sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

We tested the configuration for syntax error using the below cmdlet:

`$ sudo nginx -t`

The output showing that everything is fine

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/sudo-nginx-test.JPG)


Disabled default Nginx host that is currently configured to listen on port 80

`sudo unlink /etc/nginx/sites-enabled/default`

Reload the Nginx to apply the changes

`$ sudo systemctl reload nginx`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/nginx-reload-unlink.JPG)


We then created an index.html file in the */var/www/projectLEMP* to test if the new server block works

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/nginx-curl-test.JPG)

We access the public IP on the browser to get what we echo

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/nginx-browser-output.JPG)



**Step 5** - Testing PHP with Nginx

Created a test PHP file in the document root using nano text editor, this is to validate that Nginx can correctly hand *.php* files off to the PHP Processor

`$ nano /var/www/projectLEMP/info.php`


![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/nginx-phpinfo.JPG)

Tried to access the link *http://3.143.233.240/info.php* and we got the below


![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/nginx-browser-phpinfo.JPG)


You can use the below to remove the file for security purpose
`$ sudo rm /var/www/your_domain/info.php`





**Step 6** - Retrieving data from MySQL database with PHP


Connected to the MySQL console using the root account:

`$ sudo mysql`

Created a new database *Tofumy_DB*

`CREATE DATABASE `Tofumy_DB`;`

Created a new user and granted the user full privileges on the new database created. We used /'mysql_native_password/' as default authentication method

`CREATE USER 'Tofumy'@'%' IDENTIFIED WITH mysql_native_password BY 'damilola';`

Gave "Tofumy" permission over the "Tofumy_DB" database:

`GRANT ALL ON Tofumy_DB.* TO 'Tofumy'@'%';`

So we exited the sql session


![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/DB_Create.JPG)


Tested to see if we can logging to the SQL Server with that user created and confirmed if we could see the Database created:

`$ mysql -u Tofumy -p`

`SHOW DATABASES`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/DB_login.JPG)


Then we create a To-do list table:

``` mysql

CREATE TABLE example_database.todo_list (
item_id INT AUTO_INCREMENT,
content VARCHAR(255),
PRIMARY KEY(item_id)
);

```

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/Table_Create.JPG)


Inserted a few rows of content in the todo_list table:

`INSERT INTO Tofumy_DB.todo_list (content) VALUES ("I want to finish my DevOps Project")`

`INSERT INTO Tofumy_DB.todo_list (content) VALUES ("I want to watch some advanced linux videos")`


![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/Table_Insert.JPG)


To confirm that the data was successfully saved to the table, we wll run the below:

`SELECT * FROM Tofumy_DB.todo_list;`

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/Table_Select.JPG)


Created a new PHP file in your custom web root directory using your nano editor. 

`$ nano /var/www/projectLEMP/todo_list.php`

``` php
<?php
$user = "Tofumy";
$password = "damilola";
$database = "Tofumy_DB";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO-LIST</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}

```

![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/todo_list_php.JPG)


We test and try to access the public Ip address in the browser *http://3.143.233.240/todo_list.php*


![screenshot](https://github.com/Tofumy/Tofumy-PBL2/blob/main/todo_list_output.JPG)


**THE END**


