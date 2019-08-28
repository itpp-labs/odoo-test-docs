=========
 JS Tour
=========


Tour is a set of steps of possible scenario of module usage. 

Steps may be executed automatically for :doc:`testing <../qa/js/index>` purpose or by user for :ref:`demostrating <auto_launch_after_installation>` purpose.

.. contents::
   :local:

Tour Definition
===============

10.0+
-----
Example
~~~~~~~
Example from `website_sale <https://github.com/odoo/odoo/blob/10.0/addons/website_sale/static/src/js/website_sale_tour_buy.js>`_ module:

.. code-block:: js

    odoo.define('website_sale.tour', function (require) {
    'use strict';
    
    var tour = require("web_tour.tour");
    var base = require("web_editor.base");
    
    var options = {
        test: true,
        url: '/shop',
        wait_for: base.ready()
    };

    var tour_name = 'shop_buy_product';
    tour.register(tour_name, options,
        [
            {
                content: "search ipod",
                trigger: 'form input[name="search"]',
                run: "text ipod",
            },
            {
                content: "search ipod",
                trigger: 'form:has(input[name="search"]) .oe_search_button',
            },
            {
                content: "select ipod",
                trigger: '.oe_product_cart a:contains("iPod")',
            },
            {
                content: "select ipod 32GB",
                extra_trigger: '#product_detail',
                trigger: 'label:contains(32 GB) input',
            },
            {
                content: "click on add to cart",
                extra_trigger: 'label:contains(32 GB) input:propChecked',
                trigger: '#product_detail form[action^="/shop/cart/update"] .btn',
            },
            /* ... */
        ]
    );
    
    });


Options
~~~~~~~

Options (second argument of ``tour.register``):

* **test** -- only for tests
* **url** -- open link before running the tour
* **wait_for** -- wait for deffered object before running the script
* **skip_enabled** -- adds *Skip* button in tips

Step
~~~~

Each step may have following attrubutes:

* **content** -- name or title of the step
* **trigger** (mandatory) -- where to place tip. *In js tests: where to click*
* **extra_trigger** -- when this becomes visible, the tip is appeared. *In js tests: when to click*
* **timeout** -- max time to wait for conditions
* **position** -- how to show tip (left, rigth, top, bottom), default right
* **width** -- width in px of the tip when opened, default 270
* **edition** -- specify to execute in *"community"* or in *"enterprise"* only. By default empty -- execute at any edition.
* **run** -- what to do when tour runs automatically (e.g. in tests)

  * ``'text SOMETEXT'`` -- writes value in **trigger** element
  * ``'click'``
  * ``'drag_and_drop TO_SELECTOR'``
  * ``'auto'`` -- auto action (click or text)
  * ``function: (actions) { ... }`` -- actions is instance of RunningTourActionHelper -- see `tour_manager.js <https://github.com/odoo/odoo/blob/10.0/addons/web_tour/static/src/js/tour_manager.js>`_ for its methods.
* **auto** -- step is skipped in non-auto running

Predefined steps
~~~~~~~~~~~~~~~~

* ``tour.STEPS.MENU_MORE`` -- clicks on menu *More* in backend when visible
* ``tour.STEPS.TOGGLE_APPSWITCHER`` -- nagivate to Apps page when running in enterprise
* ``tour.STEPS.WEBSITE_NEW_PAGE`` -- clicks create new page button in frontend

More documentation
~~~~~~~~~~~~~~~~~~

* https://www.odoo.com/slides/slide/the-new-way-to-develop-automated-tests-beautiful-tours-440
* https://github.com/odoo/odoo/blob/10.0/addons/web_tour/static/src/js/tour_manager.js
* https://github.com/odoo/odoo/blob/10.0/addons/web_tour/static/src/js/tip.js


Open backend menu
=================

11.0+
-----

`No additional actions are required. <https://github.com/odoo/odoo/commit/7e008469e4e5afe9b4c7151a4738240462359f98>`__


Manual launching
================

10.0+
-----

* :doc:`activate developer mode <https://odoo-development.readthedocs.io/en/latest/odoo/usage/debug-mode.html?highlight=developer%20mode>`.
* Click *Bug* icon (between chat *icon* and *Username* at top right-hand corner)

  * click ``Start tour``

* Click *Play* button -- it starts tour in auto mode

To run *test-only* tours (or to run tours in auto mode but with some delay) do as following:

* open browser console (F12 in Chrome)
* Type in console:

  .. code-block:: js

    odoo.__DEBUG__.services['web_tour.tour'].run('TOUR_NAME', 1000); // 1000 is delay in ms before auto action


Auto Launch after installation
==============================

9.0, 10.0
---------

Some additional actions are required to work with backend menus in tours

Manifest
~~~~~~~~

Add ``web_tour`` to dependencies

.. code-block:: py

    "depends": [
        "web_tour",
    ],
    # ...
    "demo": [
        "views/assets_demo.xml",
        "views/tour_views.xml",
    ],


load_xmlid
~~~~~~~~~~

You need to set ``load_xmlid`` for *each* menu you need to open. Recommended
name for the file is ``tour_views.xml``

.. code-block:: xml

    <?xml version="1.0" encoding="utf-8"?>
    <odoo>
        <!-- Make the xmlid of menus required by the tour available in webclient -->
        <record id="base.menu_administration" model="ir.ui.menu">
            <field name="load_xmlid" eval="True"/>
        </record>
    </odoo>

Tour
~~~~

Use *trigger* selector for both editions:

.. code-block:: js


    {
        trigger: '.o_app[data-menu-xmlid="base.menu_administration"], .oe_menu_toggler[data-menu-xmlid="base.menu_administration"]',
        content: _t("Configuration options are available in the Settings app."),
        position: "bottom"
    }


10.0+
-----

TODO
