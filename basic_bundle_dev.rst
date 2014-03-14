Basic bundle development
========================

This section explains the basics of how to work with Models, Views & Controllers in Symfony2.

The goal here is not to take the place of the official Symfony2 documentation, but rather
- to help you put the pieces together
- to document the coding standard and practices

Before you dive into the next sections, please make sure you have read and understood the two following articles in
the official Symfony2 documentation:

- `The Big Picture <http://symfony.com/doc/current/quick_tour/the_big_picture.html>`_
- `Symfony2 and HTTP Fundamentals <http://symfony.com/doc/current/book/http_fundamentals.html>`_
- `Symfony2 versus Flat PHP <http://symfony.com/doc/current/book/from_flat_php_to_symfony2.html>`_
- `Creating Pages in Symfony2 <http://symfony.com/doc/current/book/page_creation.html>`_

1. Creation of a base layout template
-------------------------------------

If you are not already familiar with Twig, go ahead and read the following resources:

- `Creating and using Templates <http://symfony.com/doc/current/book/templating.html>`_
- `Twig for Template Designers <http://twig.sensiolabs.org/doc/templates.html>`_

Our symfony-starter edition provides a slightly modified version of the defautl Symfony2 base.html.twig template. You
will find it in app/Resources/views.

Your first step will be to create a base layout template. You can create it in the
src/YourNamespace/YourBundle/Resources/views directory. Our convention is to call it layout.html.twig.

Here is a sample template, with the blocks you will need to create:

