django CMS Demo
===============

A demo site that shows a simple django CMS setup. I've included the sqlite
database and some media into the project to make it easier to see how 
django CMS works.

Get the code and run the project locally.  Make sure to checkout the 
frontend editing feature and try to add some pages.

This demo assumes you know a bit about Django, Python, virtualenv, pip and that you
have Python, pip and virtualenv installed already.  Also, if you aren't using virtualenv
to pip install the requirements, shame on you.  But, it'll work
without it.


Installation (with included database)
-------------------------------------

::

    $ git clone git://github.com/andrewschoen/django-cms-demo.git
    $ cd django-cms-demo
    $ virtualenv env
    $ source env/bin/activate
    $ pip install -r requirements.txt
    $ python manage.py runserver


Installation (without included database)
----------------------------------------

::

    $ git clone git://github.com/andrewschoen/django-cms-demo.git
    $ cd django-cms-demo
    $ virtualenv env
    $ source env/bin/activate
    $ pip install -r requirements.txt
    $ python manage.py syncdb --all
    $ python manage.py migrate --fake
    $ python manage.py runserver

Viewing the demo
----------------

Open the browser, navigate to http://localhost:8000

Login to the admin at http://localhost:8000/admin

Admin credentials
+++++++++++++++++

If you're using the included database, here are the admin credentials.

un: admin

pw: djangocms


Alternative Installation (Ubuntu 12.04 Server Production - Apache)
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

If you want to install this version in production and you have trouble with the Django documention setting this up,
here an alternative way to install it on a (vanilla) Ubuntu 12.04 Server installation.

This is assuming you have a fresh install of Ubuntu Server 12.04 (I used 64 bit but it should apply for 32 bit as well)
and that you want to install in /opt/django-cms and the super user is your sudoer user account.
Furthermore, it is assumed that you want the database to run on (in my opinion) best DBMS PostgreSQL Serveri and
that you want to use Apache as your web server. Other web servers and DBMS might run marginally faster in some
setups, but Apache is the most widely used and stable web server and PostgreSQL is the most advanced DBMS that I have
come across and this includes Oracle and MySQL.
Also, it is assumed that you want to use django_cms as the db name, user name and password. Of course it is highly
recommended you use better names for production setups.

::

    $ sudo apt-get update
    $ sudo apt-get install git python python-setuptools python-imaging python-psycopg2 gcc postgresql-9.1 python-dev apache2 libapache2-mod-wsgi
    $ sudo vi /etc/postgresql/9.1/main/pg_hba.conf # add the following line:
    local   all             all                                     md5
    $ sudo vi /etc/postgresql/9.1/main/postgresql.conf # add/uncomment this line:
    listen_addresses = 'localhost'
    $ sudo su postgres
    $ createuser -E -P django_cms
    Enter password for new role: django_cms
    Enter it again: django_cms
    Shall the new role be a superuser? (y/n) n
    Shall the new role be allowed to create databases? (y/n) y
    Shall the new role be allowed to create more new roles? (y/n) n
    $ createdb django_cms
    $ exit
    $ sudo easy_install pip
    $ cd /opt
    $ sudo chown super /opt
    $ git clone git://github.com/geekdenz/django-cms-demo.git django-cms
    $ cd django-cms
    $ sudo pip install -r requirements.txt
    $ python manage.py syncdb --all
    Would you like to create one now? (yes/no): yes
    Username (leave blank to use 'super'): admin
    E-mail address: mail@example.com
    Password: admin
    Password (again): admin
    Superuser created successfully.
    Test your installation up to now by running:
    $ ifconfig #grab the ip to enter in your browser
    $ python manage.py runserver 0.0.0.0:8000
    Go to your ip in your browser to test the installation.
    http://192.168.1.101:8000
    If it works, now configure apache.
    Add this to /etc/apache

        WSGIScriptAlias /django /opt/django-cms/django.wsgi
        <Directory /opt/django-cms>
            Order deny,allow
            Allow from all
        </Directory>

    Go to http://192.168.1.101/django/ and you should see a working CMS!
    Have fun exploring!

