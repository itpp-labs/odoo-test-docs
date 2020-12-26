=====================================
 How js tour works in Odoo unittests
=====================================

The order is as following:

* OPEN *url_path* from **python** :doc:`phantom_js <phantom_js>` method
* WAIT *ready* condition (Truthy or Falsy) from **python** :doc:`phantom_js <phantom_js>` method
* OPEN *url* from :doc:`tour <../../js-tour>`'s options in **js** file
* WAIT *wait_for* (deferred object) from :doc:`tour <../../js-tour>`'s options in **js** file
* DO first step from **js** :doc:`tour <../../js-tour>`

  * WAIT when *trigger* becomes visible
  * WAIT when *extra_trigger*  becomes visible (if *extra_trigger* is presented)
  * EXECUTE action (*run* or click on *trigger*)

* DO NEXT step

  * ...

* STOP Running when:

  * error happens:

    * thrown via ``raise``
    * reported via ``console.error(...)``
    * reported by tour system on **timeout** for initial *ready* condition. `Timeout value is 60 sec <https://github.com/odoo/odoo/blob/5aa540091f3f301960aa5a07fba2a6d009f09624/odoo/tests/common.py#L753>`__ and `it cannot be changed <https://github.com/odoo/odoo/blob/5aa540091f3f301960aa5a07fba2a6d009f09624/odoo/tests/common.py#L985>`__.
    * reported by tour system on step **timeout**.
    * `Odoo 12 and below <https://github.com/odoo/odoo/commit/fec86404e7de3b22b4945812d525d4017d254c33>`__: reported via ``console.log('error', ...)``

  * ``'test successful'`` is reported via ``console.log`` (`in Odoo 12 and below <https://github.com/odoo/odoo/commit/fec86404e7de3b22b4945812d525d4017d254c33>`__ it was just ``'ok``)

    * directly by code 
    * indirectly by tour system when all steps are done

  * **timeout** from **python** :doc:`phantom_js <phantom_js>` method is occured. Default is 60 sec
  
