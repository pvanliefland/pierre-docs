Symfony Starter
================

1. Create your local working copy
---------------------------------

Go to your project directory and use composer to initialize your project::

    cd /path/to/your/www_directory
    composer create-project snowcap/symfony-starter-edition PROJECTNAME.local 2.3.x-dev

The create-project command will initialize a fresh new Symfony2 project, install the basic dependencies, and ask you
to complete interactively the parameters.yml file.

Usually, the defaults are ok, and you can just press enter to keep the default. The following parameters need to be
entered manually:

- The database name (the lowercase version of the project name is usually ok)
- The database user
- The database password
- Your sentry DSN (if you don't know what it is, can't find it, or can't create a new one, please ask your project
  manager or system administrator to help you)
- Answer yes when the prompt asks you to remove existing VCS history

2. Git setup
------------

Then, you can Initialize your working copy and remote repository::

    cd /path/to/your/www_directory/PROJECTNAME.local
    git init
    git remote add origin ssh://git.snowcap.be/var/git/PROJECTNAME.git


If you don't know which git repository to use, ask Edwin.

3. Generate your first bundle
-----------------------------

You can then create your first bundle::

    ./app/console generate:bundle


The generate:bundle command will prompt you to choose the following:

- The Bundle namespace (ProjectName/BundleName, e.a SomeNamespace/SomeBundle)
- Bundle name (keep the default)
- Target directory (keep the default)
- Configuration format (choose "annotation")
- Whether you want to generate the whole directory structure (choose "no")
- Confirmation (you can say "yes")
- Whether you want to update the Kernel automatically (say "yes")
- Whether you want to update the Routing automatically (say "yes")

In order to comply with our standard practice, you will need to perform 2 more steps:

In /path/to/your/project/directory/src/SomeNamespace/SomeBundle/Resources/config, rename services.xml to services.yml,
and clear the file content.

In /path/to/your/project/directory/src/SomeNamespace/SomeBundle/DependencyInjection/YourBundleNameExtension, replace::

    $loader = new Loader\XmlFileLoader($container, new FileLocator(__DIR__.'/../Resources/config'));
    $loader->load('services.xml');

by::

    $loader = new Loader\YamlFileLoader($container, new FileLocator(__DIR__.'/../Resources/config'));
    $loader->load('services.yml');

4. Check the result in your browser
-----------------------------------

You can then edit your host file in order to be able to access the website. On Mac OS, you can open the terminal and
type::

    sudo nano /etc/hosts

Add the following lines to the host file::

    127.0.0.1 PROJECTNAME.local

Normally, you can then access the newly created bundle in your browser::
    http://PROJECTNAME.local/hello/you

5. Commit and push
------------------

Assuming you are still in your project directory::

    git add .
    git commit -m "Project setup"
    git push origin master

