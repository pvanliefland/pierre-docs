Translations
============

This section explains the basics of how to use translations in Symfony2.

1. Enabling the translation Twig filter
---------------------------------------

In the app/config/config.yml file, remove the '#' before the line::

	translator:  { fallback: %locale% }


