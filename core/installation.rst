Installation
============


Using composer
--------------

In your composer.json add the following line in the "require" block: ::

    "snowcap/core-bundle": "dev-master"

Then launch the following command: ::

    php composer.phar update snowcap/core-bundle

The final step is enabling the bundle: ::

    <?php
    // app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(
            // ...
            new Snowcap\CoreBundle\SnowcapCoreBundle(),
        );
    }
