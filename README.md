# Django-based-database

This project is a direct tutorial for building a web application database using django rest framework and AWS from scratch and a simple handbook of how to maintain the usability on Windows. 

Duration: 1-2 weeks to build and setup

Motivation: Many companies stay with the tradtional data storage methods, such as manually saving as a single excel in HDD, or storing thousands of files in Cloud Drive like OneDrive. However, they are not safe and efficient for frequent use in a daily manner. Constantly opening and closing Excel that contains thousands of sheets will cause the program randering slow and easy to crash; and OneDrive has the consistent issue of losing T-1 backup files. Therefore, I wanted to find a way to store and protect data in a more organizable and easier way, while being less costly. This is why I found Django, which helps me build a SQL database in a considerably short time in Python. 

Reason: Reduce the risk of losing data, improve the efficiency to query and store data, shorten the latency of loading data, and use database wherever I go. 

Applications and services: AWS EC2, AWS RDs, AWS S3, Django, Django Restframe work, VSC.

Learn: How to build Django web application for database usage, how to build and test the pipeline for database, how to connect local Django to AWS and test the compatability. 

Current difficulty: Extra cost from AWS RDs, Insufficienty protection for database, Limited front-end related knowledge.  

Updated on 06/01/2023

## Install Django and Set up basic setting for Django. 

Ref: [Django Official Website](https://docs.djangoproject.com/en/4.2/intro/tutorial01/)

**To set up Django**

1. First, please use the following code to download **Django 2.2**, since [AWS official website](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-django.html) only documentated the deploying process with this version. 

```Python
pip install Django==2.2
```

2. Make sure your python version support Django 2.2 and verify the Django version: 

```Python
python -m django --version
```

3. Change the directory to the right location where you like to store your code and run:

```Python
django-admin startproject [your_customize_name]
```

Now in the directory, you should have the following structure: 

```Python
your_customize_name/
    manage.py
    your_customize_name/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

4. Verify the usability of the Django project using development server: 

```Python
python manage.py runserver
```

You’ll see the following output on the command line:

```Python
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

June 01, 2023 - 15:50:53
Django version 4.2, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```