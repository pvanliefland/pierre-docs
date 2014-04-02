Install Less Compiler on your Vagrant
=====================================

1. Install NPM if not already done
----------------------------------

On your Vagrant:

.. code-block:: console

    sudo su
    echo "deb http://ftp.us.debian.org/debian wheezy-backports main" >> /etc/apt/sources.list
    apt-get update
    apt-get install nodejs-legacy
    curl --insecure https://www.npmjs.org/install.sh | bash



2. Install Less
---------------

Still in your Vagrant:

.. code-block:: console

	npm install -g less


Now you can check if the Less compiler was correctly installed:

.. code-block:: console

	lessc

