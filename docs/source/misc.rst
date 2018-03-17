Misc
====

Python
------
Create a packege
~~~~~~~~~~~~~~~~
`python-packaging <https://python-packaging.readthedocs.io/en/latest/minimal.html>`_

`the-hitchhikers-guide-to-packaging <https://the-hitchhikers-guide-to-packaging.readthedocs.io/en/latest/quickstart.html>`_




Raspberry Pi
------------
.. code-block:: console

  unzip -p 2017-11-29-raspbian-stretch.zip | sudo dd of=/dev/sdX bs=4M conv=fsync
  dd bs=4M if=2017-11-29-raspbian-stretch.img of=/dev/sdX conv=fsync

  sudo raspi-config

Default login pi:raspberry

Git and GitHub
--------------
.. code-block:: console

  sudo apt install git

  git config --global user.name "user_name"
  git config --global user.email "email_id"
  git status

  git clone https://github.com/user/project.git
  git init Mytest
  git add smaple.c
  git commit -m "some_message"
  git remote add origin https://github.com/user/project.git
  git push origin master

RabbitMQ
--------
.. code-block:: console

  sudo apt install -y erlang rabbitmq-server

  sudo systemctl enable rabbitmq-server
  sudo systemctl start rabbitmq-server
  sudo systemctl status rabbitmq-server


Advanced Packaging Tool
-----------------------

.. code-block:: console

  sudo apt update
  sudo apt autoremove
