Version
=======

The value of ``"version"`` indicates how the transaction
should be validated.
It must be a string.

If the value is ``"2.0"``,
then the transaction will be validated according
the :ref:`transaction validation rules <Transaction Validation>`
of version 2.0.0 of the IPDB Transaction Spec.

To indicate version 2, the only allowed value is ``"2.0"``
(not ``"2"`` or ``"2.0.0"``).
