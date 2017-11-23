
### **![](<https://i.imgur.com/ToTcQml.png>) **  ###


### ****  ###


### ****  ###


### **Steps **  ###

 To create new page in the application we have to add new function for this page in the "views.py" file for given application. For instance, this is new "contact" page of the "personal" app.

![](<https://i.imgur.com/mQ7PGzY.png>)

Here we referring to 'basic.html' page, should be created in the "personal" app.

Also we creating dictionary with 'content' of the "basic.html" page.

After that we also creating "basic.html" file in the "personal" application

![](<https://i.imgur.com/8yWzfxu.png>)

Adding content in the "basic.html" file. As you can see our file will be some kind of variation of "header.html" just with different content.

Also here we iterate through our "content" key of dictionary in "views.py" file to put it in the html page

and this is how you pass variables from Python to html

![](<https://i.imgur.com/oAFLu2a.png>)

notice that iterated or condition operators in Jinja/html should be closed with special "{% end... %}" operator.

After that we adding new url pattern to the "urls.py" file of the particular app.

![](<https://i.imgur.com/1MZpbX0.png>)

And making sure that we can get access from the Home page to our new Contact page

![](<https://i.imgur.com/egHkYlN.png>)

![](<https://i.imgur.com/P1jxmAD.png>)

Now we have to add new urlpattern to the main "urls.py" file of the whole site.

Actually in this case we have to just change it a little bit.

![](<https://i.imgur.com/wpsyUod.png>)

in this case we stating that we delegating handling to the "personal" app urls.

Notice that before we had "r'^$'"  string and it has meaning: "for any url that start and immediately ended after the sitename"

Now its "r'^'"  and it has meaning :"any given combination of symbols after the sitename"

Don't use commented urlpatterns here : """url(r'^content/', include("personal.urls"))"""

regex is not compatible with string in that case and it will cause error.

And now wen we handling all not specified links in the "personal" application

We can put strict rules of how to manage "sub/sub" pages by changing the "urls.py" file for particular application

![](<https://i.imgur.com/298rZpL.png>)

we basically saying that we expect only "/contact/" to show our Contact page and everything else will be redirected to 404 page.

![](<https://i.imgur.com/tnne7r4.png>)

Or be more soft and put it like

![](<https://i.imgur.com/Dqe5ma5.png>)

and get that it doesn't matter if there is something after "contact/" or not, we still get our Contact page

![](<https://i.imgur.com/KwIAGXf.png>)

