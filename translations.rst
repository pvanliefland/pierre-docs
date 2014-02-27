Translations
============

This section explains the basics of how to use translations in Symfony2.

1. Enabling the translation Twig filter
---------------------------------------

In the app/config/config.yml file, remove the '#' before the translator filter on line 7::

	framework
		translator:  { fallback: %locale% }


