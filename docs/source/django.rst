Django
======

Setup virtual environments
--------------------------
.. code-block:: console

  cd mysite
  python3 -m venv venv
  venv/bin/active
  source myvenv/bin/activate
  pip install django


Create new project
------------------
.. code-block:: console

  django-admin startproject mysite


Useful
------

Internationalization
~~~~~~~~~~~~~~~~~~~~

http://www.marinamele.com/taskbuster-django-tutorial/internationalization-localization-languages-time-zones

https://docs.djangoproject.com/en/2.0/topics/i18n/translation/

.. code-block:: python

  python manage.py makemessages -l de
  python manage.py compilemessages -l de


python-decouple
~~~~~~~~~~~~~~~
Use the decouple package for saving secret settings.

install:

.. code-block:: console

  pip install python-decouple

in your settings.py:

.. code-block:: python

  from decouple import config

  SECRET_KEY = config('SECRET_KEY')

create file .env with:

.. code-block:: python

  SECRET_KEY=value


dj-database-url
~~~~~~~~~~~~~~~
The dj-database-url help to configuret the db-settings in one line.

install:

.. code-block:: console

  pip install dj-database-url

in your settings.py:

.. code-block:: python

  import dj_database_url

  DATABASES = {
      'default': dj_database_url.config(
          default='sqlite:///db.sqlite3'
      )
  }




Manage the project
--------------------------
.. code-block:: console

  python manage.py startapp
  python manage.py makemigrations
  python manage.py migrate
  python manage.py test
  python manage.py createsuperuser
  python manage.py collectstatic


Structure
---------
.. code-block:: console

  mysite
  ├── myapp
  │   ├── migrations
  │   │   └── __init__.py
  │   ├── __init__.py
  │   ├── admin.py
  │   ├── apps.py
  │   ├── models.py
  │   ├── tests.py
  │   └── views.py
  ├── mysite
  │   ├── settings
  │   │   ├── __init__.py
  │   │   ├── base.py
  │   │   └── development.py
  │   ├── __init__.py
  │   ├── urls.py
  │   ├── wsgi.py
  │   └── uwsgi.ini
  ├── manage.py
  ├── server.py
  ├── README.md
  └── requirements.txt
