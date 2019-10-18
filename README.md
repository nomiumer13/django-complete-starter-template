# django-complete-starter-template
Bsic Structure of Django with a working foot-print.


Here Some Quick Guides and useful commands.


+++++++++++++++++++++++++++++++++++++ Django Level 1 (Basics) ++++++++++++++++++++++++++++++

Steps of creating Virtual Env. & Django Project:

1: Create Virtual Env for python
    To Create :
                          conda create --name ENV_NAME python=VERSION
    To Delete :
                          conda env remove --name ENV_NAME  (If You want to Delete Environment )

2: Check List of Env.s
                          conda info --envs

3: Activate Env
                          conda activate ENV_NAME
                          conda deactivate (To Deactivate Currently Active Env)

4: Install Django in Env
                          conda install django



Steps to Create and Run Project:

1: Move to Directory Where to Create New Project

2: Make sure to Activate Env First

3: Create Project
                          django-admin startproject PROJECT_NAME

4: Move to project directory
                          cd TAB

5: Run the Server
                          python manage.py runserver
                          Ctrl-C To Stop Server (When Required)

6: Copy & Paste URL generated
    Ignore Migration Warnings If you found any there.

Django Project may have many Django Application...
Django Applications are just plugins like Apps (Reuseable in different Project also knows as Plugable Django Apps)




Steps to create Django App in Django Project

1: Move to Directory of Project

2: Create App
                          python manage.py startapp APP_NAME

To Register Newly Created App in Project We need to Register it in 
settings.py file of project.
Find 
INSTALLED_APPS and make entry of this App there.
And Run the Server
                          python manage.py runserver

To ensure NoErrors Out There.


(Optional for Testing)
Create View (Simple HelloWorld) in views.py in App Folder
and Link it to URL in urls.py file in Project Folder


******************************************************  URL MAPING AND MODULARITY ****************************************************

To make Apps more Moduler in Project.
We have to make a ReDirection of URL from 
urls.py file of Project  ----- TO ----> urls.py file of Specific Application 
This will now work as a gateway to Applications of Project.


                                    ( urlspatterns consulted )                ||--> App1_URL File  ----> Views of the App1
UserRequest -------> ProjectURL File -------------------->||--> App2_URL File  ----> Views of the App2
(From Browser)                                                                   ||--> App3_URL File  ----> Views of the App3




To Perform this ReDirection make urls.py file in Specific Application and then make it included in urls.py file of Project using urlpatterns list.
urls.py (Project) -------> MAP TO -------> urls.py (Application) -------> MAP TO ------->Views.py (Which View to be rendered)


******************************************************  TEMPLATES **************************************************************************
Templates are the Static HTML pages with specific Template Tags (a.k.a Django Template Variables) to populate the HTML page with dynamic content produced by python.

To let django know where Templates are we have to 
Register this directory in TEMPLATES Dictionary in setting.py file of Project

We need to use render() in views to get populated HTML Pages like
                          DATA_DICTIONARY = {}
                          render(request,'APP_NAME/FILE_NAME.html',DATA_DICTIONARY)

Note: Don't hard-code the Template Directory Path there in Templates Dictionary , use os module to get path instead. so it will be easily shareable project with other users/systems/OS.

                          TEMPLATES_DIR = os.path.join(BASE_DIR,'templates')

Templates are stored in directory with sub directory for each individual App.

1: Make Templates Directory in Project Root Level

2: Make it registered in settings.py file  in Project; by creating a variable containing, path to this directory in TEMPLATES (Dictionary)

3: Make HTML file/s in templates in subdirectories for each App

4: Add Template Tags as needed in HTML files

5: Make these Template Tags attached to Views in App to pass data to Template Tags of Step 4.

6: Render the result against request, using template (HTML file), you want to; along with content to pass in.



******************************************************  STATIC FILES **************************************************************************
Static File are to serve Static Contents to WebPages like css , images and stuff like that
Very similar process as of TEMPATES

Static Image Use Case:

1: Make static Directory with images Directory in it , in Project Root Level

2: Make it registered in settings.py file in Project by creating a variable, containing path to this directory e.g
                          STATIC_DIR = os.path.join(BASE_DIR,"static")
    and then add up a List to manage All Static File Paths of different application e.g.
                          STATICFILES_DIRS = [
                                   STATIC_DIR,
                          ]

3: Load static files in HTML pages using 
                          {% load staticfiles%} below <!DOCTYPE html>

4: Add Template Tags as needed in HTML files and set source of img tag like 
                          <img src="{% static "images/IMAGE_NAME" %}">

5: Do run and test it out.


CSS File Use Case:

