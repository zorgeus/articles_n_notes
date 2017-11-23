 ![](<https://i.imgur.com/GAxKMK9.jpg>)

[Original article](<http://www.liquidweb.com/kb/how-to-install-and-configure-phpmyadmin-on-ubuntu-14-04/>)

phpMyAdmin is an open source tool used for the administration of MySQL. In addition to offering the capability to perform administration tasks such as creating, editing, or deleting databases, and managing users and permissions, phpMyAdmin provides a graphical user interface to do all of these tasks and more.


## Premises  ##

 *   These instructions are intended specifically for installing phpMyAdmin on Ubuntu 14.04 LTS.
*   I’ll be working from a Liquid Web Core Managed Ubuntu 14.04 LTS server, and I’ll be logged in as root.
*   Apache, MySQL and PHP, must be installed on your server.

## Step 1: Install phpMyAdmin  ##

 First, you’ll follow a simple best practice: ensuring the list of available packages is up to date before installing anything new.

`
   apt-get -y update
  `

Then it’s a matter of just running one command for installation via apt-get:

`
   apt-get -y install phpmyadmin
  `


## Step 2: Basic Configuration  ##

 As the installation runs you’ll be asked a few simple questions regarding the basic configuration of phpMyAdmin.

At the first screen, select **apache2 ** by using the space bar, then hit **enter ** to continue.

At the second screen, which asks “configure the database for phpmyadmin with dbconfig-common?”, select **Yes ** , then hit **enter ** to continue. ![](<https://i.imgur.com/AGludbq.png>)

At the third screen enter your MySQL password, then hit **enter ** to continue. ![](<https://i.imgur.com/Tr19wIh.png>)

And finally at the fourth screen set the password you’ll use to log into phpmyadmin, hit **enter ** to continue,

![](<https://i.imgur.com/yWgC7vm.png>)

and confirm your password:

![](<https://i.imgur.com/W97XXlW.png>)


## Step 3: Finish the Configuration of Apache  ##

 `
   vim /etc/apache2/apache2.conf
  `

Add the following to the bottom of the file:

`
   # phpMyAdmin Configuration
    Include /etc/phpmyadmin/apache.conf `

Then exit and save the file with the command **:wq ** .

Restart Apache with the following command:

`
   service apache2 restart
  `

Restart mySQL with:

`
   service mysql restart
  `

Verify that phpMyAdmin is working by visiting 'the_IP_of_your_server/phpmyadmin'.

For example: http://127.0.0.1/phpmyadmin

Use your MySQL  "root" login and password to login to the phpmyadmin.

