Getting a working copy
======================

1. Clone the project
--------------------

.. code-block:: console

    cd ~Sites
    git clone ssh://git.snowcap.be/var/git/PROJECTNAME.git PROJECTNAME.local

2. Install vendors
------------------

Log in into your vagrant instance, and load vendors by

.. code-block:: console

    cd /var/www/PROJECTNAME.git
    composer install

3. Assets
---------

.. code-block:: console

    app/console assets:install --symlink web
    app/console assetic:dump


4. Database setup
-----------------

Create the database

.. code-block:: console

    app/console d:database:create

If your project runs under Doctrine migrations, you can launch

.. code-block:: console

    app/console d:migrations:migrate

Otherwise, update the schema by introspection

.. code-block:: console

    app/console d:schema:update --force

And if the project has fixtures

.. code-block:: console

    app/console d:fixtures:load


4. Check the result in your browser
-----------------------------------

You can then edit your host file in order to be able to access the website. On Mac OS, you can open the terminal and
type:

.. code-block:: console

    sudo nano /etc/hosts

Add the following lines to the host file:

.. code-block:: console

    127.0.0.1 PROJECTNAME.local

Normally, you can then access the newly created bundle in your browser::
    http://PROJECTNAME.local/
