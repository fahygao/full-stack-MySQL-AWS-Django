# How to update dbase.

## 1. General steps to make change and upload to AWS. 
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
If the change is related to frontend, 
Identify what changes you want to make, such as html, css, js or scss, please only modify corresponding html files in templates folder. 
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
| add_deal.html    | Add New Deal  |
| base.html      | All the django backend dbase pages configuration|
| charts.html      | template for adding charts |
| deal_detail.html     | Deal detail page for each deal|
| feeds_update.html    | All Events page html|
| form.html     | Adding Recent Events|
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

