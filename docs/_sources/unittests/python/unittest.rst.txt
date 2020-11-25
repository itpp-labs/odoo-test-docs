===============
 Odoo unittest
===============

.. contents::
   :local:


Test classes
============

Odoo 15.0+
----------

`Since Odoo 15 <https://github.com/odoo/odoo/pull/62031>`__, ``SavepointCase`` is replaced with updated ``TransactionCase``.

Complete `list of the classes <https://github.com/odoo/odoo/blob/master/odoo/tests/common.py>`__::


    class BaseCase(unittest.TestCase):
        """
        Subclass of TestCase for common OpenERP-specific code.
        
        This class is abstract and expects self.registry, self.cr and self.uid to be
        initialized by subclasses.
        """
    
    class TransactionCase(BaseCase):
        """ Test class in which all test methods are run in a single transaction,
        but each test method is run in a sub-transaction managed by a savepoint.
        The transaction's cursor is always closed without committing.
        The data setup common to all methods should be done in the class method
        `setUpClass`, so that it is done once for all test methods. This is useful
        for test cases containing fast tests but with significant database setup
        common to all cases (complex in-db test data).
        After being run, each test method cleans up the record cache and the
        registry cache. However, there is no cleanup of the registry models and
        fields. If a test modifies the registry (custom models and/or fields), it
        should prepare the necessary cleanup (`self.registry.reset_changes()`).
        """
    class SingleTransactionCase(BaseCase):
        """ TestCase in which all test methods are run in the same transaction,
        the transaction is started with the first test method and rolled back at
        the end of the last.
        """
    
    class HttpCase(TransactionCase):
        """ Transactional HTTP TestCase with url_open and phantomjs helpers.
        """

Odoo 14.0-
----------

From `odoo/tests/common.py <https://github.com/odoo/odoo/blob/14.0/odoo/tests/common.py>`_::

    class BaseCase(unittest.TestCase):
        """
        Subclass of TestCase for common OpenERP-specific code.
        
        This class is abstract and expects self.registry, self.cr and self.uid to be
        initialized by subclasses.
        """
    
    class TransactionCase(BaseCase):
        """ TestCase in which each test method is run in its own transaction,
        and with its own cursor. The transaction is rolled back and the cursor
        is closed after each test.
        """
    
    class SingleTransactionCase(BaseCase):
        """ TestCase in which all test methods are run in the same transaction,
        the transaction is started with the first test method and rolled back at
        the end of the last.
        """
    
    class SavepointCase(SingleTransactionCase):
        """ Similar to :class:`SingleTransactionCase` in that all test methods
        are run in a single transaction *but* each test case is run inside a
        rollbacked savepoint (sub-transaction).
    
        Useful for test cases containing fast tests but with significant database
        setup common to all cases (complex in-db test data): :meth:`~.setUpClass`
        can be used to generate db test data once, then all test cases use the
        same data without influencing one another but without having to recreate
        the test data either.
        """
    
    class HttpCase(TransactionCase):
        """ Transactional HTTP TestCase with url_open and phantomjs helpers.
        """

setUp and other methods
=======================

For more information see https://docs.python.org/2.7/library/unittest.html#test-cases

* ``setUp()`` -- Method called to prepare the test fixture. This is called immediately before calling the test method. It's recommended to use in ``TransactionCase`` and ``HttpCase`` classes
* ``setUpClass()`` -- A class method called before tests in an individual class run. setUpClass is called with the class as the only argument and must be decorated as a ``classmethod()``. It's recommended to use in ``SingleTransactionCase`` and ``SavepointCase`` classes

  .. code-block:: py

    @classmethod
    def setUpClass(cls):
        ...
* ``tearDown()``, ``tearDownClass`` -- are called *after* test(s). Usually are not used in odoo tests 

Assert Methods
==============

`Main methods <https://docs.python.org/3/library/unittest.html#assert-methods>`__:

+-----------------------------------------+-----------------------------+
| Method                                  | Checks that                 |
+=========================================+=============================+
| assertEqual(a, b)                       | ``a == b``                  |
+-----------------------------------------+-----------------------------+
| assertNotEqual(a, b)                    | ``a != b``                  |
+-----------------------------------------+-----------------------------+
| assertTrue(x)                           | ``bool(x) is True``         |
+-----------------------------------------+-----------------------------+
| assertFalse(x)                          | ``bool(x) is False``        |
+-----------------------------------------+-----------------------------+
| assertIs(a, b)                          | ``a is b``                  |
+-----------------------------------------+-----------------------------+
| assertIsNot(a, b)                       | ``a is not b``              |
+-----------------------------------------+-----------------------------+
| assertIsNone(x)                         | ``x is None``               |
+-----------------------------------------+-----------------------------+
| assertIsNotNone(x)                      | ``x is not None``           |
+-----------------------------------------+-----------------------------+
| assertIn(a, b)                          | ``a in b``                  |
+-----------------------------------------+-----------------------------+
| assertNotIn(a, b)                       | ``a not in b``              |
+-----------------------------------------+-----------------------------+
| assertIsInstance(a, b)                  | ``isinstance(a, b)``        |
+-----------------------------------------+-----------------------------+
| assertNotIsInstance(a, b)               | ``not isinstance(a, b)``    |
+-----------------------------------------+-----------------------------+

Also, to check error raising::

        with self.assertRaises(ValidationError):
            # some code that supposed to raise error
            ...
