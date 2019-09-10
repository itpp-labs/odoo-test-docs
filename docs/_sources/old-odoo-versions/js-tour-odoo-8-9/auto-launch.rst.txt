================================
 Auto Launch after installation
================================

8.0, 9.0
--------

To run tour after module installation do next steps.

    * Create *ToDo*
    * Create *Action*


ToDo is some queued web actions that may call *Action* like this::

    <record id="base.open_menu" model="ir.actions.todo">
        <field name="action_id" ref="action_website_tutorial"/>
        <field name="state">open</field>
    </record>

Action is like this::

    <record id="res_partner_mails_count_tutorial" model="ir.actions.act_url">
        <field name="name">res_partner_mails_count Tutorial</field>
        <field name="url">/web#id=3&amp;model=res.partner&amp;/#tutorial_extra.mails_count_tour=true</field>
        <field name="target">self</field>
    </record>

Here tutorial_extra.**mails_count_tour** is tour id.

Use eval to compute some python code if needed::

    <field name="url" eval="'/web?debug=1&amp;res_partner_mails_count=tutorial#id='+str(ref('base.partner_root'))+'&amp;view_type=form&amp;model=res.partner&amp;/#tutorial_extra.mails_count_tour=true'"/>

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
