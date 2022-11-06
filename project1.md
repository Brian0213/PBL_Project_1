## Documentation for Project 1: LAMP STACK IMPLEMENTATION 

**Preparing prerequisites**

1. AWS account setup and Provisioning an Ubuntu Server

2. Select your preferred region (the closest to you) and launch a new EC2 instance of t2.micro family with Ubuntu Server 20.04 LTS (HVM)

3. Create a private key (.pem file) and save in a secure place and do not share it with anyone! If you lose it, you will not be able to connect to your server ever again!

4. To connect to your instance, do the following:

- Install the windows terminal tool.

In the terminal, change to the folder where the PEM key is stored using the command below (if is still in your download folder):

`cd Downloads`

- Connect to the instance by running

`ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>` 

1. **Installing apache and updating the firewall**

#update a list of packages in package manager
`sudo apt update`

#run apache2 package installation

`sudo apt install apache2`

![Apache Install Status](./image/apache-install-status.PNG)

#To verify that apache2 is running as a Service in our OS, use following command:

`sudo systemctl status apache2`

![Apache Systemctl Status](./image/apache-systemctl-status.PNG)

#Edit Inbound Rule for Port 80: As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80.

![TCP Port80 Inbound Rule Status](./image/tcp-port80-inbound-rule-status.PNG)

#First, let us try to check how we can access it locally in our Ubuntu shell, run:

`curl http://localhost:80`

`curl http://127.0.0.1:80`

![Curl Http Localhost80 Status](./image/curl-http-localhost80.PNG)

#Another way to retrieve your Public IP address, other than to check it in AWS Web console, is to use following command:

`curl -s http://169.254.169.254/latest/meta-data/public-ipv4`

![Retrieve Public IP Address Status](./image/retrieve-public-ip-address-status.PNG)

#The URL in browser shall also work if you do not specify port number since all web browsers use port 80 by default.

[Apache Http Url](http://3.19.123.237:80)

[Apache Http Url](http://3.19.123.237)


![Apache Http Server Status](./image/apache-http-server.PNG)


2. **Installing MySql**

#Again, use ‘apt’ to acquire and install this software:

`sudo apt install mysql-server`

#When prompted, confirm installation by typing Y, and then ENTER.

![Apt Install Mysql Server](./image/apt-install-server-status.PNG)

#When the installation is finished, log in to the MySQL console by typing:

`sudo mysql`

![Mysql Console Status](./image/mysql-console-status.PNG)

#It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Before running the script you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.1.

`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

![Root Password Status](./image/script-root-password-status.PNG)

#Exit the MySQL shell with:

`exit`

![Exit Mysql Shell Status](./image/exit-mysql-shell-status.PNG)

#Start the interactive script by running:

`sudo mysql_secure_installation`

![Validate Password Status](./image/validate-password-status.PNG)

![Mysql Secure Installation Status](./image/sql-secure-installation-status.PNG)

#When you’re finished, test if you’re able to log in to the MySQL console by typing:

`sudo mysql -p`

#Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.

![Login to Mysql Status](./image/login-to-mysql-console-status.PNG)

#To exit the MySQL console, type:

`exit`

![Exit Mysql Shell Status](./image/exit-mysql-shell-status.PNG)


3. **Installing PHP**

#You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

#To install these 3 packages at once, run:

`sudo apt install php libapache2-mod-php php-mysql`

![Apt Install PHP Status](./image/apt-install-php-status.PNG)

#Once the installation is finished, you can run the following command to confirm your PHP version:

`php -v`

![PHP Version Status](./image/php-version-status.PNG)


4. **Creating a Virtual Host for your Website using apache**

#Create the directory for projectlamp using ‘mkdir’ command as follows:

`sudo mkdir /var/www/projectlamp`

![Create Projectlamp Directory Status](./image/create-projectlamp-status.PNG)

#Next, assign ownership of the directory with your current system user:

`sudo chown -R $USER:$USER /var/www/projectlamp`

![Create Projectlamp Directory Status](./image/assign-directory-owner-status.PNG)

#Then, create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using vi or vim (They are the same by the way):

`sudo vi /etc/apache2/sites-available/projectlamp.conf`

#This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

![Vi Command Line Status Page](./image/vi-command-line-status.PNG)

![Virtual Host Paste](./image/virtual-host-paste-status.PNG)

#You can use the ls command to show the new file in the sites-available directory:

`sudo ls /etc/apache2/sites-available`

![Ls Command Status](./image/ls-command-status.PNG)

#You can now use a2ensite command to enable the new virtual host:
`sudo a2ensite projectlamp`

![Ls Command Status](./image/new-virtual-host-status.PNG)

#You might want to disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website use a2dissite command , type:

`sudo a2dissite 000-default`

![Apache Default Website Disable](./image/disable-default-apache-website-status.PNG)

#To make sure your configuration file doesn’t contain syntax errors, run:

`sudo apache2ctl configtest`

![Configuration Error Free](./image/configuration-error-free-status.PNG)

#Finally, reload Apache so these changes take effect:

`sudo systemctl reload apache2`

![Apache Changes Status](./image/apache-changes-status.PNG)

#Your new website is now active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:

`sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`

![Index Html Status](./image/index-html-status.PNG)

#Now go to your browser and try to open your website URL using IP address:

[Website using EC2 generated IP](http://3.143.208.18:80)

#In the output you will see your server’s public hostname (DNS name) and public IP address. You can also access your website in your browser by public DNS name, not only by IP – try it out, the result must be the same (port is optional):

[Website using EC2 generated DNS Name](http://ec2-3-143-208-18.us-east-2.compute.amazonaws.com:80)

![EC2 Public IP and DNS Name Status](./image/ec2-ip-url-generated-status.PNG)


5. **Enable PHP on the website**

#To enable PHP on the website, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:

`sudo vim /etc/apache2/mods-enabled/dir.conf`

![Apache Mods Enabled Status](./image/apache-mods-enabled-status.PNG)

#After saving and closing the file, you will need to reload Apache so the changes take effect:

`sudo systemctl reload apache2`

![Apache Reload Systemctl Status](./image/apache-reload-systemctl-status.PNG)

#Create a new file named index.php inside your custom web root folder:

`vim /var/www/projectlamp/index.php`

![Create Custom Webroot Folder](./image/create-webroot-folder.PNG)

#URL to launch PHP information page:

[Website using EC2 generated IP](http://3.137.194.42/index.php)

![PHP info Page](./image/phpinfo-page.PNG)

#After checking the relevant information about your PHP server through that page in line 152, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:

`sudo rm /var/www/projectlamp/index.php`

![PHP RM Status](./image/php-rm-status.PNG)