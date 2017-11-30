Operation
=========

The operation indicates the type/kind of transaction,
and how it should be validated.
It must be a string.
The allowed values are ``"CREATE"`` and ``"TRANSFER"``.

.. note::

   Some implementations may allow other values,
   but maybe only internally.
   For example, BigchainDB Server allows the value ``"GENESIS"``.
   See :ref:`the page about implementation-specific deviations
   <Implementation-Specific Deviations>`.