1: Make a directory under static named css for example.

2: Make a CSS file under css folder e.g FILE_NAME.css

3: Load static files in HTML pages using (if you not already done)
                          {% load staticfiles%} below <!DOCTYPE html>

4: Do linking of file using template tags like
                          <link rel="stylesheet" href="{% static "css/style.css" %}">

************************************************************************************************************************************************








++++++++++++++++++++++++++++ Django Level 2 (Models & MVT Pattern) ++++++++++++++++++++++++++++

1:  Create Models in models.py in App. Make all the Models inherited from django.db.models.Model class
     Django will generate SQL Database for this Models :) Django loves developers  :-P

     USE CASE :

                          from django.db import models

                          # Create your models here.
                          class Topic(models.Model):
                              top_name = models.CharField(max_length=264,unique=True)
                          
                              def __str__(self):
                                  return self.top_name
                          
                          class Webpage(models.Model):
                              topic = models.ForeignKey(Topic,on_delete=models.CASCADE)
                              name = models.CharField(max_length=264,unique=True)
                              url = models.URLField(unique=True)
                          
                              def __str__(self):
                                  return self.name
                          
                          
2:  Apply migration to Project
                          python manage.py migrate

3:  Apply migrations to Apps
                          python manage.py makemigrations APP_NAME

4:  Apply migration to Project Again
                          python manage.py migrate

5:  To ensure Django has created SQL Databases for our Models Use some Dummy data using Shell
                          python manage.py shell
     this will show up a shell command where we can run python to test.

Use Case : (Optional only for Testing and Learning)

IMPORTANT NOTE:
    Suppose We have a Topic Model in SQL Database in first_app. Assuming that we have it, run following test. To ensure Database is
    created against the Model.

                          from first_app.models import Topic
                          print(Topic.objects.all())    ----------------------------> Will Return Empty QuerySet[] as we don't have any data yet.
                                                                                                                    Now Create new Instance of Topic and save it.

                          t = Topic(top_name="Social Media")
                          t.save()

                          print(Topic.objects.all())    ----------------------------> Will Return   QuerySet [<Topic: Social Media>] List of Topics
                                                                                                              Objects.
                          quit()

(All Above stuff is optional for testing using Shell Command) Also can be done using Django Admin.




ACCESSING MODEL FROM DJANGO-ADMIN :
TO Get access to SQL Databases from Django Admin Panel we need to register all Models of Applications in admin.py (App) and have to create a super user..

1:   Follwing is Syntax code to Register Models for Django Admin.

                          from APP_NAME.models import MODEL_NAME _1 ,MODEL_NAME_2

                          admin.site.register(MODEL_NAME_1)
                          admin.site.register(MODEL_NAME_2)      ---------> Register All in same Way.

2:   To get access to Admin Interface we need to Create Super User
                          python manage.py createsuperuser
      Choose Username & Password (master_user & master_pass)

3:   Run the Server
                          python manage.py runserver

4:   Hit the URL
                          COPIED_URL/admin

5:   Do enter the Credential and Login as SuperUser. Hit the Chill Button of your Mind :-P




*************************************************** Populating With Dummy Data using Faker Lirary ****************************************

1:   Activate Env and move to Project Folder and do install Faker Library
                          pip install Faker

2:   After installing we need to create a script to populate Database with Dummy Data.
                    Checkout the Script Code in  populate_first_app.py (Project Django-Level2) 

3:   Execute the Script and Run the Server to look into database.



**************************************************************** MTV Model Template View**************************************************

1:   Import the Model/s in views.py  (App) (Model which you want to render in html)

2:   Get the data using that model and pass it to render() 

3:   Handle the data in HTML file using template tags , coming from views.py's render() 
                          Visit 7. Django Level Two Project Solutions for review everything done till now.




+++++++++++++++++++++++++++++++++ Django Level 3 (Forms)++++++++++++++++++++++++++++++++++++

1:   Create forms.py file in App, create Classes for Froms and make it inherited from django.forms [As same we did in models.py ] like :
                          from django import forms

                          class FormName(forms.Form):
                              name = forms.CharField()
                              email = forms.EmailField()


2:   Define function n views.py you want to get called against URL request and render the Form Template in it.

                          def form_name_view(request):
                              form = forms.FormName()  -----------> Pass this form object as dictionary to render() to put in form template.
                              return render(request,"basicapp/form_page.html",{'form':form})



3:   Mention the URL to ping in urls.py (App) to get this Form called against request.

