jupyter
=======

Setup system and create user

.. code-block:: console

  sudo apt install python3-venv
  sudo adduser --shell /bin/bash --disabled-password axju

setup venv and install jupyter for the new user

.. code-block:: console

  sudo -u axju -i
  python3 -m venv venv
  source venv/bin/activate
  pip install --upgrade pip jupyter

Create work folder

.. code-block:: console

  mkdir /home/axju/jupyter

Create run script

.. code-block:: console

  nano start_jupyter

with

.. code-block:: text

  #!/bin/bash
  source /home/axju/venv/bin/activate
  cd /home/axju/jupyter
  jupyter-notebook

Create certificate

.. code-block:: console

  cd
  mkdir certificate
  cd certificate/
  openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout cert.key -out cert.pem

Generate password hash in file ~/.jupyter/jupyter_notebook_config.json

.. code-block:: console

  jupyter notebook password

Create jupyter configuration

.. code-block:: console

  jupyter notebook --generate-config
  nano .jupyter/jupyter_notebook_config.py

Edit configuration

.. code-block:: text

  c.NotebookApp.password = u'<password hash>'
  c.NotebookApp.port = 9999
  c.NotebookApp.notebook_dir = '/home/axju/jupyter'
  c.NotebookApp.allow_origin = 'j.short-report.de'
  c.NotebookApp.allow_remote_access = True
  c.NotebookApp.open_browser = False


nginx
-----
Edit

.. code-block:: console

  sudo nano /etc/nginx/sites-available/jupyter


with

.. code-block:: text

  upstream notebook {
    server localhost:9999;
  }

  server {
    listen 443 ssl;
    server_name <domain or ip>;

    ssl_certificate        /home/axju/certificate/cert.pem;
    ssl_certificate_key    /home/axju/certificate/cert.key;

    location / {
      proxy_pass            http://notebook;
      proxy_set_header      Host $host;
    }

    location ~ /api/kernels/ {
      proxy_pass            http://notebook;
      proxy_set_header      Host $host;
      # websocket support
      proxy_http_version    1.1;
      proxy_set_header      Upgrade "websocket";
      proxy_set_header      Connection "Upgrade";
      proxy_read_timeout    86400;
    }

    location ~ /terminals/ {
      proxy_pass            http://notebook;
      proxy_set_header      Host $host;
      # websocket support
      proxy_http_version    1.1;
      proxy_set_header      Upgrade "websocket";
      proxy_set_header      Connection "Upgrade";
      proxy_read_timeout    86400;
    }
  }

Enable site, check config and restart nginx

.. code-block:: console

  sudo ln -s /etc/nginx/sites-available/jupyter /etc/nginx/sites-enabled/
  sudo rm /etc/nginx/sites-enabled/default
  sudo nginx -t
  sudo service nginx restart


Setup supervisor
----------------
Edit

.. code-block:: console

  sudo nano /etc/supervisor/conf.d/jupyter.conf

with

.. code-block:: text

  [program:jupyter]
  user=axju
  directory=/home/axju/
  command=/home/axju/start_jupyter
  autostart=false
  autorestart=true
  startsecs=0

  stdout_logfile=/var/log/jupyter/stdout.log
  stdout_logfile_maxbytes=1MB
  stdout_logfile_backups=10
  stdout_capture_maxbytes=1MB
  stderr_logfile=/var/log/jupyter/stderr.log
  stderr_logfile_maxbytes=1MB
  stderr_logfile_backups=10
  stderr_capture_maxbytes=1MB

Create log folder and enable jupyter

.. code-block:: console

  sudo mkdir /var/log/jupyter

  sudo supervisorctl reread
  sudo supervisorctl update

Control jupyter
---------------
.. code-block:: console

  sudo supervisorctl status jupyter
  sudo supervisorctl start jupyter
  sudo supervisorctl stop jupyter
