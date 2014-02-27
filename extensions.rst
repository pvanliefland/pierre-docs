Extensions
==========

This section explains the basics of how to implement and use a custom Twig extension.

1. Create the Extension Class
-----------------------------

Create the SiteExtention.php file following this folder structure::

	YourNamespace/YourBundle/Twig/Extension/SiteExtension/SiteExtension.php

Then define the namespace, and the class like the example below::

	namespace Pharco\SiteBundle\Twig\Extension;

	class SiteExtension extends \Twig_Extension
	{
    public function getName()
    {
        return 'site_extension';
    }

    public function getFilters()
    {
        return [
            new \Twig_SimpleFilter(‘yourfiltername', [$this, 'YourFunctionName'], array('is_safe' => array('html'))),
        ];
    }

    public function YourFunctionName($parameter)
    {
        return [];
    }

2. Register an Extension as a Service
-------------------------------------

In src/YourNamespace/YourBundle/config/services.yml file, add those lines::

	# TwigExtensions
   YourNamespace.twig.site_extension:
        class: YourNamespace\YourBundle\Twig\Extension\SiteExtension
        tags:
            - { name: twig.extension }

3. Using the custom Extension
-----------------------------

In your Twig template, use your custom filter like any other::

	{{ 'some text'|yourfiltername }}

See more documentation here:
- `Custom Twig Extension <http://symfony.com/doc/current/cookbook/templating/twig_extension.html>`_
