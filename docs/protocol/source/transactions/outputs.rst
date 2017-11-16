Outputs
=======

A list/array/tuple of transaction outputs.

Each output indicates the crypto-conditions which must be satisfied
by anyone wishing to spend/transfer that output.
It also indicates the number of shares of the asset tied to that output.

An output can be implemented as an :term:`associative array`
in almost any programming language (e.g. as a dictionary in Python).
It has the following basic structure:

.. code-block:: bash

   {
      "condition": condition,
      "public_keys": ["<List of all public keys associated with the condition object>"],
      "amount": "<Number of shares of the asset (an integer in a string)>"
   }


The Keys in a Transaction Output
--------------------------------

**condition**

The :ref:`page about conditions <Conditions>` explains the contents of a condition.


**public_keys**

A list of public keys (strings) that should be consistent with the public keys
in the ``condition``.


**amount**

The number of shares of the asset associated with the output in question.
It's an integer converted to a string, e.g. ``"7"``.

In a TRANSFER transaction, the sum of the output amounts must be the same
as the sum of the outputs that it transfers (i.e. the sum of the input amounts).
For example, if a TRANSFER transaction has two outputs,
one with ``"amount": "2"`` and one with ``"amount": "3"``,
then the sum of the outputs is 5 and so
the sum of the outputs-being-transferred must also be 5.