4:   Specify the Django Template Tags in Forms HTML file like

                          {{ form }}    OR    {{ form.as_p }} --------> to enforce the html be rendered in top to bottom rendering of Form

      in form html tag with method GET/POST
      Also make sure to use csrf_toke in form like

                          {% csrf_token %}

5:   Now to accept data from user, in function ( defined at step 2 ) do check request method and check forms validity and access data
      values from POST request form coming ,  Like

                              if request.method ==  'POST':
                                  form = forms.FormName(request.POST)
                                  if form.is_valid():
                                      print('Validation Successful..')
                                      print("Name = {}".format(form.cleaned_data['name']))






********************************************************** FORMS CUSTOM VALIDATION *********************************************
To learn form validation to protect from fake users and bots we will use hidden fields and Built-in Django Validators everything we need to do is concerened with forms.py file (App).
Will  learn :
1)   Check for empty Filed
2)   Check for Bot
3)   Adding clean method to entire form.

1):  CUSTOM VALIDATION USING HIDDEN FIELDS (For Bots) / ANY FIELD

      Note : In this method we need to follow the convension while defining function, to check any specific filed. Function name should be
                  like.
                           clean_FIELDNAME(self)
                 USE CASE :

                           botcatcher = forms.CharField(required=False,widget=forms.HiddenInput)
                           def clean_botcatcher(self):
                               botcatcher = self.cleaned_data['botcatcher']
                               if len(botcatcher)>0:
                                   raise forms.ValidationError("GOTCHA BOT..!")
                               return botcatcher
      

2):   BUILT-in VALIDATORS
                           from django.core import validators

                           botcatcher = forms.CharField(required=False,widget=forms.HiddenInput,
                                                                              validators=[validators.MaxLengthValidator(0)])


3):  CUSTOM VALIDATION USING FUNCTION DDFINING OUTSIDE THE CLASS
      To check the name should start with z , for example

      Define a function outside the class, with value names paramenter i.e

                           def check_for_z(value):
                               if value[0].lower() != 'z':
                                   raise forms.ValidationError('Name should start with z.')

       Pass this into validators list of name field

                           name = forms.CharField( validators=[check_for_z])


4):   CUSTOM CLEAN ALL FORM FIELDS
       We need to define a function named clean(self) and call super.clean() to get data. Then implement the logic. Like to check email
       confirmation logic will be :

                               email = forms.EmailField()
                               verify_email = forms.EmailField(label='Enter Email again.')
                     
                               def clean(self):
                                   all_cleaned_data = super().clean()
                                   email = all_cleaned_data['email']
                                   verify_email = all_cleaned_data['verify_email']
                                   if email != verify_email:
                                       raise forms.ValidationError('Email should match')





***************************************************************** MODEL FORMS *************************************************************
Model Forms are used to map out the fields defined in Model with the Fields of Forms, to make it easy to get user input and save it into Model.
Model Forms are extended version of forms, Look into django_level3_complete





+++++++++++++++++++++++++++++++++ Django Level 4 (URL ROUTING) +++++++++++++++++++++++++++++

*************************************************************** TEMPLATE TAGGING **********************************************************

Template Tagging is used to generate the Relative URLs not hard-corded URLs.

1):   Make a app_name variable in urls.py (App) with value of name of application. Like

                               #TEMPLATES TAGGING
                               app_name = 'APP_NAME'

2):   Use Template Tagging to generate URLs in href like.
       Syntax:
                               <a href="{% url 'APP_NAME:VIEW_FUNCTION_NAME' %}">URL_TEXT_TO_SHOW</a>
       Use Case:
                               <a href="{% url 'test_app:relative' %}">Refresh</a>
                               <a href="{% url 'test_app:other' %}" target="_blank">Other</a>
                               <a href="{% url 'admin:index' %}" target="_blank">admin</a>
                               <a href="{% url 'index' %}" target="_blank">Index</a>




********************************************************** URL TEMPLATE INHERITANCE *****************************************************
In this way we define a base.html (Template) with some Template Tag to add blocks in this template. Like

1):   Add template tag in base.html file.

                               <div class="container">
                                   {% block USER_DEFINED_BLOCK_NAME %}
                                       <!-- Everything outside of this block will be inherited if you extend -->
                                   {% endblock %}
                               </div>

2):   Then associate / extend other (Child) templates with this base template.

                               {% extends 'APP_NAME/BASE_FILE_NAME.html' %}
                                  {% block USER_DEFINED_BLOCK_NAME %}
                                     <h1>Welcome to Other (Example of Template Inheritance)</h1>
                                  {% endblock %}




********************************************************** TEMPLATE FILTERS & CUSTOM FILTERS *******************************************
Filters are used to transform the values of variables and tag arguments from HTML side with manipulating at Python Side.

