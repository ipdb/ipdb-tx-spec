Transaction ID
==============

The ID of a transaction is the SHA3-256 hash
of the unsigned transaction, loosely speaking.
It's a string.
An example is:

``"0e7a9a9047fdf39eb5ead7170ec412c6bffdbe8d7888966584b4014863e03518"``

Here are the steps to compute it.

1. Construct an :term:`associative array` named ``d`` of the form
   (based on JSON syntax):

.. code-block:: bash

   {
      "version": version,
      "inputs": inputs,
      "outputs": outputs,
      "operation": operation,
      "asset": asset,
      "metadata": metadata
    }

Note how ``d`` is missing the ``"id"`` key and value.

2. Compute ``id = hash_of_aa(d)``. There's pseudocode for the ``hash_of_aa()`` function
   on :ref:`the page about cryptographic hashes <Computing the Hash of an Associative Array>`.
   The result (``id``) is a string: the transaction ID.
