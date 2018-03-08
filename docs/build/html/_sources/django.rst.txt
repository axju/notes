Django
======

Install
-------
.. code-block:: console

  pip3 install django

Create new project
------------------
.. code-block:: console

  django-admin startproject mysite


Setup virtual environments
--------------------------
.. code-block:: console

  cd mysite
  python3 -m venv myvenv
  myvenv/Scripts/active
  source myvenv/bin/activate
  pip install django



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
