Setup OS
========
Install Ubuntu MATE minimal from ubuntu netinstaller (`mini.iso <http://archive.ubuntu.com/ubuntu/dists/artful/main/installer-amd64/current/images/netboot/mini.iso>`_)

I use usb-creator-gtk to create th bootable USB medium.

.. code-block:: console

  sudo apt-get install usb-creator-gtk

Basic structure and tools
-------------------------
.. code-block:: console

  sudo apt purge firefox
  cd
  rm -r ./*
  mkdir projects storage sync tmp
  mkdir projects/python projects/blender projects/android
  mkdir storage/apps storage/docs storage/music storage/movies storage/memories

  sudo apt install -y build-essential language-pack-de curl


Change incoming folder with comand:

.. code-block:: console

  blueman-services

Change the desktop settings under: System -> Einstellungen -> Darstellung -> MATE Tweak


Install Nvidia GTX 750 ti
-------------------------
.. code-block:: console

  sudo add-apt-repository ppa:graphics-drivers/ppa
  sudo apt update
  sudo apt install nvidia-384 nvidia-settings


Vivaldi
-------
.. code-block:: console

  echo "deb http://repo.vivaldi.com/stable/deb/ stable main" | sudo tee /etc/apt/sources.list.d/vivaldi.list > /dev/null
  wget -O - http://repo.vivaldi.com/stable/linux_signing_key.pub | sudo apt-key add -
  sudo apt update
  sudo apt install vivaldi-snapshot

Adons:
  `uBlock Origin <https://chrome.google.com/webstore/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm?hl=de>`_


Dropbox
-------
.. code-block:: console

  sudo apt install nautilus-dropbox

Dropbox -> Preferences... -> Sync -> Dropbox folder location = sync/Dropbox

Atom
----
.. code-block:: console

  sudo add-apt-repository ppa:webupd8team/atom
  sudo apt update
  sudo apt install atom

Install some packages:

1. platformio-ide-terminal

   Add shortcuts Edit -> Keymap...

   .. code-block:: console

     'atom-text-editor':
       'ctrl-enter': 'platformio-ide-terminal:insert-selected-text'


2. rst-preview-pandoc

   Add Atom-package "language-reStructuredText" and install pandoc

   .. code-block:: console

    sudo apt install -y pandoc




VirtualBox
----------
Quelle: `virtualbox.org <https://www.virtualbox.org/wiki/Linux_Downloads>`_

.. code-block:: console

  sudo sh -c 'echo "deb http://download.virtualbox.org/virtualbox/debian $(lsb_release -sc) contrib" >> /etc/apt/sources.list'

  wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
  wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -

  sudo apt update
  sudo apt install virtualbox-5.2

  curl -O http://download.virtualbox.org/virtualbox/5.2.0/Oracle_VM_VirtualBox_Extension_Pack-5.2.0-118431.vbox-extpack
  sudo VBoxManage extpack install Oracle_VM_VirtualBox_Extension_Pack-5.2.0-118431.vbox-extpack
  rm Oracle_VM_VirtualBox_Extension_Pack-5.2.0-118431.vbox-extpack

VirtuelBox -> Einstellungen -> Allgemein -> Speicherot = 'storage/apps/VirtuelBox'


Basci tools and libs
--------------------
.. code-block:: console

  sudo apt install -y libssl-dev libffi-dev
  sudo apt install -y libxvidcore4 libfaac0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-ugly gstreamer1.0-plugins-bad gstreamer1.0-alsa gstreamer1.0-fluendo-mp3 gstreamer1.0-libav
  sudo apt install -y libreoffice libreoffice-l10n-de libreoffice-help-de
  sudo apt-get install hunspell hunspell-tools
  sudo apt install -y blender keepassx tree git vlc filezilla gimp gnome-disk-utility
  sudo apt install -y texlive texlive-lang-german texlive-latex-extra texmaker
  sudo apt install -y nmap


Thunderbird
-----------
.. code-block:: console

  sudo apt install -y thunderbird thunderbird-locale-de

Copy folder ~/.thunderbird/


Python
------
twine for uploading packages

.. code-block:: console

  sudo apt install -y python3-dev python3-pip python3-venv
  sudo -H pip3 install --upgrade pip
  sudo -H pip3 install sphinx



Android Studio
--------------
.. code-block:: console

  sudo apt -y install openjdk-8-jdk libc6-i386 lib32stdc++6 lib32gcc1 lib32ncurses5 lib32z1 lib32z1-dev
  sudo add-apt-repository ppa:maarten-fonville/android-studio
  sudo apt update
  sudo apt install android-studio

Nothing import
customer Setting
get everything
Change Sdk-Folder to 'storage/apps/Android/Sdk'
