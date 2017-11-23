
### ![](<https://i.imgur.com/aGZu7tZ.jpg>)  ###


###  ###


###  ###


###  ###


### Premises  ###

 Start from a brand new VPS.  OS is up to date:  `
   sudo apt-get update
  `  `
   sudo apt-get upgrade
  `


### Installing of the Apache2  ###

 `
   sudo apt-get install apache2
  `

And if you open your site in a browser,

you'll see something like this

![](<https://i.imgur.com/AD989P6.png>)

Installing of the `
   mod_wsgi
  ` module:

[su_tabs][su_tab title="Python 3"] ***sudo apt-get install libapache2-mod-wsgi-py3 * ** [/su_tab]

[su_tab title="Python 2"] ***sudo apt-get install libapache2-mod-wsgi * ** [/su_tab][/su_tabs]


### Creation of sites  ###

 Check your available Python versions:

`
   find / -type f -executable -iname 'python*' -exec file -i '{}' ; | awk -F: '/x-executable; charset=binary/ {print $1}' | xargs readlink -f | sort -u | xargs -I % sh -c 'echo -n "%: "; % -V'
  ` `
  `

you'll see something like this:

![](<https://i.imgur.com/4EzMikA.png>)

As you can see there is at least two versions of Python available in Ubuntu "right away".

Next we have to install `
   pip
  ` module:

[su_tabs][su_tab title="Python 3"] ***sudo apt-get install python3-pip * ** [/su_tab]

[su_tab title="Python 2"] ***sudo apt-get install python-pip * ** [/su_tab][/su_tabs]

With `
   pip
  ` we can install `
   virtualenv
  ` :

[su_tabs]

[su_tab title="Python 3"] ***sudo pip3 install virtualenv * ** [/su_tab]

[su_tab title="Python 2"] ***sudo pip install virtualenv * ** [/su_tab]

[/su_tabs]

The Python `
   virtualenv
  ` package allows you to create self-contained environments for various projects. Using this technology, you can install Django in a project directory without affecting the greater system. This allows you to provide per-project customization and packages easily.

Now we can start our projects.

Go to the "/home" directory and create the first project:

`
   mkdir testsitefolder
  ` `
  `

move into it:

`
   cd testsitefolder
  `

Now, create a virtual environment within the project directory by typing:

`
   virtualenv testsiteenv
  `

This will install a standalone version of Python, as well as `
   pip
  ` , into an isolated directory structure within your project directory. A directory will be created with the name you select, which will hold the file hierarchy where your packages will be installed.

Like this:

![](<https://i.imgur.com/wln1S7C.png>)

To install packages into the isolated environment, you must activate it by typing:

`
   source
   testsiteenv  /bin/activate `

Your prompt should change to reflect that you are now in your virtual environment.

![](<https://i.imgur.com/2OQqzER.png>)

In your new environment, you can use `
   pip
  ` to install Django. Regardless of whether you are using version 2 or 3 of Python, it should be called just `
   pip
  ` when you are in your virtual environment. Also note that you *do not * need to use `
   sudo
  ` since you are installing locally:

[su_tabs]

[su_tab title=" **For the last available version ** "] ***pip install django * ** [/su_tab]

[su_tab title=" **For any particular version ** "] ***pip install Django==1.5.5 * ** [/su_tab]

[/su_tabs]

Instal Jinja2:

`
   pip install Jinja2
  `

Use `
   pip list
  ` command to check all installed Python modules.

Use `
   pip freeze > requirements.txt
  ` command to store a list of your currently used modules in the file.

To leave your virtual environment, you need to issue the `
   deactivate
  ` command from anywhere on the system.

The prompt should revert to the conventional display. When you wish to work on your project again, you should re-activate your virtual environment by moving back into your project directory and activating:

`
   cd ~/home/testsitefolder
  `

`
   source
   testsiteenv  /bin/activate `

Second site will be created in `
   /home/stagesitefolder
  ` :

`
   cd /home  `

`
   mkdir stagesitefolder  `

Here we'll create different Python virtual environment that will use the Python2.7 interpreter:

`
   cd stagesitefolder  `

`
   virtualenv -p /usr/bin/python2.7 stagesiteenv
  ` `
  `

This way "stagesiteenv" folder will be created.

All packages for this particular environment will be stored here

![](<https://i.imgur.com/22Ugqyv.png>)

Now we can activate this environment:

`
   source
   stagesiteenv  /bin/activate `

Prompt will look like this:

![](<https://i.imgur.com/4tKSIao.png>)

and for this site we are going to install an 1.8.12 Django version ( [because it's the latest LTS version](<https://www.djangoproject.com/download/>) ):

`
   pip install Django==1.8.12  `

We can use `
   pip list  ` command to see all installed in this environment packages.

And `
   pip freeze > requirements.txt  `

to store a list of our packages in the file for future use.

Now `
   deactivate  ` the virtual environment.


### Creating a project  ###

 Returning to the first site folder:

`
   cd /home/testsitefolder
  ` `
  `

Activate the virtual environment for this site

`
   source testsiteenv/bin/activate
  ` `
  `

Now you can use the `
   django-admin
  ` command to create a project:

`
   django
   -  admin startproject testsite `

Go into it:

`
   cd testsite
  `

Start a new application:

`
   django
   -  admin startapp home ` `
  `

Now the structure of your project looks something like this:

![](<https://i.imgur.com/BJeCRo9.png>)

To bootstrap the database (this uses SQLite by default) you can type:

`
   python manage.py migrate
  `

Migration procedure output will be:

![](<https://i.imgur.com/MRZiSaW.png>)

Use `
   deactivate
  ` command to return to the normal prompt.

Open `
   settings
   .  py ` file in the `
   ~/testsitefolder/testsite/testsite
  `

See here:

![](<https://i.imgur.com/zvhhoez.png>)

and “install” the new “ *home * ” application:

![](<https://i.imgur.com/KEuDZVF.png>)

Open `
   urls
   .  py ` file in the `
   ~/testsitefolder/testsite/testsite
  `

See here:

![](<https://i.imgur.com/ym6wT8F.png>)

And add the URL pattern for the "home" app:

![](<https://i.imgur.com/Ir2RS6Q.png>)

to instruct Django to look for the function `
   home_view
  ` within the `
   views
   .  py ` file of the "home"app.

Edit the `
   views
   .  py ` file of the “home” app:

`
  from  django .  http import  HttpResponse def  home_view  (  request )  :  return  HttpResponse (  'This is the TESTSITE home page'  )  ` For the second site we going to

`
   cd /home/stagesitefolder
  `

Activating our virtual environment `
   source stagesiteenv/bin/activate
  `

Creating a new project:

`
   django
   -  admin startproject stagesite `

and a new app:

`
   cd stagesite
  `

`
   django
   -  admin startapp mainapp `

Use `
   deactivate
  ` command to return to the normal prompt.

Open `
   settings
   .  py ` file in the `
   ~/stagesitefolder/stagesite/stagesite
  `

And add/install "mainapp" to the list of applications.

Open `
   urls
   .  py ` file in the `
   ~/stagesitefolder/stagesite/stagesite
  `

And add the row:

`
   url(r'^', 'mainapp.views.main_view'),
  `

to instruct Django to look for the function `
   main_view
  ` within the `
   views
   .  py ` file of the "mainapp"app.

Edit the `
   views
   .  py ` file of the “mainapp” app:

`
  from  django .  http import  HttpResponse def  main_view  (  request )  :  return  HttpResponse (  'This is the Stage SITE home page'  )  `
### Connection between Django and Apache  ###

 To serve the Django application through `
   mod_wsgi
  ` , we need to write a WSGI script that serves as a connection between Apache and Django.

[su_spoiler title="The Django file structure by default looks like this:" style="fancy" icon="caret"]

testsite/ manage.py testsite/ __init__.py settings.py urls.py wsgi.py home/ models.py views.py ... [/su_spoiler]

We're going to change it a bit and add an `
   apache
  ` directory:

`
   mkdir apache
  `

within the `
   /testsitefolder/testsite/testsite
  ` folder

[su_spoiler title="to contain three files:" style="fancy" icon="caret"]

testsite/ manage.py testsite/ __init__.py settings.py urls.py **apache/ __init__.py wsgi.py override.py ** home/ models.py views.py ... [/su_spoiler]

Existed "wsgi.py" file can be deleted.

This helps to separate the logic, and you could also ignore this directory as a whole in your version control system.

[su_spoiler title="Content of 'override.py' file:" style="fancy" icon="caret"]

# override.py  from testsite.settings import * DEBUG = True ALLOWED_HOSTS = ['www.mydomain.com', 'mydomain.com'] *insted of "mydomain.com" use your actual domain.

**don't forget to change "DEBUG" to "False" before deploying your site to the production.

[/su_spoiler]

Finally, the `
   wsgi
   .  py ` file contains the WSGI settings. We're assuming that the root directory shown above is contained in the home directory of the user ( `
   /  home /  testsitefolder /  ` ):

[su_spoiler title=" 'wsgi.py' file content:" style="fancy" icon="caret"]

#wsgi.py  import os, sys # Calculate the path based on the location of the WSGI script.  apache_configuration= os.path.dirname(__file__) project = os.path.dirname(apache_configuration) workspace = os.path.dirname(project) sys.path.append(workspace) sys.path.append(project) # Add the path to 3rd party django application and to django itself.  sys.path.append('/home/testsitefolder') os.environ['DJANGO_SETTINGS_MODULE'] = 'testsite.apache.override' from django.core.wsgi import get_wsgi_application application = get_wsgi_application() [/su_spoiler]

You also need to transfer the ownership of the `
   apache
  ` directory to Apache's default user `
   www
   -  data ` in order to allow it to access the directory:

`
   sudo chown www
   -  data :  www -  data /etc/apache2 /  `

To configure Apache to use your WSGI script in daemon mode, you need to create the "example1.com.conf" file in the

`
   /  etc /  apache2 /  sites-available ` directory.

[su_spoiler title="And add there next rows:" style="fancy" icon="caret"]

<VirtualHost *:80> ServerName www.example1.com ServerAlias example1.com ServerAdmin webmaster@example1.com WSGIScriptAlias / /home/testsitefolder/testsite/testsite/apache/wsgi.py process-group=example1.com WSGIDaemonProcess example1.com python-path=/home/testsitefolder/testsite:/home/testsitefolder/testsiteenv/lib/python3.4/site-packages WSGIProcessGroup example1.com <Directory /home/testsitefolder/testsite> <Files wsgi.py> Require all granted </Files> </Directory> </VirtualHost> [/su_spoiler]

After that initiate site with:

`
   a2ensite example1.com
  ` `
  `

and reload the apache server:

`
   service apache2 reload
  ` `
  `

Finally your first site should work!

![](<https://i.imgur.com/fPmlNut.png>)

Now all the same steps for the second site

[su_spoiler title="Changing the site structure:" style="fancy" icon="caret"]

stagesite/ manage.py stagesite/ __init__.py settings.py urls.py **apache/ __init__.py wsgi.py override.py ** mainapp/ models.py views.py ... [/su_spoiler]

[su_spoiler title="Content of 'override.py' file:" style="fancy" icon="caret"]

# override.py  from stagesite.settings import * DEBUG = True ALLOWED_HOSTS = ['www.mydomain.com', 'mydomain.com'] *insted of "mydomain.com" use your actual domain.

**don't forget to change "DEBUG" to "False" before deploying your site to the production.

[/su_spoiler]

[su_spoiler title=" 'wsgi.py' file content:" style="fancy" icon="caret"]

#wsgi.py  import os, sys # Calculate the path based on the location of the WSGI script.  apache_configuration= os.path.dirname(__file__) project = os.path.dirname(apache_configuration) workspace = os.path.dirname(project) sys.path.append(workspace) sys.path.append(project) # Add the path to 3rd party django application and to django itself.  sys.path.append('/home/stagesitefolder') os.environ['DJANGO_SETTINGS_MODULE'] = 'stagesite.apache.override' from django.core.wsgi import get_wsgi_application application = get_wsgi_application() [/su_spoiler]

To configure Apache to use your WSGI script in daemon mode, you need to create the "example2.com.conf" file in the

`
   /  etc /  apache2 /  sites-available ` directory.

[su_spoiler title="And add there next rows:" style="fancy" icon="caret"]

<VirtualHost *:80> ServerName www.example2.com ServerAlias example2.com ServerAdmin webmaster@example2.com WSGIScriptAlias / /home/stagesitefolder/stagesite/stagesite/apache/wsgi.py process-group=example2.com WSGIDaemonProcess example2.com python-path=/home/stagesitefolder/stagesite:/home/stagesitefolder/stagesiteenv/lib/python2.7/site-packages WSGIProcessGroup example2.com <Directory /home/stagesitefolder/stagesite> <Files wsgi.py> Require all granted </Files> </Directory> </VirtualHost> [/su_spoiler]

After that initiate site with:

`
   a2ensite example2.com
  ` `
  `

and reload the apache server:

`
   service apache2 reload
  ` `
  `

Second site should work now!

![](<https://i.imgur.com/96adAnQ.png>)


### That's all!  ###

