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

	composer.phar update snowcap/admin-bundle

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

.. code-block:: console

	./app/console generate:bundle

Your bundle must extend SnowcapAdminBundle in order for it to work.

.. code-block:: php

	<?php
	// src/Yournamespace/AdminBundle/YournamespaceAdminBundle.php

	public function getParent()
	{
	    return 'SnowcapAdminBundle';
	}

4. Enable admin routing
-----------------------

.. code-block:: yaml

	# app/config/routing.yml

	snowcap_admin:
	    resource: "@SnowcapAdminBundle/Resources/config/routing.yml"
	    prefix: /admin

5. Configure Assetic
--------------------

SnowcapAdminBundle uses assetic in order to speed up the display of the admin pages. You must add SnowcapAdminBundle to the list of configured assetic bundles. Additionally, cssrewrite filter must be enabled for the AdminBundle to work.

.. code-block:: yaml

	# app/config/config.yml

	assetic:
	    debug: "%kernel.debug%"
	    use_controller: false
	    bundles: ["YournamespaceAdminBundle", "SnowcapAdminBundle"]
	    filters:
	        cssrewrite: ~

6. Configure security
---------------------

The AdminBundle requires at least an active firewall.

You can use whichever authentication mechanism you like. In order to make your life easier, SnowcapAdminBundle provides a base user class, and a few other extras to be used with Doctrine's entity user provider and standard login form authentication.

First, create a user class in your AdminBundle's entity directory:

.. code-block:: php

	<?php
	// src/Yournamespace/AdminBundle/Entity/AdminUser.php

	namespace Yournamespace\AdminBundle\Entity;

	use Doctrine\ORM\Mapping as ORM;

	use Snowcap\AdminBundle\Entity\User;

	/**
	 * @ORM\Entity
	 * @ORM\Table
	 */
	class AdminUser extends User
	{

	}

You can then change your security.yml config file:

.. code-block:: yaml

	# app/config/security.yml

	snowcap_admin:
	    security:
	        user_class: Yournamespace\AdminBundle\Entity\AdminUser

	security:
	    encoders:
	        Snowcap\AdminBundle\Entity\User: sha512

	    providers:
	        admin_users:
	            entity: { class: YournamespaceAdminBundle:AdminUser, property: username }

	    firewalls:
	        ...

	        admin:
	            pattern:    ^/admin
	            provider: admin_users
	            anonymous: ~
	            form_login:
	                login_path:  snowcap_admin_login
	                check_path:  snowcap_admin_login_check
	            logout:
	                path: snowcap_admin_logout

	    access_control:
	        - { path: ^/admin/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
	        - { path: ^/admin, role: ROLE_ADMIN }

Don't forget to update your database schema, using schema:update or migrations:diff / migrations:migrate:

.. code-block:: console

	./app/console doctrine:schema:update --force

When this is done, you can create admin users through the command line:

.. code-block:: console

	./app/console snowcap:admin:generate:user

Follow the instructions to generate the login, email, password and role (ROLE_ADMIN)

7. Additional configuration steps
---------------------------------

.. code-block:: yaml

	# app/config/config.yml

	snowcap_admin:
	    default_translation_domain: admin


