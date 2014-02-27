Translations
============

This section explains the basics of how to use translations in Symfony2.

1. Enabling the translation Twig filter
---------------------------------------

In the app/config/config.yml file, remove the '#' before the translator filter on line 7::

	framework
		translator:  { fallback: %locale% }



2. Create translation files
---------------------------

In src/YourNamespace/YourBundle/Resources/ create the 'translations' directory
For each language, create a messages.'thechosenlanguage'.yml


Now you can structure the yml file as you will. Keep in mind to make it simple, don't use to many sub-levels.
We recommend you to use only 3 levels.

For example::

	sitename: Your site name

	meta:
	    home:
	        title: Home
	    default:
	        title: Your campaign name
	        description: ...

In your twig template, simply call the translated message this way::

	{{ 'meta.home.title'|trans }}

