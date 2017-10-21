#### **![](https://i.imgur.com/ToTcQml.png)**

#### **Prerequisites**

Installed Python with pip  
Installed latest version of Django

#### **Steps**

Create folder for a new website/project

Press Shift , hover mouse pointer over the <project folder>, right-button mouse click on it and choose "Open command window here" item from the menu.

#### ![](https://i.imgur.com/DzVUYDk.png)

Write in the console

django-admin startproject <sitename>,  
where <sitename> is the name of your future site.

![](https://i.imgur.com/jD3GZxO.png)

Check that site is successfully created in a chosen folder

#### ![](https://i.imgur.com/xdwSGN5.png)

Open console again and change directory to the <sitename> :

cd <sitename>

#### ![](https://i.imgur.com/5mxCNlV.png)

Then if you have more than one version of Python installed on your device you might want to explicitly point what version of Python have to be used for youre site. To do so you have to type full path to the Python version you want.  
Example: C:/Python35/python manage.py runserver

End if you have just one version of Python installed on your machine you can just type:  
python manage.py runserver

![](https://i.imgur.com/AzY0uFD.png)

after that you'll see next message

#### ![](https://i.imgur.com/J3p97rB.png)

It's totally fine. We'll repair it soon.

For now just open your browser and type in the address row:  
127.0.0.1:8000

After that you have to see message like this:

#### ![](https://i.imgur.com/gC7WnGR.png)

#### ![](https://i.imgur.com/yz0Ii5n.png)

Also now you can see this db.sqlite file in your site directory

#### ![](https://i.imgur.com/71sH1Yk.png)

Now you can press Ctrl + C to stop your Django server.

So now we have an empty Django server!

In the next step we'll add some app.
