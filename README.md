# Django-based-database
- [1. Overview](#1-overview)
  - [1.1. Prerequisites](#11-prerequisites)
- [2. Install Django and Set up basic setting for Django](#2-Install-Django-and-Set-up-basic-setting-for-Django)
  - [2.1. Set up Django](#21-Set-up-Django)
  - [2.2. Connect MySQL to AWS and implement to Django](#22-Connect-MySQL-to-AWS-and-implement-to-Django)
- [3. Install AWS CLI and Set up EB Environment](#3-Install-AWS-CLI-and-Set-up-EB-Environment)
  - [3.1 Advanced usage](#31-advanced-usage)
  - [3.2 Clone this repository](#32-Clone-this-repository)
  - [3.3 Install or Upgrade the EB CLI](#33-Install-or-Upgrade-the-EB-CLI)
- [4. License](#4-license)

## 1. Overview 

This project is a direct tutorial for building a web application database using django rest framework and AWS from scratch and a simple handbook of how to maintain the usability on Windows. 

Duration: 1-2 weeks to build and setup

Motivation: Many companies stay with the tradtional data storage methods, such as manually saving as a single excel in HDD, or storing thousands of files in Cloud Drive like OneDrive. However, they are not safe and efficient for frequent use in a daily manner. Constantly opening and closing Excel that contains thousands of sheets will cause the program randering slow and easy to crash; and OneDrive has the consistent issue of losing T-1 backup files. Therefore, I wanted to find a way to store and protect data in a more organizable and easier way, while being less costly. This is why I found Django, which helps me build a SQL database in a considerably short time in Python. 

Reason: Reduce the risk of losing data, improve the efficiency to query and store data, shorten the latency of loading data, and use database wherever I go. 

Learn: How to build Django web application for database usage, how to build and test the pipeline for database, how to connect local Django to AWS and test the compatability. 

Current difficulty: Extra cost from AWS RDs, Insufficienty protection for database, Limited front-end related knowledge.  

Updated on 06/01/2023

### 1.1. Prerequisites

Applications and services: AWS EC2, AWS RDs, AWS S3, Django, Django Restframe work, VSC.


## 2. Install Django and Set up basic setting for Django. 

Ref: [Django Official Website](https://docs.djangoproject.com/en/4.2/intro/tutorial01/)

### 2.1. Set up Django

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

Note: if the server has been occupied, we can go to setting and change the following content: 

```Python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000", #example of new server
    "http://127.0.0.1:8000",
]
```

Then run the following: 

```Python
python manage.py runserver localhost:3000
```

5. Set up your database by running the following code: 


```Python
python manage.py migrate
```

Please following the [tutorial from Django](https://docs.djangoproject.com/en/4.2/intro/tutorial02/) to create your model and your superuser.

After creating your model, run the following code: 

```Python
python manage.py makemigration
```
```Python
python manage.py migrate
```

Now your project will be like: 

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
    db.sqlite3
    manage.py
```


### 2.2. Connect MySQL to AWS and implement to Django

1. Please follow the [tutorial](https://medium.com/@ryanzhou7/connecting-a-mysql-workbench-to-amazon-web-services-relational-database-service-36ae1f23d424) to make sure you have set up your AWS account with the AWS **Free Tier** as well as [AWS EC2](https://aws.amazon.com/ec2/). 

2. Connect the Django to AWS by following [AWS official toturial](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create-deploy-python-django.htmlc)

After configuring Django application for Electic Beanstalk, your project folder will be having the following structure: 

```Python
your_customize_name/
|-- .ebextensions
|   |-- django.config
|-- your_customize_name
|   |-- __init__.py
|   |-- settings.py
|   |-- urls.py
|   |-- wsgi.py
|-- db.sqlite3
|-- manage.py
|-- requirements.txt
```

3. Make sure the following packages have been added to requirements.txt and MySQL Workbench has been installed in your PC:

```Python
mysql-connector==2.2.9
mysql-connector-python==8.0.31
mysqlclient==2.1.1
```

4. Please chaneg the following line in setting.py to be: 

```Python
DATABASES = { 
    'default': {
        'ENGINE'  : 'django.db.backends.mysql', 
        'NAME'    : os.getenv('DB_NAME'     , '<database_name>'),
        'USER'    : os.getenv('DB_USERNAME' , '<database_user>'),
        'PASSWORD': os.getenv('DB_PASS'     , '<database_password>'),
        'HOST'    : os.getenv('DB_HOST'     , '<database_endpoint>'),
        'PORT'    : os.getenv('DB_PORT'     , 3306),
    }
 }
 ```

## 3. Install AWS CLI and Set up EB Environment

### 3.1. Prerequisites

You will need to have the following prerequisites installed before running the install script.

* **Git**
  * If not already installed you can download git from the [Git downloads page](https://git-scm.com/downloads).
* **Python**
  * We recommend that you install Python using the [pyenv](https://github.com/pyenv/pyenv) Python version manager. Alternately, you can download Python from the [Python downloads page](https://www.python.org/downloads/).
* **virtualenv**
  * Follow the [virtualenv documentation](https://virtualenv.pypa.io/en/latest/installation.html) to install virtualenv.

### 3.2. Clone this repository

Use the following:

```
git clone https://github.com/aws/aws-elastic-beanstalk-cli-setup.git
```

### 3.3. Install/Upgrade the EB CLI

#### Windows
In **PowerShell** or in a **Command Prompt** window:

```
python .\aws-elastic-beanstalk-cli-setup\scripts\ebcli_installer.py
```

## 4. Usage

The `ebcli_installer.py` Python script will install the [awsebcli](https://pypi.org/project/awsebcli/) package in a virtual environment to prevent potential conflicts with other Python packages.

For most use cases you can execute the `ebcli_installer.py` script with no arguments.

```
python ./aws-elastic-beanstalk-cli-setup/scripts/ebcli_installer.py
```
## 5. License 

