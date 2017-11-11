Cryptographic Hashes
====================

IPDB Protocol Standard Hashes
-----------------------------

The IPDB Protocol uses NIST-standard SHA3-256 hashes.

.. warning::

   During the finalization of SHA3, NIST changed the delimiter suffix from 0x01 to 0x06.
   Older SHA3 libraries might use the old suffix,
   so make sure you use an up-to-date library when calculating SHA3-256 hashes.

A SHA3-256 hash can be represented as a sequence of 256 bits, 32 bytes,
or many other ways.
When representing hashes as strings
(e.g. inside :ref:`transactions <Transactions>`),
the IPDB Protocol represents them with a hexadecimal encoding:
a sequence of hexadecimal digits (0–9 and a–f).
Every byte can be represented by two hexadecimal digits
so the hexadecimal string should have 64 characters.
An example is:

``"ee788e85a9b5ae9aa9af4fe71458e8b3b72d2e0f290f3e6bc0bdaa262b60a860"``


**Example Python 3 Code**

Install the package pysha3 1.0 or greater (from PyPI).
It's a wrapper around 
`the optimized Keccak Code Package (KCP) <https://github.com/gvanas/KeccakCodePackage>`_.
The following code snippet calculates the SHA3-256 hash
of the input ``json_bytes`` (a Python 3
`bytes object <https://docs.python.org/3/library/stdtypes.html#bytes-objects>`_)
and then converts it to a hexadecimal string (a Python 3
`str object <https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str>`_).

.. code-block:: python

   import sha3

   # json_bytes is a bytes object
   hash_as_hex_string = sha3.sha3_256(json_bytes).hexdigest()

Note: ``sha3.sha3_256(json_bytes)`` is an intermediate object of class
'_pysha3.sha3_256'.


Computing the Hash of an Associative Array
------------------------------------------

There's an IPDB Protocol standard way to compute the hash
of an :term:`associative array`. We've called that function ``hash_of_aa()``
elsewhere in this documentation. It takes an associative array ``d`` as input
and returns a string as output. Here is what that function must do:

1. Convert ``d`` to a standard Unicode JSON string. See the page about :ref:`JSON serialization and deserialization <JSON Serialization & Deserialization>`. Call the resulting string ``d_json``.
2. Convert ``d_json`` to bytes (i.e. a sequence of bytes). See the page about :ref:`converting strings to bytes <Converting Strings to Bytes>`. Call the resulting bytes ``d_bytes``.
3. Compute the SHA3-256 hash of ``d_bytes`` as outlined at the top of this page, and represent the hash as a hexadecimal string.
