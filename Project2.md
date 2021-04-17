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










### To save and close the file, simply follow the steps below:

- Hit the esc button on the keyboard
- Type :
- Type wq. w for write and q for quit
- Hit ENTER to save the file

Used the below cmdlet to show the new file we created in the sites-available directory

`$ sudo ls /etc/apache2/sites-available`

See screenshot of the above cmds ran

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/Virtualhost.PNG)
![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/Virtualhost2.PNG)


Enabled the new virtualhost created with the below

`$ sudo a2ensite projectlamp`

Disabled the default website that comes installed with Apache with the below

`$ sudo dissite 000-default`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/enable-disableVH.PNG)


Ran the below cmd to make sure the config file does not contain any syntax error

`$ sudo apache2ctl configtest`

Used the below to reload Apache so the changes can take effect

`$ sudo systemctl reload apache2`


![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/config-test.PNG)


Created an index.html file in the */var/www/projectlamp* location for testing if the virtual host works fine

`sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`


The below screen shot is the output which shows Virtual host is working properly


![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/config-test-output.PNG)




**Step 5** - Enable PHP on the website

We needed to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive so as to allow the php page be the landing page

`sudo vim /etc/apache2/mods-enabled/dir.conf`

```xml

<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

```

![](https://github.com/Tofumy/Tofumy-PBL/blob/main/change-dirconf.PNG)


Create a new file named index.php inside your custom web root folder:

`$ vim /var/www/projectlamp/index.php`


```php

<?php
phpinfo();

```

Reloaded apache for changes to take place

`$ sudo systemctl reload apache2`

![screenshot](https://github.com/Tofumy/Tofumy-PBL/blob/main/indexphp.PNG)



The output shows that PHP installation is working as expected

![PHP landing](https://github.com/Tofumy/Tofumy-PBL/blob/main/landingphp.PNG)
