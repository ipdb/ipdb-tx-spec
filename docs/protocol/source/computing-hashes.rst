Computing Hashes
================

The IPDB Protocol uses NIST-standard SHA3-256 hashes.

.. warning::

   During the finalization of SHA3, NIST changed the delimiter suffix from 0x01 to 0x06.
   Older SHA3 libraries might use the old suffix,
   so make sure you use an up-to-date library when calculating SHA3-256 hashes.

A SHA3-256 hash can be represented as a sequence of 256 bits or 32 bytes
but for the purposes of the IPDB Protocol,
you should represent them as a hexadecimal string
(i.e. a sequence of hexadecimal digits).
Every byte can be represented by two hexadecimal characters
so the hexadecimal string should have 64 characters.
An example is
"ee788e85a9b5ae9aa9af4fe71458e8b3b72d2e0f290f3e6bc0bdaa262b60a860".


**Python 3**

Install the package pysha3 1.0 or greater (from PyPI).
It's a wrapper around 
`the optimized Keccak Code Package (KCP) <https://github.com/gvanas/KeccakCodePackage>`_.
The following code snippet calculates the SHA3-256 hash
of the input ``json_bytes`` (a Python 3
`bytes object <https://docs.python.org/3/library/stdtypes.html#bytes-objects>`_)
and then converts it to a hexadecimal string (a Python 3
`str object <https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str>`_).

.. code:: python

   import sha3

   # json_bytes is a bytes object
   hash_as_hex_string = sha3.sha3_256(json_bytes).hexdigest()

Note: ``sha3.sha3_256(json_bytes)`` is an intermediate object of class
'_pysha3.sha3_256'.
