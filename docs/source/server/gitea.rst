gitea
=====
Setup system
------------
.. code-block:: console

  sudo apt-get install nginx postgresql git supervisor certbot
  sudo adduser -m --shell /bin/bash --disabled-password git

Get gitea
---------
Find the right binary on https://dl.gitea.io/gitea

For linux 64 bit: https://dl.gitea.io/gitea/master/gitea-master-linux-amd64

For Raspberry Pi B: https://dl.gitea.io/gitea/master/gitea-master-linux-arm-7

.. code-block:: console

  sudo -u git -i
  mkdir gitea
  cd gitea
  wget -O gitea https://dl.gitea.io/gitea/master/gitea-master-linux-amd64
  chmod +x gitea
  exit

Create database
---------------
.. code-block:: console

  sudo su postgres
  psql

Create user gitea with <password>

.. code-block:: console

  CREATE USER gitea WITH PASSWORD '<password>';
  CREATE DATABASE gitea OWNER gitea;
  \q
  exit

Setup supervisor
----------------
Edit

.. code-block:: console

  sudo nano /etc/supervisor/conf.d/gitea.conf

with

.. code-block:: text

  [program:gitea]
  directory=/home/git/gitea/
  command=/home/git/gitea/gitea web
  autostart=true
  autorestart=true
  startsecs=10
  stdout_logfile=/var/log/gitea/stdout.log
  stdout_logfile_maxbytes=1MB
  stdout_logfile_backups=10
  stdout_capture_maxbytes=1MB
  stderr_logfile=/var/log/gitea/stderr.log
  stderr_logfile_maxbytes=1MB
  stderr_logfile_backups=10
  stderr_capture_maxbytes=1MB
  environment = HOME="/home/git", USER="git"
  user = git

Create log folder and enable gitea

.. code-block:: console

  sudo mkdir /var/log/gitea

  sudo supervisorctl reread
  sudo supervisorctl update
  sudo supervisorctl status gitea


nginx
-----
Edit

.. code-block:: console

  sudo nano /etc/nginx/sites-available/gitea


with

.. code-block:: text

  server {
    listen 80;
    server_name <domain or ip>;

    location / {
        proxy_pass http://localhost:3000;
    }

    proxy_set_header X-Real-IP $remote_addr;
  }

Enable site, check config and restart nginx

.. code-block:: console

  sudo ln -s /etc/nginx/sites-available/gitea /etc/nginx/sites-enabled/
  sudo rm /etc/nginx/sites-enabled/default
  sudo nginx -t
  sudo service nginx restart

Configure gitea
---------------
Go to <domain or ip> and configure your gitea. Or you can edit the app.ini file

.. code-block:: console

  sudo -u git -i
  nano gitea/custom/conf/app.ini
  exit

See my app.ini file below. After you edit the configuration restart gitea

.. code-block:: console

  sudo supervisorctl restart gitea

letsencrypt (optional but recommended)
--------------------------------------
Edit

.. code-block:: console

  sudo nano /etc/nginx/sites-available/gitea

with

.. code-block:: text

  server {
    listen 80;
    server_name <domain>;
    return 301 <domain>$request_uri;
  }

  server {
    listen 443 ssl;
    server_name <domain>;

  # First without the letsencrypt files. Because they do not exist yet.
  #	ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem;
  # ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem;

  	location /.well-known/acme-challenge/ {
          root /home/git/gitea/letsencrypt;
      }

      location / {
          proxy_pass http://localhost:3000;
      }
  }

Run certbot

.. code-block:: console

  sudo -u git -i
  mkdir /home/git/gitea/letsencrypt
  cd /home/git/gitea/letsencrypt
  certbot certonly --webroot -w /home/git/gitea/letsencrypt -d <domain>

Now remove the # from line 12, 13

.. code-block:: console

  sudo nano /etc/nginx/sites-available/gitea

.. code-block:: text

  ...

  # First without the letsencrypt files. Because they do not exist yet.
   	ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/<domain>/privkey.pem;

  ...

Restart nginx

.. code-block:: console

  sudo systemctl restart nginx

rst-files (optional)
--------------------
Install docutils

.. code-block:: console

  sudo apt-get install python3-docutils

Setup markup for rst-files

.. code-block:: console

  sudo -u git -i
  nano gitea/custom/conf/app.ini
  exit

Add

.. code-block:: text

  [markup.restructuredtext]
  ENABLED = true
  FILE_EXTENSIONS = .rst
  RENDER_COMMAND = rst2html
  IS_INPUT_FILE = false

Do not forget to restart gitea

.. code-block:: console

  sudo supervisorctl restart gitea

My app.ini-file
---------------
.. code-block:: text

  APP_NAME = Git for AxJu
  RUN_USER = git
  RUN_MODE = prod

  [security]
  INTERNAL_TOKEN = <hide>
  INSTALL_LOCK   = true
  SECRET_KEY     = <hide>

  [database]
  DB_TYPE  = postgres
  HOST     = 127.0.0.1:5432
  NAME     = gitea
  USER     = gitea
  PASSWD   = <password>
  SSL_MODE = disable
  PATH     = /home/git/gitea/data/gitea.db

  [repository]
  ROOT = /home/git/gitea-repositories

  [server]
  SSH_DOMAIN       = <domain>
  DOMAIN           = <domain>
  HTTP_PORT        = 3000
  ROOT_URL         = https://<domain>/
  DISABLE_SSH      = false
  SSH_PORT         = 22
  LFS_START_SERVER = true
  LFS_CONTENT_PATH = /home/git/gitea/data/lfs
  LFS_JWT_SECRET   = <hide>
  OFFLINE_MODE     = false

  [mailer]
  ENABLED = false

  [service]
  REGISTER_EMAIL_CONFIRM            = false
  ENABLE_NOTIFY_MAIL                = false
  DISABLE_REGISTRATION              = true
  ALLOW_ONLY_EXTERNAL_REGISTRATION  = false
  ENABLE_CAPTCHA                    = false
  REQUIRE_SIGNIN_VIEW               = false
  DEFAULT_KEEP_EMAIL_PRIVATE        = false
  DEFAULT_ALLOW_CREATE_ORGANIZATION = true
  DEFAULT_ENABLE_TIMETRACKING       = true
  NO_REPLY_ADDRESS                  = noreply.example.org

  [picture]
  DISABLE_GRAVATAR        = false
  ENABLE_FEDERATED_AVATAR = true

  [openid]
  ENABLE_OPENID_SIGNIN = true
  ENABLE_OPENID_SIGNUP = true

  [session]
  PROVIDER = file

  [log]
  MODE      = file
  LEVEL     = Info
  ROOT_PATH = /home/git/gitea/log

  [markup.restructuredtext]
  ENABLED = true
  FILE_EXTENSIONS = .rst
  RENDER_COMMAND = rst2html
  IS_INPUT_FILE = false
