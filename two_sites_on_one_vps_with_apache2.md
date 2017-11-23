
## ![](<https://i.imgur.com/qg24LeT.jpg>)  ##


##  ##


## Creating VirtualHosts  ##

 Create folders for your domains on server. For example:

`
  /home/apache/testsite
/home/apache/stagesite
 ` Put index.html file in each folder with any text.

Go to /etc/apache2/sites-available folder.

Create file testsite.conf

`
  sudo nano testsite.conf

<VirtualHost *:80>
DocumentRoot /home/apache/testsite
ServerName test.example.com
ServerAlias www.test.example.com
</VirtualHost>
 ` Create file stagesite.conf

`
  sudo nano stagesite.conf

<VirtualHost *:80>
DocumentRoot "/home/apache/stagesite"
ServerName stage.example.com
ServerAlias www.stage.exmple.com
</VirtualHost>
 ` You can create completely different domains same way.

`
  sudo nano blog.conf

<VirtualHost *:80>
DocumentRoot /home/apache/blog
ServerName blog.com
ServerAlias www.blog.com
</VirtualHost>
 ` Enable created sites

`
  sudo a2ensite testsite
sudo a2ensite stagesite
 ` here you have to use actual folders names (e.g. "/home/apache/ **testsite **  ")

Open /etc/apache2/apache.conf file

Add there:

`
  <Directory /home/apache/testsite/>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

<Directory /home/ole/stagesite/>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
 ` Restart apache

`
  sudo service apache2 reload
 `
## Redirecting domain to server  ##

 Created Virtual Hosts will work only if you redirect your domain name to server IP. Domains are just names that can be translated to IP numbers.


# Local computer  #

 To test your configuration on local machine, you need to edit hosts file.

`
  sudo nano /etc/hosts
 ` It should look like this.

`
  127.0.0.1       localhost domain1.com domain2.com
 ` Hosts file tells your computer that domain needs to be redirected to local machine.

**IMPORTANT! ** If you create entry in hosts file for existing domain, for example

`
  127.0.0.1       stackoverflow.com
 ` you will lose access to this website.


# Server  #

 In order to redirect domain to you web server, you need to create or modify "A"-type DNS record for given domain to IP address of your server. You can do it by panel control provided by your domain registrar.

If you do not know IP address of your server, log in to that server and type in command line:

`
  ifconfig
