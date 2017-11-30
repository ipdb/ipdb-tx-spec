Version
=======

The version indicates the transaction validation rules
to be used when validating the transaction,
i.e. the rules associated with that version
of the IPDB Transaction Spec.
It must be a string.

For example, if the value is ``"2.0"``,
then the transaction will be validated according
the :ref:`transaction validation rules <Transaction Validation>`
of version 2.0.0 of the IPDB Transaction Spec.

To indicate version 2, the only allowed value is ``"2.0"``
(not ``"2"`` or ``"2.0.0"``).
