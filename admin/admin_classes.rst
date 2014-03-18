Your first admin class
======================

Creating a Content Admin can be done in 2 steps:
1. Create a Content Admin class
2. Register your admin class with the Service Container

1. Create a Content Admin class
-------------------------------

The first step is to create an Admin class that extends the abstract ContentAdmin class. You will have to implement at least four methods:

1. *getForm* must return a Symfony/Component/Form/FormInterface instance
2. *getDatalist* must return a Snowcap/AdminBundle/Datalist/DatalistInterface instance
3. *getEntityName* receives an entity as sole argument and must return a textual representation of that entity (its name or its title for instance)
4. *getEntityClass* must return the fully qualified class name of the managed entity

Step by step, we are going to generate a Page entity

.. code-block:: php

	<?php
	// src/Yournamespace/AdminBundle/Admin/PageAdmin.php

	namespace Yournamespace\AdminBundle\Admin;

	class PageAdmin extends ContentAdmin
	{
		/**
	     * Return the main admin list for this content
	     *
	     * @return \Snowcap\AdminBundle\Datalist\DatalistInterface
	     */
		public function getDatalist()
	    {
	        return $this->getDatalistFactory()
	            ->createBuilder('yournamespace_admin_content', array('admin' => $this))
	            ->addField('adminTitle', 'text', array('label' => 'page.datalist.title'))
	            ->addField('publishedOn', 'datetime', array('label' => 'page.datalist.published_on'))
	            ->getDatalist();
	    }
	    /**
	     * @return string
	     */
	    public function getEntityClass()
	    {
	        return 'Yournamespace\YourBundle\Entity\Page';
	    }

	    /**
	     * Return the main admin form for this content
	     *
	     * @return \Symfony\Component\Form\Form
	     */
	    public function getForm()
	    {
	        return $this->getFormFactory()->create(new PageType());
	    }

	    /**
	     * @param mixed $entity
	     *
	     * @return string
	     */
	    public function getEntityName($entity)
	    {
	        return $entity->getAdminTitle();
	    }
	}

2. Create a Form type
---------------------

Now, you have to build the form to save your data. Be sure to follow the entity structure.

.. code-block:: php

	<?php
	// src/Yournamespace/AdminBundle/Form/Type/PageType.php

	namespace Isostar\AdminBundle\Form\Type;

	use Yournamespace\YourBundle\Entity\Page;
	use Symfony\Component\Form\AbstractType;
	use Symfony\Component\Form\FormBuilderInterface;
	use Symfony\Component\OptionsResolver\OptionsResolverInterface;

	class PageType extends AbstractType
	{

	}


