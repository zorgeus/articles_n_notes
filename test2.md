# Django site from scratch w/ NGINX and MySQL

([0 comments](http://blog.zorgeus.gq/django-site-from-scratch-w-nginx-and-
mysql/#comments))

#### ![](http://i.imgur.com/ToTcQml.png)

#### Login to the server via SSH with

login: root

pswd: ******* (scaleway will pic up RSA key from your credentials)

If needed beforehand configure an RSA key and add it to your account

#### Update and upgrade your OS:

`sudo apt-get update`

`sudo apt-get upgrade`

#### Add a new user to the server

`adduser user`

Switch to the new user:

`su user`

Go back to root:

`exit`

Adding the new user to the sudo group:

`usermod -aG sudo username`

#### Make your user a member of the adm group to access nginx logs

`sudo usermod -aG adm user`

#### Configuring access to the server for the new user via RSA key.

On your work computer open PuTTY Gen and generate a pair of keys.

Save your private key onto PC.

Got to your home directory on the server:

`cd`

create `~/.ssh/authorized_keys` file in your Home directory

`mkdir .ssh`

`cd .ssh`

`touch authorized_keys`

Open `authorized_keys `file and past your public key there (from the putty gen
window itself)

`vim authorized_keys`

INSERT

Right mouse button click

ESC

`:wq`

Key has to begin with "ssh-rsa " and ends with "== rsa-key-&lt;date&gt;".

Exit from the server completely. Relogin as a user with RSA key

#### Optional Step Four--Disable the Password for Root Login

Once you have copied your SSH keys unto your server and ** ensured that you
can log in with the SSH keys alone**, you can go ahead and restrict the root
login to only be permitted via SSH keys.

In order to do this, open up the SSH config file:

`sudo vim /etc/ssh/sshd_config`

Within that file, find the line that includes `PermitRootLogin` and modify it
to ensure that users can only connect with their SSH key:

`PermitRootLogin without-password`

To completely forbid root login

Edit to this:` `

`PermitRootLogin no `

and  
`PasswordAuthentication no`

Put the changes into effect:

`reload ssh`

or

`sudo service ssh restart`

or

`sudo /etc/init.d/ssh restart`

**Now you can login through Putty only after specifying login and rsa key beforehand in Connection/Data and SSH/Auth respectively.**

If you want to deny certain users from logging in, put this in the
configuration file:

`DenyUsers root`

This takes the blacklisting approach. Whitelisting is generally preferable. If
your company needs to allow the `rob` and `admin` users log in on the server,
use the following configuration directive:

`AllowUsers rob admin`

#### Install nginx

`sudo apt-get install nginx`

`sudo service nginx restart`

Open your server's ip address in a browser. You should see a message from
nginx

#### Configuring a folder for static and media files

Add the folder /var/www

`sudo mkdir -p /var/www/static`

`sudo mkdir -p /var/www/media`

change owner of this folder to www-data:www-data (33:33) so nginx would be
able to serve files from there and write there:

`sudo chown -R 33:33 www`

Change permissions for this folder so group members can write to it:

`sudo chmod -R 775 www/`

make your user a member of www-data group:

`sudo usermod -aG 33 username`

#### Install MySQL

`sudo apt-get install mysql-server mysql-client`

specify your password for the root user when prompted (save the password)

You can check your MySQL version by using `mysql -V`

For ubuntu:

    
    
    sudo apt-get install python3-dev libmysqlclient-dev

in order for mysql to work with python3

#### MySQL configuring

Run the mysql_secure_installation script to address several security concerns
in a default MySQL installation:

`sudo mysql_secure_installation`

You will be given the choice to change the MySQL root password, remove
anonymous user accounts, disable root logins outside of localhost, and remove
test databases. It is recommended that you answer yes to these options.

So enter your root user passwd or just hit ENTER if you haven't set it yet.

Confirm that you don't want to change your root password by entering "n" and
hitting ENTER.

Confirm removing an anonymous user (enter "y").

Disallow root login remotely (enter "y"). So no one from the outside can guess
your password.

Remove test database and access to it (enter "y").

Reload privileges tables now (enter "y").

Now our MySQL installation is secure!

Lets proceed to creating a database for our  site.

Login to MySQL:

    
    
    mysql -u root -p

Enter your root password for MySQL.

#### Create a New MySQL User and Database

Database "testdb" creating:

    
    
    CREATE DATABASE testdb;

You can see the whole list of  databases:

`SHOW DATABASES;`

Creating a user:

    
    
    CREATE USER testuser@localhost IDENTIFIED BY 'password';

You can see list of users with:

    
    
    SELECT User FROM mysql.user;

`testdb` is the name of the database, `testuser` is the user, and `password`
is the user's password.

Grant access to the DB.

    
    
    GRANT ALL ON testdb.* TO 'testuser';

Refresh the database permissions table:

`FLUSH PRIVILEGES;`

Exit from the MySQL interface

`exit`

#### Install pip

`sudo apt-get install python3-pip`

#### Install Virtualenv Wrapper

`sudo apt install virtualenv`

`sudo python3 -m pip install virtualenvwrapper`

Add four lines to your shell startup file (`.bashrc`) to set the location
where the virtual environments should live, the location of your development
project directories, VIRTUALENVWRAPPER_PYTHON variable (so virtualenv will
work with the right python version) and the location of the script installed
with this package:

    
    
    export WORKON_HOME=$HOME/.Envs
    export PROJECT_HOME=$HOME/Devel
    VIRTUALENVWRAPPER_PYTHON='/usr/bin/python3'
    source /usr/local/bin/virtualenvwrapper.sh

After editing it, reload the startup file (e.g., run ``source ~/.bashrc``).

Create virtualenv for your site:

`mkvirtualenv site`

Install all needed packages through pip.

    
    
    pip install mysqlclient

####

#### Install GIT

`sudo apt-get install git`

Go to your home folder and clone your files to the project directory

`git clone <project link> .`

Configure your production settings file to work with created database.

    
    
    # settings.py
    DATABASES = { 'default': { 'ENGINE': 'django.db.backends.mysql', 'OPTIONS': { 'read_default_file': '/path/to/my.cnf', }, }
    } # my.cnf
    [client]
    database = NAME
    user = USER
    password = PASSWORD
    default-character-set = utf8

Collect static:

`workon <siteenvname>`

`python manage.py collectstatic`

Migrate your project:

`python manage.py migrate`

### Setting up the uWSGI Application Server

Now that we have two Django projects set up and ready to go, we can configure
uWSGI. uWSGI is an application server that can communicate with applications
over a standard interface called WSGI. To learn more about this, read [this
section](https://www.digitalocean.com/community/tutorials/how-to-set-up-uwsgi-
and-nginx-to-serve-python-apps-on-ubuntu-14-04#definitions-and-concepts) of
our guide on setting up uWSGI and Nginx on Ubuntu 14.04.

### Installing uWSGI

Unlike the guide linked above, in this tutorial, we'll be installing uWSGI
globally. This will create less friction in handling multiple Django projects.
~~~~

we can install uWSGI globally through `pip` by typing:

    
    
    sudo pip3 install uwsgi
    

### Creating Configuration Files

we will run uWSGI in "Emperor mode", which allows a master process to manage
separate applications automatically given a set of configuration files.

Create a directory that will hold your configuration files. Since this is a
global process, we will create a directory called `/etc/uwsgi/sites` to store
our configuration files. Move into the directory after you create it:

    
    
    sudo mkdir -p /etc/uwsgi/sites
    cd /etc/uwsgi/sites
    

In this directory, we will place our configuration files. We need a
configuration file for each of the projects we are serving. The uWSGI process
can take configuration files in a variety of formats, but we will use `.ini`
files due to their simplicity.

Create a file for your first project and open it in your text editor:``

    
    
    sudo vim sitename.ini
    

Inside, we must begin with the `[uwsgi]` section header. All of our
information will go beneath this header. We are also going to use variables to
make our configuration file more reusable. After the header, set a variable
called `project` with the name of your first project. Add a variable called
`base` with the path to your user's home directory.

Next, we need to configure uWSGI so that it handles our project correctly. We
need to change into the root project directory by setting the `chdir` option.
We can combine the home directory and project name setting that we set earlier
by using the `%(variable_name)` syntax. This will be replaced by the value of
the variable when the config is read.

In a similar way, we will indicate the virtual environment for our project. By
setting the module, we can indicate exactly how to interface with our project
(by importing the "application" callable from the `wsgi.py` file within our
project directory).

We want to create a master process with 5 workers.

Next we need to specify how uWSGI should listen for connections.

Instead of using a network port, since all of the components are operating on
a single server, we can use a Unix socket. This is more secure and offers
better performance. This socket will not use HTTP, but instead will implement
uWSGI's `uwsgi` protocol, which is a fast binary protocol for designed for
communicating with other servers. Nginx can natively proxy using the `uwsgi`
protocol, so this is our best choice.

We will also modify the permissions of the socket because we will be giving
the web server write access. We'll set the `vacuum` option so that the socket
file will be automatically cleaned up when the service is stopped:

    
    
    [uwsgi]
    project = firstsite
    base = /home/user chdir = %(base)/%(project)
    home = %(base)/Env/%(project)
    module = %(project).wsgi master = true
    processes = 5 socket = /tmp/%(project).sock
    chmod-socket = 664
    vacuum = true logto = /var/log/uwsgi/%n.log
    

With this, our first project's uWSGI configuration is complete. Save and close
the file.

### Create an Upstart Script for uWSGI

We now have the configuration files we need to serve our Django projects, but
we still haven't automated the process. Next, we'll create an Upstart script
to automatically start uWSGI at boot.

We will create an Upstart script in the `/etc/init` directory, where these
files are checked:

    
    
    sudo vim /etc/init/uwsgi.conf
    

Start by setting a description for your uWSGI service and indicating the
runlevels where it should automatically run. We will set ours to run on
runlevels 2, 3, 4, and 5, which are the conventional multi-user runlevels.

Next, we need to set the username and group that the process will be run as.
We will run the process under our own username since we own all of the files.
For the group, we need to set it to the `www-data` group that Nginx will run
under. Our socket settings from the uWSGI configuration file should then allow
the web server to write to the socket. Change the username below to match your
username on the server.

Finally, we need to specify the actual command to execute. We need to start
uWSGI in Emperor mode and pass in the directory where we stored our
configuration files. uWSGI will read the files and serve each of our projects:

    
    
    description "uWSGI application server in Emperor mode" start on runlevel [2345]
    stop on runlevel [!2345] setuid user
    setgid www-data exec /usr/local/bin/uwsgi --emperor /etc/uwsgi/sites
    

When you are finished, save and close the file.

## Configure Nginx as a Reverse Proxy

Once Nginx is installed, we can go ahead and create a server block
configuration file for each of our projects. Start with the first project by
creating a server block configuration file:

    
    
    sudo vim /etc/nginx/sites-available/firstsite
    

Inside, we can start our server block by indicating the port number and domain
name where our first project should be accessible. We'll assume that you have
a domain name for each.

Next, we can tell Nginx not to worry if it can't find a favicon. We will also
point it to the location of our static files directory where we collected our
site's static files.

After that, we can use the `uwsgi_pass` directive to pass the traffic to our
socket file. The socket file that we configured was called `firstproject.sock`
and it was located in our project directory. We will use the `include`
directive to include the necessary `uwsgi` parameters to handle the
connection:

    
    
    server { listen 80; server_name firstsite.com www.firstsite.com; location = /favicon.ico { access_log off; log_not_found off; } location /static/ { root /home/user/firstsite; } location / { include uwsgi_params; uwsgi_pass unix:/tmp/firstsite.sock; }
    }
    

That is actually all the configuration we need. Save and close the file when
you are finished.

Next, link your new configuration file to Nginx's `sites-enabled` directory to
enable them:

    
    
    sudo ln -s /etc/nginx/sites-available/firstsite /etc/nginx/sites-enabled

Check the configuration syntax by typing:

    
    
    sudo service nginx configtest
    

If no syntax errors are detected, you can restart your Nginx service to load
the new configuration:

    
    
    sudo service nginx restart
    

If you remember from earlier, we never actually started the uWSGI server. Do
that now by typing:

    
    
    sudo service uwsgi start
    

You should now be able to reach your project by going to their respective
domain names. Both the public and administrative interfaces should work as
expected.

[Share on
Twitter](https://twitter.com/intent/tweet?url=http%3A//blog.zorgeus.gq/django-
site-from-scratch-w-nginx-and-
mysql/&text=Django%20site%20from%20scratch%20w/%20NGINX%20and%20MySQL) [Share
on
Facebook](https://www.facebook.com/sharer/sharer.php?u=http://blog.zorgeus.gq
/django-site-from-scratch-w-nginx-and-mysql/)

### Related posts

  * [Multiple Django sites with Apache2 in daemon mode](http://blog.zorgeus.gq/multiple-django-sites-with-apache2-in-daemon-mode/)
