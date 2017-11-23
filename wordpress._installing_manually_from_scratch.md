
### ![](<https://i.imgur.com/RBqEkJw.jpg>)  ###


###  ###


###  ###


### Prerequisites  ###

 FTP and SSH access to the server with Ubuntu

User with root permissions.

The OS is up to date:

`
   sudo apt-get update && apt-get upgrade
  `


### Installing Apache2  ###

 `
   sudo apt-get install apache2
  `

you can check your Apache version by using `
   apache2ctl -V
  `


### Installing MySQL  ###

 `
   sudo apt-get install mysql-server mysql-client
  `

specify your password for the root user when prompted (save the password)

You can check your MySQL version by using `
   mysql -V
  `


### MySQL configuring  ###

 Run the mysql_secure_installation script to address several security concerns in a default MySQL installation:

`
   sudo mysql_secure_installation
  `

You will be given the choice to change the MySQL root password, remove anonymous user accounts, disable root logins outside of localhost, and remove test databases. It is recommended that you answer yes to these options.

So enter your root user passwd or just hit ENTER if you haven't set is yet.

Confirm that you don't want to change your root password by entering "n" and hitting ENTER.

Confirm removing an anonymous user (enter "y").

Disallow root login remotely (enter "y"). So no one from the outside can guess your password.

Remove test database and access to it (enter "y").

Reload privileges tables now (enter "y").

Now our MySQL installation is secure!

Lets proceed to creating a database for our wordpress site.

Login to MySQL:

mysql -u root -p Enter your root password for MySQL.


### Create a New MySQL User and Database  ###

 Database "testdb" creating:

`
  CREATE DATABASE testdb;
 ` You can see the whole list of databases:

`
   SHOW DATABASES;
  `

Creating a user:

CREATE USER testuser@localhost IDENTIFIED BY 'password'; You can see list of users with:

`
  SELECT  User  FROM  mysql  .  user  ;  ` `
   testdb
  ` is the name of the database, `
   testuser
  ` is the user, and `
   password
  ` is the user’s password.

Grant access to the DB.

GRANT ALL ON testdb.* TO 'testuser'; Refresh the database permissions table:

`
   FLUSH PRIVILEGES;
  `

Exit from the MySQL interface

`
   exit
  `


### Installing PHP  ###

 `
   sudo apt-get install php5 libapache2-mod-php5 php5-mysql php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-ps php5-pspell php5-recode php5-sqlite php5-tidy php5-xmlrpc php5-xsl
  ` `
  `

you can check your PHP version by using `
   php -v
  `


### Wordpress downloading and configuring  ###

 Run

`
   cd /tmp/ && wget http://wordpress.org/latest.tar.gz
  `

to download the latest version of wordpress.

Extract the downloaded package:

`
   tar -xvzf latest.tar.gz
  `

Delete the default "index.html" file in the `
   /var/www/html
  ` :

`
   sudo rm /var/www/html/index.html
  `

and copy there the content of the "wordpress" folder from the /tmp directory:

`
   sudo mv wordpress/* /var/www/html/
  `

Make a copy of the " **wp-config-sample.php" ** file in the `
   /var/www/html/
  ` and call it " **wp-config.php": **

`
   sudo cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
  `

Then edit the newly created file and enter your database information you created earlier.

`
   sudo vim /var/www/html/wp-config.php
  `

// ** MySQL settings – You can get this info from your web host ** // /** The name of the database for WordPress */ define(‘DB_NAME’, ‘ wpdb  ‘); /** MySQL database username */ define(‘DB_USER’, ‘ wpuser  ‘); /** MySQL database password */ define(‘DB_PASSWORD’, ‘ password  ‘); Save your changes and continue.

The last and final step is to configure the correct security permissions on WordPress files and folders. To do that, run the commands below.

`
   sudo chown -R www-data:www-data /var/www/html/
    sudo chmod -R 755 /var/www/html/ `

Restart Apache2:

`
   sudo service apache2 restart
  `

and browse to the server’s IP address or hostname and you should see WordPress default installation page.

Now if yuo open your site you should see initial wordpress page.

NOTE.

If you want to use fancy permalinks don't forget to enable rewrite module of the apache:

`
   sudo a2enmod rewrite
  `

To use mod_rewrite from within .htaccess files (which is a very common use case), edit the default VirtualHost with

`
   sudo vim /etc/apache2/sites-available/000-default.conf
  `

Search for “DocumentRoot /var/www/html” and add the following lines directly below:

`
   <Directory "/var/www/html">
  `  `
   AllowOverride All
  `  `
   </Directory>
  `

`
   sudo service apache2 restart
  `

Problem with updating:

Reset the permissions of all files to `
   664
  ` :

`
  > find /path/to/site/ -type f -exec chmod
  664  {} \; ` Reset permissions of directories to `
   775
  ` :

`
  > find /path/to/site/ -type d -exec chmod
