# djangocms-postgres-docker
Build a docker environment with Django 2, Django CMS 3.7, and Postgres 4 running on python 3.7

## How to use this docker images
Once you clone this repositorie yo can see the following file structure:\
&nbsp;\
djangocms-postgres-docker\
&nbsp;&nbsp;|-- docker-compose.yml\
&nbsp;&nbsp;|-- Dockerfile\
&nbsp;&nbsp;|-- requirements.txt\
&nbsp;\
Perform this command to create your Django project (you can change the project name "djangoexample") inlude the "point" at the end of the command line:
```
sudo docker-compose run web django-admin startproject djangoexample .
```
If you are in Linux, need to change the owner user of the project name folder and manage.py file:
```
sudo chown -R $USER:$USER djangoexample manage.py
```
In your new project root directory ("djangoexample" in our example) create 1 folder named "templates" and a file named "cms_menu.py", then into the templates directory creat other file named home.html, the final file structure must look like this:\
&nbsp;\
djangocms-postgres-docker\
&nbsp;&nbsp;&nbsp;&nbsp;|-- djangoexample\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|-- templates\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|-- home.html\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|-- __ init.py __ \
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|-- cms_menu.py_\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|-- settings.py_\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|-- urls.py_\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|-- wsgi.py_\
&nbsp;&nbsp;&nbsp;&nbsp;|-- docker-compose.yml\
&nbsp;&nbsp;&nbsp;&nbsp;|-- Dockerfile\
&nbsp;&nbsp;&nbsp;&nbsp;|-- manage.py\
&nbsp;&nbsp;&nbsp;&nbsp;|-- requirements.txt\
&nbsp;\
In the _home.html_ file copy this code:
```
{% load cms_tags sekizai_tags %}
<html>
    <head>
        <title>{% page_attribute "page_title" %}</title>
        {% render_block "css" %}
    </head>
    <body>
        {% cms_toolbar %}
        {% load menu_tags %}
        <ul>
            {% show_menu 0 100 100 100 %}
        </ul>
        {% placeholder "content" %}
        {% render_block "js" %}
    </body>
</html>
```
In thr _cms_menu.py_ file copy this code:
```
from menus.base import NavigationNode
from menus.menu_pool import menu_pool
from django.utils.translation import ugettext_lazy as _
from cms.menu_bases import CMSAttachMenu

class TestMenu(CMSAttachMenu):

    name = _("test menu")

    def get_nodes(self, request):
        nodes = []
        n1 = NavigationNode(_('sample anchor 1'), "/#sa1", 1)
        n2 = NavigationNode(_('sample anchor 2'), "/#sa2", 2)
        n3 = NavigationNode(_('sample anchor 3'), "/#sa3", 3)

        nodes.append(n1)
        nodes.append(n2)
        nodes.append(n3)
        return nodes

menu_pool.register_menu(TestMenu)
```
Update the _settings.py_ file in this sections:
```
ALLOWED_HOSTS = ['*']
```
```
# Change djangoexample by your project name
INSTALLED_APPS = [
    'djangocms_admin_style',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.sites',
    'djangoexample',
    'cms',
    'menus',
    'treebeard',
    'sekizai',
    'filer',
    'easy_thumbnails',
    'mptt',
    'djangocms_text_ckeditor',
    'djangocms_link',
    'djangocms_file',
    'djangocms_picture',
    'djangocms_video',
]
```
