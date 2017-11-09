Transaction Outputs
===================

A transaction output is a JSON object with a particular schema,
as outlined in this page and subsequent pages.
A transaction output must contain the following top-level JSON keys
(also called names or fields):

.. code-block:: json

   {
       "condition": {"<Condition object>"},
       "public_keys": ["<List of all public keys associated with the condition object>"],
       "amount": "<Number of shares of the asset (an integer in a string)>"
   }


The JSON Keys in a Transaction Output
-------------------------------------

**condition**

The :ref:`page about conditions <Conditions>` explains the contents
of a ``condition`` object.


**public_keys**

A list of public keys (strings) 
that should be consistent with the public keys
in the ``condition`` object.


**amount**

The number of shares of the asset associated with the output in question.
It's an integer converted to a string, e.g. ``"7"``.

In a TRANSFER transaction, the sum of the output amounts must be the same as the sum of the outputs that it transfers (i.e. the sum of the input amounts). For example, if a TRANSFER transaction has two outputs, one with ``"amount": "2"`` and one with ``"amount": "3"``, then the sum of the outputs is 5 and so the sum of the outputs-being-transferred must also be 5.
