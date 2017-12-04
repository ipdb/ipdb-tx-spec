Transaction Validation
======================

If a transaction satisfies the constraints (or rules) listed below,
then it is considered valid.
The process of checking those constraints is called transaction validation.

Each version X.Y.Z of the IPDB Transaction Spec may have different constraints.
That is, the constraints may change from one version to the next.


How Validation Code Decides Which Version to Use
------------------------------------------------

When given a transaction to validate,
validation code should check the value of ``"version"``
inside the transaction.
The valid values are those with a corresponding set of JSON Schema files
(which can be found in the ``tx_schema`` directory
of the `IPDB Transaction Spec repository
<https://github.com/ipdb/ipdb-tx-spec>`_).
If ``"version"`` doesn't have a valid value, then the transaction is invalid.
Otherwise, the transaction should be validated according
to the validation constraints described in that version of the IPDB Transaction Spec.


JSON Schema Validation
----------------------

JSON Schema Validation is done by checking the transaction against
a formal `JSON Schema  <http://json-schema.org/>`_
defined in a set of `JSON Schema <http://json-schema.org/>`_ files.
Those files can be found
in the ``tx_schema/`` directory
of the `IPDB Transaction Spec repository
<https://github.com/ipdb/ipdb-tx-spec>`_.

.. note::

   Tip 1: There's a nice explanation of JSON Schema in the website
   `"Understanding JSON Schema"
   <https://spacetelescope.github.io/understanding-json-schema/index.html>`_.

   Tip 2: Python 3 code for checking a transaction against JSON Schema files
   can be found in the `source code of BigchainDB Server
   <https://github.com/bigchaindb/bigchaindb>`_.
   At the time of writing, it was in the file
   ``bigchaindb/common/schema/__init__.py``.


Other Constraints
-----------------

The output.amount Rule
^^^^^^^^^^^^^^^^^^^^^^

For all outputs, once output.amount has been converted
from a string to an integer,
it must be between 1 and 9×10^18, inclusive.
The reason for the upper bound is to keep amount within what a server
can comfortably represent using a 64-bit signed integer,
i.e. 9×10^18 is less than 2^63.

The Duplicate Transaction Rule
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If a transaction is a duplicate of a previous transaction,
then it's invalid.
A quick way to check that is by checking
to see if a transaction with the same transaction ID
is already stored.

The Transaction ID Rule
^^^^^^^^^^^^^^^^^^^^^^^

The transaction ID (id) must be valid in that it must agree
with the hash computed using the instructions given
on :ref:`the page about transaction ID <Transaction ID>`.

The TRANSFER Transaction Rules
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If a transaction is a TRANSFER transaction:

#. If an input attempts to fulfill an output
   that has already been fulfilled (i.e. spent or transferred)
   by a previous valid transaction,
   then the transaction is invalid.
   (You don't have to check if the fulfillment string is valid.)
#. If two or more inputs
   (in the transaction being validated)
   attempt to fulfill the *same* output,
   then the transaction is invalid.
   (You don't have to check any fulfillment strings.)
#. The sum of the amounts on the inputs must equal
   the sum of the amounts on the outputs.
   In other words, a TRANSFER transaction can't create or destroy asset shares.
#. For all inputs, if input.fulfills points to:
    - a transaction that doesn't exist, then it's invalid.
    - a transaction that's invalid, then it's invalid.
      (This check may be skipped if invalid transactions are never kept.)
    - a transaction output that doesn't exist, then it's invalid.
    - a transaction with an asset ID that's different
      from *this* transaction's asset ID, then this transaction is invalid.
      (The asset ID of a CREATE transaction is the same as the transaction ID.
      The asset ID of a TRANSFER transaction is asset.id.)

Note: The first two rules prevent double spending.

The input.fulfillment Rule
^^^^^^^^^^^^^^^^^^^^^^^^^^

Regardless of whether the transaction is a CREATE or TRANSFER transaction:
For all inputs, input.fulfillment must be valid.
See :ref:`the page about inputs <Inputs>` for more details
about what that means.
