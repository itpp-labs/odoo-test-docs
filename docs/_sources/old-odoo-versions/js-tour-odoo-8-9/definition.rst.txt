==================
 Tour Definition
==================

Example
~~~~~~~

.. code-block:: js

        {
            id: 'mails_count_tour',
            name: _t("Mails count Tour"),
            mode: 'test',
            path: '/web#id=3&model=res.partner',
            steps: [
                {
                    title:     _t("Mails count tutorial"),
                    content:   _t("Let's see how mails count work."),
                    popover:   { next: _t("Start Tutorial"), end: _t("Skip") },
                },
                {
                    title:     _t("New fields"),
                    content:   _t("Here is new fields with mails counters. Press one of it."),
                    element:   '.mails_to',
                },
                {
                    waitNot:   '.mails_to:visible',
                    title:     _t("Send message from here"),
                    placement: 'left',
                    content:   _t("Now you can see corresponding mails. You can send mail to this partner right from here. Press <em>'Send a mesage'</em>."),
                    element:   '.oe_mail_wall .oe_msg.oe_msg_composer_compact>div>.oe_compose_post',
                },
            ]
        }

Tour.register
~~~~~~~~~~~~~

In odoo 8 tour defines this way::

    (function () {
    'use strict';
    var _t = openerp._t;
    openerp.Tour.register({ ...

In odoo 9 tour defines that way::

    odoo.define('account.tour_bank_statement_reconciliation', function(require) {
    'use strict';
    var core = require('web.core');
    var Tour = require('web.Tour');
    var _t = core._t;
    Tour.register({ ...

Important details:

    * **id** - need to call this tour
    * **path** - from this path tour will be started in test mode

Step
~~~~

Next step occurs when **all** conditions are satisfied and popup window will appear near (chose position in *placement*) element specified in *element*. Element must contain css selector of corresponding node.
Conditions may be:

    * **waitFor** - this step will not start if *waitFor* node absent.
    * **waitNot** - this step will not start if *waitNot* node exists.
    * **wait** - just wait some amount of milliseconds before **next** step.
    * **element** - similar to *waitFor*,  but *element* must be visible
    * **closed window** - if popup window have close button it must be closed before next step.

Opened popup window (from previous step) will close automatically and new window (next step) will be shown.

Inject JS Tour file on page::

    <template id="res_partner_mails_count_assets_backend" name="res_partner_mails_count_assets_backend" inherit_id="web.assets_backend">
        <xpath expr="." position="inside">
            <script src="/res_partner_mails_count/static/src/js/res_partner_mails_count_tour.js" type="text/javascript"></script>
        </xpath>
    </template>

More documentation
~~~~~~~~~~~~~~~~~~

Some docs is here (begin from 10 slide):
http://www.slideshare.net/openobject/how-to-develop-automated-tests
Also checkout here:
https://github.com/odoo/odoo/blob/9.0/addons/web/static/src/js/tour.js