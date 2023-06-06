# Issues related to DataBase:

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

## What is the difference between different environment and applications? 

### Definitions:

In AWS EB, an environment is associated with an application. An environment represents a specific instance of your application, including the underlying resources and configurations. So, you can consider an environment as being nested within an application. When creating a domain in AWS EB, you first create an application that represents your software solution. Once the application is created, you then set up an environment within that application. The environment is where your application will be deployed and accessible through a domain.When you deploy your project to AWS EB, it automatically provisions the necessary resources to run your application, including instances. An instance in AWS EB is a virtual machine that hosts your application and acts as a container for the server environment provided by AWS. Once your project is deployed and running on the instance within the environment, you can access your application through the domain you created. The domain serves as the entry point to your application, allowing users to interact with it over the internet.

### Differences between different applications

In AWS Elastic Beanstalk (EB), two different applications represent separate and independent software solutions or systems. Each application in AWS EB has its own set of resources, configurations, and deployments. Here are the key differences between two different applications in AWS EB:

1. Isolation: Each application operates independently and is isolated from other applications. They have their own environments, resources, and configurations. This isolation ensures that changes or issues in one application do not affect the others.

2. Codebase: Each application has its own codebase, which consists of the application's source code, dependencies, and configurations. This means that you can have different codebases for different applications, allowing you to manage and deploy distinct software solutions.

3. Environments: Applications can have multiple environments associated with them, representing different deployment targets or stages (e.g., development, testing, staging, production). Each environment within an application can have its own configurations, such as environment variables, scaling options, instance types, and security groups.

4. Access and Permissions: AWS EB provides granular access control and permissions management at the application level. You can assign different users or teams permissions to manage specific applications, allowing for fine-grained control over who can modify or deploy to each application.

5. Resource Allocation: AWS EB allows you to allocate resources (such as instances, databases, and storage) independently for each application. This flexibility enables you to allocate resources based on the specific needs and demands of each application, optimizing performance and cost.

In summary, different applications in AWS EB represent separate software solutions with their own resources, configurations, and deployments. They provide isolation, allowing you to manage and deploy distinct codebases and environments for each application independently. So in our case, this is also the reason why we cannot use the old backend server config to match with the new application and environment.
