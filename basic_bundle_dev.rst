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

Here is a sample template, with the blocks you will need to create::

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
  * vendors

If you need to call some javascript libraries, download them in the vendors directory.
But keep their directory structure if it's the case.

For example:

 * vendors (folder)
  * bootstrap (folder)
  * jquery.js
  * modernizr.js
  * respond.js

You have to add your bundle to the Assetic configuration, so that Assetic can parse your
javascripts and stylesheets tags (in app/config.yml)::

    assetic:
	    bundles: [“YournamespaceYourBundle”]

You also need to launch the assets:install command, so that Symfony2 copies your asset files in an accessible directory
(in this case, the web directory)::

    ./app/console assets:install --symlink web

2. Controller and templates
---------------------------

In the previous section (todo: add link), we let Symfony2 automatically generate a controller for us.

Now, you can start the generated controller. Usually, the first step is to have an index action in the default
controller, without any parameter, to serve as the starting point of the website or application.

You can achieve that result by modifying the indexAction method and the associated @Route annotation::

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
Simply add this line into your child template::

	{% extends "YournamespaceYourBundle::layout.html.twig" %}

If you want to print a block multiple times you can however use the block function.
Define some blocks in your layout.html.twig template::

	{% block title %}
	{% block body %}
	{% block stylesheets %}
	{% block javascripts %}

You will find more information about how to use the block function:

- `Twig blocks <http://twig.sensiolabs.org/doc/tags/extends.html>`_
