
### ![](<https://i.imgur.com/aGZu7tZ.jpg>)  ###


###  ###


###  ###


### Prerequisites  ###

 Brand new server VPS with Ubuntu 14.04

SSH and FTP access to the server


### **Steps **  ###

 **1. Preparations **

First of all let's bring our server up to date:

`
   sudo apt
   -  get update `

then

`
   sudo apt
   -  get upgrade `


### **Python installing **  ###

 You can use `
   python -V
  ` to show you the version of Python that the `
   python
  ` command resolves to.

To see all Python versions that currently installed in your Ubuntu OS:

`
   find / -type f -executable -iname 'python*' -exec file -i '{}' ; | awk -F: '/x-executable; charset=binary/ {print $1}' | xargs readlink -f | sort -u | xargs -I % sh -c 'echo -n "%: "; % -V'
  ` `
  `

Python 3 and 2.7.6 installed in Ubuntu "out of the box" at least for 14.04, and starting from 16.04 Ubuntu has "as boss" - Python 3 configured.

Now lets install pip for Python3:  `
   sudo apt
   -  get install python3 -  pip `

(if you prefer Python 2.7 use `
   sudo apt
   -  get install python -  pip ` instead)

Installation of the Django on server:  `
   sudo pip3 install Django
  ` (use `
   sudo pip install Django
  ` for Python2)

for specific version of the Django you can explicitly enter  `
   sudo pip3 install Django
   ==  1.5  .  5  `

Instal Jinja2:

`
   sudo pip3 install Jinja2
  `

Use `
   pip3 freeze
  ` command to check all installed Python modules.


##### 2. Creating Django Project   #####

 Go to the directory where you want to store your site and create a new Django project

`
  django
  -  admin startproject helloworld ` Go to the new project folder:

`
  cd helloworld
 ` and start a new application:

`
  django
  -  admin startapp helloapp ` Go to the "helloworld" directory of a current project (e.g. "home/helloworld/helloworld"):

`
  cd helloworld
 ` Open " `
   settings
   .  py ` " file and "install" the new " *helloapp * " application.

You can do this by using embedded "vim" file editor:

`
   vim settings
   .  py `

Then press "INSERT" and add a new row "helloapp":

`
  INSTALLED_APPS
  =  (  'django.contrib.auth'  ,  'django.contrib.contenttypes'  ,  'django.contrib.sessions'  ,  'django.contrib.sites'  ,  'django.contrib.messages'  ,  'django.contrib.staticfiles'  ,  'helloapp'  )  ` then press "Esc" key and type `
   :wq
  ` and confirm by hitting "Enter".

Also you can access this file with your FTP client (e.g. WinSCP, Filezilla)

Add an URL pattern to the `
   urls
   .  py ` of the project:

`
  from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
