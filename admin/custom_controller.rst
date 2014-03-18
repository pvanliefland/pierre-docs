Custom Admin Controller
=======================


1. Create your controller
-------------------------

Your controller must extend the ContentController:

.. code-block:: php

    <?php

    // AlbumController.php

    namespace Acme\AdminBundle\Controller;

    use Snowcap\AdminBundle\Controller\ContentController;
    use Snowcap\AdminBundle\Admin\ContentAdmin;

    class AlbumController extends ContentController
    {
        public function changeStatusAction(Request $request, ContentAdmin $admin)
        {
            $album = $this->getDoctrine()->getRepository('AcmeSiteBundle:Album')->find($request->query->get('id'));
            if(null === $album) {
                throw new NotFoundHttpException('No album with this id was found.');
            }

            $this->secure($admin, 'ADMIN_CONTENT_UPDATE', $album);

            // Do whatever you want there
        }
    }


2. Add your route for the admin
-------------------------------

You have to override the addRoutes() function in your admin:

.. code-block:: php

    <?php

    // AlbumAdmin.php

    namespace Acme\AdminBundle\Admin;

    use Snowcap\AdminBundle\Admin\ContentAdmin;
    use Snowcap\AdminBundle\Datalist\DatalistInterface;
    use Symfony\Component\Routing\RouteCollection;

    class AlbumAdmin extends ContentAdmin
    {
        // ...

        /**
         * @param RouteCollection $routeCollection
         */
        public function addRoutes(RouteCollection $routeCollection)
        {
            parent::addRoutes($routeCollection);

            // Add changeStatus route
            $routeCollection->add(
                $this->getRoutingHelper()->getRouteName($this, 'changeStatus'),
                $this->getRoutingHelper()->getRoute($this, 'changeStatus', array('id'))
            );
        }


        // You can even add a button directly in your datalist

        /**
         * @return DatalistInterface
         */
        public function getDatalist()
        {
            return $this->getDatalistFactory()
                ->createBuilder('base', array(
                        'admin' => $this,
                        'data_class' => $this->getEntityClass(),
                    )
                )
                // ... All your fields and filters here

                ->addAction(
                    'changeStatus',
                    'content_admin',
                    array(
                        'admin' => $this,
                        'action' => 'changeStatus',
                        'label' => 'datalist.change_status',
                        'icon' => 'pencil',
                    )
                )
                ->getDatalist();
        }
    }
