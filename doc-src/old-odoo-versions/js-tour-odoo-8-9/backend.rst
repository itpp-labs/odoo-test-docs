===================
 Open backend menu
===================

8.0
---

The only way to open menu is search by string, for example

.. code-block:: js

    {
        title:     "go to accounting",
        element:   '.oe_menu_toggler:contains("Accounting"):visible',
    },

9.0
---

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