.. code-block:: jinja

    {% extends "::base.html.twig" %}

    {% block stylesheets %}
        {% stylesheets
        'bundles/yourbundle/css/screen.css'
        filter='cssrewrite,?yui_css'
        output='cache/assetic/css/all.css' %}
        <link href="{{ asset_url }}" rel="stylesheet" type="text/css" media="screen"/>
        {% endstylesheets %}
        {% stylesheets
        'bundles/yourbundle/css/ie.css'
        filter='cssrewrite,?yui_css'
        output='cache/assetic/css/ie.css' %}
        <!--[if IE]>
        <link href="{{ asset_url }}" rel="stylesheet" type="text/css" media="screen"/>
        <![endif]-->
        {% endstylesheets %}
    {% endblock %}

    {% block head_javascripts %}
        {% javascripts
        'bundles/yourbundle/vendors/modernizr.js'
        filter='?yui_js'
        output='cache/assetic/js/modernizr.js' %}
        <script type="text/javascript" src="{{ asset_url }}"></script>
        {% endjavascripts %}
        <!--[if lte IE 8 ]>
        {% javascripts
        'bundles/yourbundle/vendors/respond.js'
        filter='?yui_js'
        output='cache/assetic/js/respond.js' %}
        <script type="text/javascript" src="{{ asset_url }}"></script>
        {% endjavascripts %}
        <![endif]-->
    {% endblock %}

    {% block javascripts %}
        {% javascripts
        'bundles/yourbundle/vendor/jquery.js'
        'bundles/yourbundle/js/screen.js'
        filter='?yui_js'
        output='cache/assetic/js/all.js' %}
        <script type="text/javascript" src="{{ asset_url }}"></script>
        {% endjavascripts %}
    {% endblock %}

    {% block body %}

        {# Place your layout code in here #}

    {% endblock body %}

2. Assets setup
---------------

As you can see in the example above, asset files (js and css) are located in a "public" directory in your bundle
Resources folder. You will need to create that directory yourself.

We use the following structure:

* Resources

 * Public

  * css
  * images
  * less
  * fonts
  * js
  * vendor

If you need to call some javascript libraries, download them in the vendors directory.
But keep their directory structure if it's the case.

For example:

 * vendor (folder)

  * bootstrap (folder)
  * jquery.js
  * modernizr.js
  * respond.js

You can download some interesting libraries here:

- `Jquery <http://jquery.com/download/>`_

- `Jquery UI <http://jqueryui.com/download/>`_

- `Bootstrap <http://getbootstrap.com/getting-started/#download>`_

- `Modernizr <http://modernizr.com/download/>`_

- `Respond <https://github.com/scottjehl/Respond>`_ (fix for min/max-width CSS3 Media Queries for IE 6-8, and more)


You have to add your bundle to the Assetic configuration, so that Assetic can parse your
javascripts and stylesheets tags (in app/config.yml):

.. code-block:: yaml

    assetic:
	    bundles: ["YournamespaceYourBundle"]

You also need to launch the assets:install command, so that Symfony2 copies your asset files in an accessible directory
(in this case, the web directory)::

    ./app/console assets:install --symlink web

3. Controller and templates
---------------------------

In the previous section (todo: add link), we let Symfony2 automatically generate a controller for us.

Now, you can start the generated controller. Usually, the first step is to have an index action in the default
controller, without any parameter, to serve as the starting point of the website or application.

You can achieve that result by modifying the indexAction method and the associated @Route annotation:

.. code-block:: php

    <?php
    /**
     * @Route("")
     * @Template()
     */
    public function indexAction($name)
    {
        return [];
    }

You will find more information about how to use route and controllers in the following articles:

- `Controller <http://symfony.com/doc/current/book/controller.html>`_
- `Routing <http://symfony.com/doc/current/book/routing.html>`_

Note : The controller renders the YournamespaceYourBundle:Default:index.html.twig template,
which uses the following naming convention::

	BundleName:ControllerName:TemplateName

This is the logical name of the template, which is mapped to a physical location using the following convention::

	/path/to/YournamespaceYourBundle/Resources/views/ControllerName/TemplateName

Now, you probably would extend the layout.html.twig template into child templates.
The extends tag should be the first tag in the template.
Simply add this line into your child template:

.. code-block:: jinja

	{% extends "YournamespaceYourBundle::layout.html.twig" %}

If you want to print a block multiple times you can however use the block function.
Define some blocks in your layout.html.twig template:

.. code-block:: jinja

	{% block title %}
	{% block body %}
	{% block stylesheets %}
	{% block javascripts %}

You will find more information about how to use the block function:

- `Twig blocks <http://twig.sensiolabs.org/doc/tags/extends.html>`_

4. Coding standards
-------------------

* PSR-0 is a standard that has been adopted by some frameworks like Zend Framework 2 to facilitate the autoloading process across platforms.
  This is accomplished by following namespace and class naming standards that correspond to the relative location of the resource or file in question.

If you are not already familiar with PHP Specification Request, go ahead and read the following resource:

-`PSR-0 <https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md>`_

* CamelCase for Symfony2 entity:

When contributing code to Symfony2, you must follow its coding standards. Symfony follows the standards defined in the PSR-0, PSR-1 and PSR-2 documents.

In short :

 * Use camelCase, not underscores, for variable, function and method names, arguments;
 * Use underscores for option names and parameter names;
 * Use namespaces for all classes;
 * Prefix abstract classes with Abstract. Please note some early Symfony2 classes do not follow this convention and have not been renamed for backward compatibility reasons. However all new abstract classes must follow this naming convention;
 * Suffix interfaces with Interface;
 * Suffix traits with Trait;
 * Suffix exceptions with Exception;
 * Use alphanumeric characters and underscores for file names;
 * Don't forget to look at the more verbose Conventions document for more subjective naming considerations.

If you need more documentation, read those articles below:

-`Coding Standards <http://symfony.com/doc/current/contributing/code/standards.html>`__

-`Conventions <http://symfony.com/doc/current/contributing/code/conventions.html>`_

* Twig coding standards

Twig is the default template engine of Symfony2. Twig uses a syntax similar to the Django and Jinja template languages which inspired the Twig runtime environment.

Filters, variables and functions names are in lowercase and can be more readable by using underscore to separate parts of their name.

Example:

.. code-block:: jinja

	{% set foo_bar = 'foo' %}

	{{ a_variable }}

	{{ 'some example text'|custom_filter(some_parameters) }}

	{{ render_my_function(some_parameters) }}


If you want to learn more about how to use Twig synthax and semantic, please visit those pages:

-`Twig for Template Designer <http://twig.sensiolabs.org/doc/templates.html>`_

-`Coding Standards <http://twig.sensiolabs.org/doc/coding_standards.html>`__

-`Extending Twig <http://twig.sensiolabs.org/doc/advanced.html>`_


