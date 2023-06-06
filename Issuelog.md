# Issues Log:
January.02.2023
---
#### What caused the issue: deploy failed to the AWS EB? 

Deploy failed because packages failed to mysql load and download. 
Error message: 
``` Python
Collecting mysqlclient==2.1.1
  Downloading mysqlclient-2.1.1.tar.gz (88 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 88.1/88.1 kB 20.9 MB/s eta 0:00:00
  Preparing metadata (setup.py): started
  Preparing metadata (setup.py): finished with status 'error'

2023/01/03 22:12:42.348066 [INFO]   error: subprocess-exited-with-error
  
  × python setup.py egg_info did not run successfully.
  │ exit code: 1
  ╰─> [16 lines of output]
      /bin/sh: mysql_config: command not found
      /bin/sh: mariadb_config: command not found
      /bin/sh: mysql_config: command not found
      Traceback (most recent call last):
        File "<string>", line 36, in <module>
        File "<pip-setuptools-caller>", line 34, in <module>
        File "/tmp/pip-install-dw_djq9i/mysqlclient_2c8300e5eee443f7bd3392e323820cce/setup.py", line 15, in <module>
          metadata, options = get_config()
        File "/tmp/pip-install-dw_djq9i/mysqlclient_2c8300e5eee443f7bd3392e323820cce/setup_posix.py", line 70, in get_config
          libs = mysql_config("libs")
        File "/tmp/pip-install-dw_djq9i/mysqlclient_2c8300e5eee443f7bd3392e323820cce/setup_posix.py", line 31, in mysql_config
          raise OSError("{} not found".format(_mysql_config_path))
      OSError: mysql_config not found
      mysql_config --version
      mariadb_config --version
      mysql_config --libs
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
error: metadata-generation-failed

× Encountered error while generating package metadata.
╰─> See above for output.

note: This is an issue with the package mentioned above, not pip.
hint: See above for details.
```

Solution: I manually downloaded all the packages to the virtual environments first ( ``` Python pip install -r requirements.txt```), then deployed again. 


June.02.2023
---
## What caused the issue: backend sever rejected to respond our Post/Get request? 

The backend server (nginx) rejected to connect to the new environment (django-env4) and application (one68_db_env), then the new domain could access to the backend apis and etc.

In file (.elasticbeanstalk/config.yml):
```Python
branch-defaults:
  default:
    environment: one68-db-env
    # django-env4 #this is the new environment added on Friday
    group_suffix: null
environment-defaults:
  one68-db-env:
    branch: null
    repository: null
global:
  application_name: dj-db-one68
  # one68_db_env #this is the new application added on Friday
  ...
```
However, I do not know how to get the exact line to show what the backend server is connecting to. Need to do more research and ask AWS might help. 

#### What is the difference between different environment and applications? 

##### Definitions:

In AWS EB, an environment is associated with an application. An environment represents a specific instance of your application, including the underlying resources and configurations. So, you can consider an environment as being nested within an application. When creating a domain in AWS EB, you first create an application that represents your software solution. Once the application is created, you then set up an environment within that application. The environment is where your application will be deployed and accessible through a domain.When you deploy your project to AWS EB, it automatically provisions the necessary resources to run your application, including instances. An instance in AWS EB is a virtual machine that hosts your application and acts as a container for the server environment provided by AWS. Once your project is deployed and running on the instance within the environment, you can access your application through the domain you created. The domain serves as the entry point to your application, allowing users to interact with it over the internet.

##### Differences between different applications

In AWS Elastic Beanstalk (EB), two different applications represent separate and independent software solutions or systems. Each application in AWS EB has its own set of resources, configurations, and deployments. Here are the key differences between two different applications in AWS EB:

1. Isolation: Each application operates independently and is isolated from other applications. They have their own environments, resources, and configurations. This isolation ensures that changes or issues in one application do not affect the others.

2. Codebase: Each application has its own codebase, which consists of the application's source code, dependencies, and configurations. This means that you can have different codebases for different applications, allowing you to manage and deploy distinct software solutions.

3. Environments: Applications can have multiple environments associated with them, representing different deployment targets or stages (e.g., development, testing, staging, production). Each environment within an application can have its own configurations, such as environment variables, scaling options, instance types, and security groups.

4. Access and Permissions: AWS EB provides granular access control and permissions management at the application level. You can assign different users or teams permissions to manage specific applications, allowing for fine-grained control over who can modify or deploy to each application.

5. Resource Allocation: AWS EB allows you to allocate resources (such as instances, databases, and storage) independently for each application. This flexibility enables you to allocate resources based on the specific needs and demands of each application, optimizing performance and cost.

In summary, different applications in AWS EB represent separate software solutions with their own resources, configurations, and deployments. They provide isolation, allowing you to manage and deploy distinct codebases and environments for each application independently. So in our case, this is also the reason why we cannot use the old backend server config to match with the new application and environment.
