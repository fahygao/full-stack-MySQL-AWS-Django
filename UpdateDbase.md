# How to update dbase.

##1. General steps to make change and upload to AWS. 
If the change is related to backend, such as adding a new attribute or modifying existing attributes to current models: 
1. Make change to 'models.py' 
2. Save and run (in your folder's directory) 
    ```Python 
    python manage.py makemigrations
    ``` 
    then run:
    ```Python 
    python manage.py migrate```
    then: 
    ```Python 
    eb deploy```

Identify what changes you want to make.
1. If the change is related to frontend, such as html, css, js or scss, please only modify corresponding html files in templates folder. 

To make any change in dbase, please follow the steps below. 

