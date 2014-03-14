Sitemap
=======


Enable routing
--------------

In app/config/routing.yml add the following at the end of the file: ::

    # CoreBundle routing configuration
    snowcap_core_sitemap:
        resource: "@SnowcapCoreBundle/Resources/config/routing_sitemap.yml"



Create the service
------------------

Then create a file that extends Snowcap\\CoreBundle\\Sitemap\\AbstractSitemap: ::

    <?php
    // Acme/SiteBundle/Sitemap/Sitemap.php

    namespace Acme\SiteBundle\Sitemap;

    use Snowcap\CoreBundle\Sitemap\AbstractSitemap;
    use Doctrine\ORM\EntityManager;
    use Symfony\Component\Routing\Router;

    class Sitemap extends AbstractSitemap
    {
        /**
         * @var \Doctrine\ORM\EntityManager
         */
        private $em;

        /**
         * @var string
         */
        private $locale;

        /**
         * @param EntityManager $em
         * @param string $locale
         */
        public function __construct(EntityManager $em, $locale) {
            $this->em = $em;
            $this->locale = $locale;
        }

        /**
         * @param Router $router
         * @return mixed|void
         */
        public function build(Router $router)
        {
            // Homepage
            $this->addUrl($router->generate('acme_site_default_index', ['_locale' => $this->locale], true));

            // Pages
            $pages = $this->em->getRepository('AcmeSiteBundle:Page')->findAllPublished($this->locale);
            foreach($pages as $page) {
                $pageSlug = $page->getTranslations()->get($this->locale)->getSlug();
                $loc = $router->generate('acme_site_page_view', ['slug' => $pageSlug, '_locale' => $this->locale], true);
                $this->addUrl($loc, null, self::CHANGEFREQ_MONTHLY);
            }
        }
    }



Register the service(s)
-----------------------

Finally all you need is to populate your services.yml file with one or more sitemap services like this: ::

    # Acme/SiteBundle/Resources/config/services.yml
    services:
        # ...

        # Sitemaps
        acme_site.sitemap_fr:
            class: Acme\SiteBundle\Sitemap\Sitemap
            arguments: [@doctrine.orm.entity_manager, "fr"]
            tags:
                - { name: snowcap_core.sitemap, alias: fr }
        acme_site.sitemap_en:
            class: Acme\SiteBundle\Sitemap\Sitemap
            arguments: [@doctrine.orm.entity_manager, "en"]
            tags:
                - { name: snowcap_core.sitemap, alias: en }



Now your main sitemap is available at http://yourhost/sitemap.xml.
If you defined several with aliases then the main sitemap will list all of them and with the example we would have:

* sitemap.xml
* sitemap_fr.xml
* sitemap_en.xml