# Examples:
# url(r'^$', 'helloworld.views.home', name='home'),
url(r'^admin/', admin.site.urls),
url
  (  r '^'  ,  include('helloapp.urls') )  ,  ` then, for more complicated sites, you have to add in your "urls.py" file of the "helloapp" appropriate instructions,

but, for now, you can use more simple method :

`
  url
  (  r '^'  ,  'helloapp.views.home_view'  )  ,  ` to instruct Django to look for the function `
   home_view
  ` within the `
   views
   .  py ` file of the app " helloapp" (for this simple example-site).

Edit the `
   views
   .  py ` file of the "helloapp":

`
  from  django .  http import  HttpResponse def  home_view  (  request )  :  return  HttpResponse (  'Hello World'  )  ` simple response for the testing purpose.


##### 3. Server installing and configuration   #####

 Install Apache2:

`
  sudo apt
  -  get install apache2 ` Now you can check if apache works by opening your site/ip address in a browser.

Install `
   mod_wsgi
  ` module:

`
  sudo apt
  -  get install libapache2 -  mod -  wsgi -  py3 ` for Python3

or use

`
   sudo apt
   -  get install libapache2 -  mod -  wsgi ` for Python2.

To serve the Django application through `
   mod_wsgi
  ` , we need to write a WSGI script that serves as a connection between Apache and Django. The Django file structure by default is something like this:

`
  helloworld
  /  manage .  py helloworld /  __init__ .  py settings .  py urls .  py helloapp /  models .  py views .  py ` We're going to change it a bit and add an `
   apache
  ` directory:

`
   mkdir apache
  `

within the `
   helloworld
  ` folder to contain three files:

`
  helloworld/  manage .  py helloworld /  __init__ .  py settings .  py urls .  py apache /  __init__ .  py override .  py wsgi .  py helloapp /  models .  py views .  py ` This helps separate the logic, and you could also ignore this directory as a whole in your version control system.

You can create this files by using:

`
   touch override
   .  py  `

Edit `
   override
   .  py file:  `

`
  # override.py  from  helloworld .  settings import  *  DEBUG =  True  ALLOWED_HOSTS =  [  'www.mydomain.com'  ,  'mydomain.com'  ]  ` Finally, the `
   wsgi
   .  py ` file contains the WSGI settings. We're assuming that the root directory shown above is contained in the home directory of the user ( `
   /  home /  ole /  ` ):

`
  #wsgi.py  import  os ,  sys # Calculate the path based on the location of the WSGI script.  apache_configuration =  os .  path .  dirname (  __file__ )  project =  os .  path .  dirname (  apache_configuration )  workspace =  os .  path .  dirname (  project )  sys .  path .  append (  workspace )  sys .  path .  append (  project )  # Add the path to 3rd party django application and to django itself.  sys .  path .  append (  '/home/ole'  )  os .  environ [  'DJANGO_SETTINGS_MODULE'  ]  =  'helloworld.apache.override'  from  django .  core .  wsgi import  get_wsgi_application application =  get_wsgi_application (  )  ` You also need to transfer the ownership of the `
   apache
  ` directory to Apache's default user `
   www
   -  data ` in order to allow it to access the directory:

`
  sudo chown www
  -  data :  www -  data /etc/apache2 /  ` To configure Apache to use your WSGI script, you need to edit the configuration file as shown below (using a text editor, in this case VIM):

`
  sudo vi
  /  etc /  apache2 /  sites -  enabled /  000  -  default .  conf ` Add the following lines to the file:

`
  <VirtualHost *:80> WSGIScriptAlias / /home/ole/helloworld/helloworld/apache/wsgi.py <Directory "/home/ole/helloworld/helloworld/apache/"> Require all granted </Directory>  ` The first line adds an alias of `
   /mypath  ` to the root of your web application. Your web application would now run on your domain— `
   http
   :  /  /  www .  mydomain .  com /  mypath /  ` . Replace `
   /  mypath /  ` above with `
   /  ` if you want your domain, `
   http
   :  /  /  www .  mydomain .  com /  ` , to directly point to the Django application.

The `
   <  Directory >  ` block is to allow requests to the directory that contains the WSGI script.

If you have a custom `
   robots
   .  txt ` and favicon, you may add an alias as follows:

`
  Alias
  /  robots .  txt /  home /  ole /  helloworld /  robots .  txt Alias /  favicon .  ico /  home /  ole /  helloworld /  favicon .  ico ` In each of the lines above, the first argument after the keyword `
   Alias
  ` signifies the URL pattern, and the second argument shows the path to the file to be served. This example assumes that your files `
   robots
   .  txt ` and `
   favicon
   .  ico ` are saved in the `
   helloworld
  ` directory.

To serve static and media files, you need to create their alias entries separately:

`
  Alias
  /  media /  /  home /  ole /  helloworld /  media /  Alias /  static /  /  home /  ole /  helloworld /  static /  <  Directory /  path /  to /  mysite .  com /  static >  Require all granted <  /  Directory >  <  Directory /  path /  to /  mysite .  com /  media >  Require all granted <  /  Directory >  ` Change configuration of the `
   /etc/apache2/apache2.conf
  ` file to

`
  <Directory /home/ole/helloworld>
    Options FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
 ` Finally, save and close the file and restart Apache to see the changes:

`
  sudo service apache2 restart
 `
### That's all!  ###


### Note for static files being served by Django packages  ###

 Some Django packages have their own static and media files. In the development version, it's taken care of by Django, but it doesn't work that way when serving through Apache (including the Django admin static files). The static files are usually located in the same place where packages are installed.

An easy way to override them is to copy their static files to your static directory (and commit them), which is a rather messy solution. A better way would be to create an alias for the particular set of static files, just like you created alias entries for static and media roots.

[A discussion on StackOverflow](<http://stackoverflow.com/a/9500866>) gives an example of how to take care of the Django admin static and media files. You need to follow the same pattern for every other package that uses static and media files.

