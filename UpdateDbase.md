# How to update dbase.

## 1. General steps to make change and upload to AWS
### 1.1 Backend and frontend

If the change is related to backend, such as adding a new attribute or modifying existing attributes to current models: 
1. Make change to 'models.py' 
2. Save and run (in your folder's directory) 
    ```Python 
    python manage.py makemigrations
    ``` 
    then run:
    ```Python 
    python manage.py migrate
    ```
    then: 
    ```Python
    eb deploy
    ```
### 1.2 Only frontend

If the change is related to frontend, identify what changes you want to make, such as html, css, js or scss, please only modify corresponding html files in templates folder. 
1. Make change to 'XXXX.html' 
2. Run test (in your folder's directory) 
    ```Python 
    python manage.py runserver
    #or
    python manage.py runserver localhost:3000
    ``` 
    then go to the local ip to view the change and if the changes are correct, run:
    ```Python
    eb deploy
    ```
### 2. File documentation.
Under **db4_one68**
| File Name  | Functionality |
| ------------- | ------------- |
| __pycache___ |bytecode files (.pyc) for improved performance (Do not need to modify)|
|  __init__.py | Null  |
| settings.py  | Central configurations and file path |
| urls.py      | Urls path related to each pages |
| views.py     | Setup display of each pages with htmls |
| wsgi.py      | Web Server Gateway Interface|

Under **deals**
| File Name  | Functionality |
| ------------- | ------------- |
| __pycache___  | Bytecode files (.pyc) for improved performance|
| migrations    | Historical migration files |
| static        | Frontend attributes related to pages under deals|
| template      | Htmls (all of them are not functioning now |
|__init__.py    | Null  |
| admin.py      | All the django backend dbase pages configuration|
| apps.py       | Contain the config for 'deals' |
| forms.py      | Attributes related to forms |
| models.py     | All the tables and attributes related to each table|
| serializers.py| All the attributes related to APIs|
| tests.py      | null (this can be used to do local testing on webserver and queryset|
| urls.py       | Urls related to APIs|
| views.py      | Display set up for deal details page|

Under **media**: all the media files store locally (this folder should be empty).

Under **static**: all the css and js related to all pages (please do not make any change in this folder unless you understand basic css and js knowledge. Some key files are listed below:
| File Name  | Functionality |
| ------------- | ------------- |
|  admin_color.css |    All the css attributes related to backend dbase page |
|  admin/css/forms.css| All the backend form css attributes (change the textbox size and so on) |

Under **templates**: all the static web pages. I will add * to all the origin files I created.
| File Name  | Functionality |
| ------------- | ------------- |
| admin/base_site.html  | Django backend dbase site|
| 401.html              | Display when unauthorized (have not used) |
| 404.html       | Display when files nto found (have not used)|
| 500.html      | Htmls (all of them are not functioning now |
| add_deal.html*    | Add New Deal  |
| base.html*       | All the django backend dbase pages configuration|
| charts.html      | template for adding charts |
| deal_detail.html*      | Deal detail page for each deal|
| feeds_update.html*     | All Events page html|
| form.html*      | Adding Recent Events|
| index.html*       | Dashboard|
| layout-sidenav-light.html    | template to change the theme of the website|
| layout-static.html     |template to change theme of the website|
| login.html       | Login |
| password.html    | Reset password (Current not functioning)|
| recentchanges.html*     | Real time entries page html|
| register.html       | Register a new user (Current not functioning)|
| search.html*    | Search|
| tables.html     | tables template html|
| update_event.html*       | Form for recent event (Recent event update page)|
## 3. Most frequent changes & examples.

### 3.1 Change the general css of textbox or color in backend: 
1. Locate the class name using inspection on the web page.
2. Go to file base_site.html and add the corresponding styles to the same class.

Exmple: I want to make modification on all the input box width

![image](https://github.com/fahygao/full-stack-dbase-project/assets/48902014/eaf14401-b9d7-4db9-a802-d06616ac002d)

I locate the css class of this textbox: 

![image](https://github.com/fahygao/full-stack-dbase-project/assets/48902014/d929d9f5-230f-489f-82e5-8000841cdc71)

Then I go to base_site.html, ```ctrl+F``` to find the class name and see if we have it. If not, create a new class. 

![image](https://github.com/fahygao/full-stack-dbase-project/assets/48902014/16629e15-043f-4801-9144-f526e74aa4e1)

And change the attributes to what you would like to set, and ```ctrl + S```. 

Finish deploying by following the steps in - [1.2 Only frontend](#12-Only-frontend)

### 3.2 Add attribute into the Deal Basics:
1. Locate the deal basics class 'DealName' in ```deals/models.py```. 
2. Add a new row before 'reviewed' with corresponding model attributes (CharField, RichTextfield or DateField). Then save the file ```ctrl+S```
3. Go to forms.py, add the new attribute to the class 'DealForm' (remember to add in fields, labels, and widgets).  
4. Go to admin, add the new attribute to the class 'DealAdmin'.
5. Check if we need to add the attribute to ```serializers.py``` as well. Normally, we do not need to change this file. 
6. Last, go to ```deal_detail.html``` and add the new attribute to the corresponding location where you want to see in the front end. Basic format: 
```Python
    {% if i.NEW_ATT_NAME %}
         <div class="deal-basics-title">NEW_ATT_NAME:</div> {{i.NEW_ATT_NAME}}<br>
    {% endif %}
```
7.  Finish deploying by following the steps in - [1.1  Backend and frontend](#11-Backend-and-frontend)