1):   Built-in Filter can be used with pipe sign | along with variable name Like.

                               {{  VARIABLE_NAME | FILTER_NAME  }}

2):   To create custom filters create a templatetags folder in App with __init__.py (Empty File) in it. Then make FILE_NAME.py file to
       define custom filters in it, Make sure to register file as template filter. Like

                               from django import template
                               register = template.Library()

3):   Define Logic of your template filter in this file.

                               def cut(value,arg):
                                     """
                                     This will remove all args from String :) Custom Defined Filter.
                                     """
                                     return value.replace(arg,'')

4):   Register it.
                               register.filter('cut',cut)


5):   Use in Templates (HTML Files) as you want to.









++++++++++++++++++++++++++++ Django Level 5 (Passwords , Logins , Securities) +++++++++++++++++++++++++
Visit Lecture for detailed info.
To make Web Apps more secure we never save passwords as plain texts. We encrypt using SHA-256 , SHA-1 etc Encrypting Algorithms.
Create Project , App , Do migrations and try run project, then...

1):   Install Hashing/Encryption Libraries Like

                               pip install bcrypt
                               pip install django[argon2]


2):   Mention Your Password Hasher algorithms in settings.py (Project), High Prioritized Hasher should be on top of the list then move to
       low Prioritized.

                               PASSWORD_HASHERS = [
                                   'django.contrib.auth.hashers.Argon2PasswordHasher',
                                   'django.contrib.auth.hashers.BCryptSHA256PasswordHasher',
                                   'django.contrib.auth.hashers.BCryptPasswordHasher',
                                   'django.contrib.auth.hashers.PBKDF2PasswordHasher',
                                   'django.contrib.auth.hashers.PBKDF2SHA1PasswordHasher',
                               ]


3):   Create templates , static , media folders. templates , static we alread learned about those, media is concerened to save the contents
       uploaded by users like profile pic etc Like:

                               TEMPLATES_DIR = os.path.join(BASE_DIR,"templates")
                               STATIC_DIR = os.path.join(BASE_DIR,'static')
                               MEDIA_DIR = os.path.join(BASE_DIR,'media')

       Go all the way down in settings.py and make both Directory Paths get registered :) As follow...

                               STATIC_URL = '/static/'
                               STATICFILES_DIRS = [STATIC_DIR,]

                               # MEDIA
                               MEDIA_ROOT = MEDIA_DIR
                               MEDIA_URL = '/media/'


4):   Make a Custom User Model in models.py (App) and extend it from User (A built-in Model from Django) , by OneToOne Relationship.
       USE CASE :
                         
                               from django.db import models
                               from django.contrib.auth.models import User
                               # Create your models here.
                         
                               class UserProfileInfo(models.Model):
                                   user = models.OneToOneField(User,on_delete=models.CASCADE)
                                   # Additional Attributes
                                   portfolio_site = models.URLField(blank=True)
                                   profile_pic = models.ImageField(upload_to='test_app/profile_pics',blank=True)
                         
                                   def __str__(self):
                                       return self.user.username
                         
                         
5):   Make SubDirectory in media Folder. To save Profile Pics uploaded.

6):   Do install Imaging Library for Python by
                               pip install pillow
                               # Optional: pip install pillow --global-option="build_ext" --global-option="--disable-jpeg"


7):   Create a Model Form and define its attributes.
       USE CASE :

                               from django import forms
                               from django.contrib.auth.models import User
                               from test_app.models import UserProfileInfo
                               
                               class UserForm(forms.ModelForm):
                                   password = forms.CharFields(widget=forms.PasswordInput())
                               
                                   class Meta():
                                       model = User
                                       fields = ('username', 'email', 'password')
                         
                               class UserProfileInfoForm(forms.Model):
                                   class Meta():
                                       model = UserProfileInfo
                                       fields = ('portfolio_site', 'profile_pic')
                         
8):   Register your Model in admin.py (App)

                               from django.contrib import admin
                               from test_app.models import UserProfileInfo
                               admin.site.register(UserProfileInfo)

9):   Do set up Views and Logic to register, setup templates as well.



************************************************************** LOGIN TO APP *****************************************************************

1):   Create User Login URL in settings.py :

                               LOGIN_URL = '/test_app/user_login'

2):   Create Login Template and set up login form there

3):   Do imports necessary for authentication Like

                               # Extra Imports for the Login and Logout Capabilities
                               from django.contrib.auth import authenticate, login, logout
                               from django.http import HttpResponseRedirect, HttpResponse
                               from django.urls import reverse
                               from django.contrib.auth.decorators import login_required

