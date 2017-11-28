Operation
=========

A string indicating what kind of transaction this is,
and how it should be validated.
The allowed values are ``"CREATE"``, ``"TRANSFER"`` and ``"GENESIS"``
(but there should only be one transaction whose operation is ``"GENESIS"``:
the one in the GENESIS block).

.. note::

   The ``"GENESIS"`` transaction might be deprecated in future versions
   of the IPDB Transaction Spec.