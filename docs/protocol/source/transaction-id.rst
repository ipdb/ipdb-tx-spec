Computing Transaction ID
========================

Below is a list of steps to compute the ID of a transaction.
The steps are based on how it's done in Python 3.
The exact steps and intermediate data types may differ in your programming language.

1. Find out what your programming language calls associative arrays. Wikipedia has an `article about programming language support for associative arrays <https://en.wikipedia.org/wiki/Comparison_of_programming_languages_(associative_array)>`_. For example, Python calls them dictionaries.
2. Construct an :term:`associative array` containing ``version``, ``inputs``, ``outputs``, ``operation``, ``asset``, ``metadata`` (but not ``id``) and their values.
3. In each of the inputs, replace the value of each ``fulfillment`` with the equivalent of ``null`` in your programming language.
4. Convert that to a standard Unicode JSON string. See the page about :ref:`JSON serialization and deserialization <JSON Serialization & Deserialization>`.
5. Convert that to bytes. See the page about :ref:`converting strings to bytes <Converting Strings to Bytes>`.
6. Compute the SHA3-256 hash of that and represent the hash as a hexadecimal (Base16) string. See the page about :ref:`computing hashes <Computing Hashes>`.

The final (hex-encoded) string is the transaction ID.
