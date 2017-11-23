
### **![](<https://i.imgur.com/ToTcQml.png>) **  ###


### ****  ###


### ****  ###


### ****  ###


### ****  ###


### ****  ###


### **Prerequisites **  ###

 Django server is successfully started and closed

Console is opened from the project folder

![](<https://i.imgur.com/G5F8yWM.png>)


### **Steps **  ###

 1. If you have only one Python version installed on you machine, you can just write into the console:

pytnon manage.py startapp <appname>

where <appname> is your application name.

And if you want to use some specific version of Python, you have to write it explicitly.

Example :

C:/Python34/python manage.py startapp <appname>

![](<https://i.imgur.com/l4JbpUH.png>)

And now if you open your project folder you can see a new folder in it, with the name of your app

![](<https://i.imgur.com/H6GZOhC.png>)

And bunch of files in it

![](<https://i.imgur.com/1kYkv5e.png>)

2. In Windows Explorer open your project folder and go to the next project folder and open settings.py file of your project for editing (how to set up Python IDLE for opening by default)

![](<https://i.imgur.com/QEjT2gi.png>)

Add your new app name in the INSTALLED_APPS section

![](<https://i.imgur.com/54ykdBD.png>)

and save settings.py file. Close it.  3. Then in the same directory open urls.py file for editing and add next string in the urlpatterns section :  url(r'^<appname>/', include('<appname>.urls')),

where <appname> is the name of your new app

![](<https://i.imgur.com/f9YGGiX.png>)

By this we actually adding a pattern and saying:  "for urls that has <appname> in it (read more about regular expressions), look at the file <appname>.urls

And to make this include work you have to import it to the urls.py file.  To do so just add , includ to the end of  from django.conf.urls import url string

![](<https://i.imgur.com/N4TYrsr.png>)

Save file and close it.  4. Go to your <appname> folder and open views.py file. Type there your views. For instance :  from django.http import HttpResponse

def index (request):  return HttpResponse ("<h2> HEY!</h2>")

![](<https://i.imgur.com/3ft9OQa.png>)

Save file.  5. Go to your current app folder, create there urls.py file, open it and add your code.  For example:

from django.conf.urls import url  from . import viewsurlpatterns = [  url(r'^$', views.index, name = 'index')]

![](<https://i.imgur.com/Aq1dXH8.png>)

. (period) - is a relative import, we just importing from current package

Save file and exit.  6. Open your site folder and start console. Run server:

python manage.py. runserver

![](<https://i.imgur.com/fNOCtW3.png>)

server still has errors. But if we open browser end go to 127.0.0.1:8000 we can see what actually our server is looking for

![](<https://i.imgur.com/ODVe5m5.png>)

it's looking for ^admin/ or ^webapp/ urls.  So if you type in the address field

127.0.0.0:8000/webapp

you'll see our response

![](<https://i.imgur.com/BJmlx4C.png>)

Our first very simple application is working now!  Now it's time to look at templates

