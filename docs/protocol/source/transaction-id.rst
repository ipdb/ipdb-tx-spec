Transaction ID
==============

The ID of a transaction is, roughly speaking, the hash of the transaction
after changing all its ``fulfillment`` values to ``null``.
Below is a list of steps to compute it.

1. Find out what your programming language calls associative arrays. Wikipedia has an `article about programming language support for associative arrays <https://en.wikipedia.org/wiki/Comparison_of_programming_languages_(associative_array)>`_. For example, Python calls them dictionaries.
2. Construct an :term:`associative array` ``d1`` containing ``version``, ``inputs``, ``outputs``, ``operation``, ``asset``, ``metadata`` (but not ``id``) and their values.
3. In ``d1``, for each of the inputs in ``inputs``, replace the value of each ``fulfillment`` with the equivalent of ``null`` in your programming language. Call the resulting associative array ``d2``.
4. Compute ``id = hash_of_aa(d2)``. There's pseudocode for the ``hash_of_aa()`` function on :ref:`the page about cryptographic hashes <Computing the Hash of an Associative Array>`. The result (``id``) is a string: the transaction ID.
