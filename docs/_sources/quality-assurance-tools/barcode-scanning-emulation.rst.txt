===================
 Emulation barcode
===================

Barcode scanner connected with computer work as keyboard. E.g. after scanning send sequence of symbols as if fast typing on the keyboard.

Emulation via OS
================

Install **xdotool** app if you haven't it yet.

.. code-block:: shell

    sudo apt-get install xdotool

Emulation scanning barcode:

.. code-block:: shell

    sleep 3 && xdotool type 1234567890128 &

or so:

.. code-block:: shell

    sleep 3 && xdotool type 3333333333338 &

Where: 3 - sleep seconds; 3333333333338 - barcode.

After successfully scanning you will see '3333333333338' in the command line. If toggle to other window that symbols appear in the input field in the this window. So we can send sequence in the app as if we scanning it.

Emulation via browser
=====================

Open browser console (e.g. via F12 button) and type:

.. code-block:: js

    odoo.__DEBUG__.services['web.core'].bus.trigger('barcode_scanned', '1234567890128', $('.web_client')[0])
