Transaction ID
==============

The ID of a transaction is the SHA3-256 hash
of the unsigned transaction, loosely speaking.
It's a string.
An example is:

``"0e7a9a9047fdf39eb5ead7170ec412c6bffdbe8d7888966584b4014863e03518"``

Here are the steps to compute it.

1. Construct an :term:`associative array` named ``d`` like:

.. code-block:: bash

   {
      "version": version,
      "inputs": inputs,
      "outputs": outputs,
      "operation": operation,
      "asset": asset,
      "metadata": metadata
    }

Note how ``d`` is missing the "id" key and value.

2. In ``d``, for each of the inputs in ``inputs``, replace the value of each ``fulfillment``
   with the equivalent of :term:`null` in your programming language.
   Call the resulting associative array ``d2``.
3. Compute ``id = hash_of_aa(d2)``. There's pseudocode for the ``hash_of_aa()`` function
   on :ref:`the page about cryptographic hashes <Computing the Hash of an Associative Array>`.
   The result (``id``) is a string: the transaction ID.
