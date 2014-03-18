Installation
============


1. Download SnowcapAdminBundle using composer
---------------------------------------------

Add SnowcapAdminBundle in your composer.json:

.. code-block:: yaml

	{
	    "require": {
	        "snowcap/admin-bundle": "dev-master"
	    }
	}

Now tell composer to download the bundle by running the command:

.. code-block:: console

	$ php composer.phar update snowcap/admin-bundle

Composer will install the bundle to your project's vendor/snowcap directory.

2. Enable the bundle
--------------------

Enable the bundle in the kernel:

.. code-block:: php

	<?php
	// app/AppKernel.php

	public function registerBundles()
	{
	    $bundles = array(
	        // ...
	        new Snowcap\AdminBundle\SnowcapAdminBundle(),
	        // Required dependencies
	        new Snowcap\CoreBundle\SnowcapCoreBundle(),
	        new Snowcap\BootstrapBundle\SnowcapBootstrapBundle(),
	    );
	}

3. Create your Admin Bundle
---------------------------

In order to be able to use the Snowcap Admin Bundle, you need to create your own Admin Bundle in your project.

.. code-block:: php

	$ php ./app/console generate:bundle

