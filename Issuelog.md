# Issue related to Data Base:

1. What caused the issue? 

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

2. 
