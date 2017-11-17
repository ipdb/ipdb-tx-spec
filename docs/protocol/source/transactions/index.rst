Transactions
============

A transaction can be implemented as an :term:`associative array`
in almost any programming language (e.g. as a dictionary in Python).
It has the following basic structure:

.. code-block:: bash

   {
      "id": id,
      "version": version,
      "inputs": inputs,
      "outputs": outputs,
      "operation": operation,
      "asset": asset,
      "metadata": metadata
    }

You may wonder where the transaction signatures are.
They're in the inputs.

.. toctree::
   :maxdepth: 1

   transaction-id
   version
   inputs
   outputs
   conditions
   operation
   asset
   metadata
   construct-a-tx
   example-transactions
