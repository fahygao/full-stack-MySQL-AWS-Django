# Django project with AWS Deployment
- [1. Overview](#1-overview)
  - [1.1. Prerequisites](#11-prerequisites)
- [2. Install Django and Set up basic setting for Django](#2-Install-Django-and-Set-up-basic-setting-for-Django)
  - [2.1. Set up Django](#21-Set-up-Django)
  - [2.2. Connect MySQL to AWS and implement to Django](#22-Connect-MySQL-to-AWS-and-implement-to-Django)
- [3. Install AWS CLI and Set up EB Environment](#3-Install-AWS-CLI-and-Set-up-EB-Environment)
  - [3.1 Advanced usage](#31-advanced-usage)
  - [3.2 Clone this repository](#32-Clone-this-repository)
  - [3.3 Install or Upgrade the EB CLI](#33-Install-or-Upgrade-the-EB-CLI)
- [4. Alternative method to download EB](#4-Alternative-method-to-download-EB)
- [5. License](#5-License)

## 1. Overview 

I have created a full stack web application that combines database with front-end UI pages. This is a tutorial I wrote for anyone that wants to create a similar web application using django rest framework and AWS from scratch.

Duration: 1-2 weeks to build and setup

Motivation: Many companies stay with the tradtional data storage methods, such as manually saving as a single excel in HDD, or storing thousands of files in Cloud Drive like OneDrive. However, they are not safe and efficient for frequent use in a daily manner. Constantly opening and closing Excel that contains thousands of sheets will cause the program randering slow and easy to crash; and OneDrive has the consistent issue of losing T-1 backup files. Therefore, I wanted to find a way to store and protect data in a more organizable and easier way, while being less costly. This is why I found Django, which helps me build a SQL database in a considerably short time in Python. 

Reason: Reduce the risk of losing data, improve the efficiency to query and store data, shorten the latency of loading data, and use database wherever I go. 

Learn: How to build Django web application for database usage, how to build and test the pipeline for database, how to connect local Django to AWS and test the compatability. 

Current difficulty: Extra cost from AWS RDs and insufficienty protection for database.

Current snowflake schema ER diagram map: 
![image](https://github.com/fahygao/full-stack-dbase-project/assets/48902014/5679a918-c01e-4224-9307-333576a3365b)


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
ref: [AWS Official Setup](https://github.com/aws/aws-elastic-beanstalk-cli-setup)

### 3.1. Prerequisites

You will need to have the following prerequisites installed before running the install script.

* **Git**
  * If not already installed you can download git from the [Git downloads page](https://git-scm.com/downloads).
* **Python**
  * We recommend that you install Python using the [pyenv](https://github.com/pyenv/pyenv) Python version manager. Alternately, you can download Python from the [Python downloads page](https://www.python.org/downloads/).
* **virtualenv**
  * Follow the [virtualenv documentation](https://virtualenv.pypa.io/en/latest/installation.html) to install virtualenv.

  PS: Reason to use virtual environments: 

  1. Dependency Management: Django applications often have specific versions and dependencies for Python packages. Using a virtual environment allows you to manage and control these dependencies independently for each project. It ensures that the project uses the correct versions of packages and avoids conflicts with other projects or the system-wide Python installation.

  2. Reproducible Environments: With a virtual environment, you can create an environment that closely matches your development environment. It helps ensure that the application behaves consistently across different environments, such as development, testing, and production. By specifying the exact versions of packages in the virtual environment, you can replicate the environment easily and avoid unexpected issues caused by version mismatches.

  3. Isolation and Security: Virtual environments provide isolation by creating a separate environment for each project. This isolation prevents conflicts between packages and allows for better security. It ensures that any changes or installations made within the virtual environment do not affect the global Python installation or other projects running on the same server.

  4. Portability: Virtual environments make the deployment process more portable. You can package the virtual environment along with the project code, making it easier to deploy the application on different servers or cloud platforms. It provides a consistent environment for the application to run, regardless of the underlying system configuration.

  5. Easy Management: Virtual environments simplify the management of project dependencies. You can use tools like pip (Python package manager) to install, update, and remove packages within the virtual environment without affecting other projects or the system-wide Python installation. It allows for better control and organization of project-specific dependencies.

### 3.2. Clone this repository

Use the following:

```
git clone https://github.com/aws/aws-elastic-beanstalk-cli-setup.git
```

### 3.3. Install or Upgrade the EB CLI

#### Windows
In **PowerShell** or in a **Command Prompt** window:

```
python .\aws-elastic-beanstalk-cli-setup\scripts\ebcli_installer.py
```

## 4. Alternative method to download EB

To set up Elastic Beanstalk (EB) on Windows using cmd, you can follow these steps:

1. Install Python: EB requires Python, so make sure you have Python installed on your Windows machine. You can download the latest version of Python from the official Python website (https://www.python.org/) and follow the installation instructions.

2. Install the AWS Command Line Interface (CLI): The AWS CLI allows you to interact with AWS services, including Elastic Beanstalk. You can download the AWS CLI installer for Windows from the official AWS Command Line Interface website (https://aws.amazon.com/cli/) and follow the installation instructions.

3. Configure AWS CLI: Once the AWS CLI is installed, open a command prompt or PowerShell window and run the following command:
   ```
   aws configure
   ```
   This command will prompt you to enter your AWS Access Key ID, AWS Secret Access Key, default region name, and default output format. You can obtain the Access Key ID and Secret Access Key from the AWS Management Console.

4. Install EB CLI: The EB CLI is a command-line interface specifically for Elastic Beanstalk. To install it, open a command prompt or PowerShell window and run the following command:
   ```
   pip install awsebcli
   ```

5. Verify the installation: After installing the EB CLI, you can verify the installation by running the following command:
   ```
   eb --version
   ```
   This should display the version of the EB CLI if it was installed correctly.

6. Initialize your EB project: Navigate to the root directory of your Django project using the command prompt or PowerShell window. Then, run the following command to initialize your EB project:
   ```
   eb init
   ```
   This command will guide you through the process of configuring your EB environment, selecting a region, and setting up your AWS credentials. If you are not sure, please follow [this](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-configuration.html)

7. Check your EB environment: After initializing your project, you can check the status with the existing eb environment (one68-db-env) by running the following command:
   ```
   eb status one68-db-env
   ```
   This will build a connection to our AWS EB based on the configuration settings you specified during the initialization.

8. Deploy your application: To deploy your Django application to the EB environment, use the following command:
   ```
   eb deploy
   ```
   This command will package and upload your application code to the EB environment, where it will be deployed and made accessible.


PS: for some reasons, I can only load css properly, when I use ``` Python STATICFILES_DIRS = (
    os.path.join(BASE_DIR, "static");``` however, ```STATIC_ROOT = "static"``` can only work after uploading to AWS.  


## 5. License 

This library is licensed under the MIT License.

