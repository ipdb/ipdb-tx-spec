Cryptographic Keys
==================

The IPDB Protocol uses
the `Ed25519 <https://ed25519.cr.yp.to/>`_ public-key signature system
for generating its public/private key pairs
(also called verifying/signing key pairs).
Ed25519 is an instance of the
`Edwards-curve Digital Signature Algorithm (EdDSA) <https://en.wikipedia.org/wiki/EdDSA>`_.
There's more information about EdDSA and Ed25519 in 
`RFC 8032 <https://tools.ietf.org/html/rfc8032>`_.

When representing public/private keys as strings
(e.g. inside :ref:`transactions <Transactions>`),
the IPDB Protocol represents them with a
`Base58 encoding <https://en.wikipedia.org/wiki/Base58>`_.

.. warning::

   There is no standard for Base58 encoding.
   The meaning of Base58 varies from library to library.
   The Base58 encoding used by the IPDB Protocol
   is that which is implemented by
   `the Python package named base58 <https://pypi.python.org/pypi/base58>`_,
   which is the same as what's used for Bitcoin addresses.

Here's an example Ed25519 public/private key pair:

.. code:: javascript

   "keypair": {
      "public": "9WYFf8T65bv4S8jKU8wongKPD4AmMZAwvk1absFDbYLM",
      "private": "3x7MQpPq8AEUGEuzAxSVHjU1FhLWVQJKFNNkvHhJPGCX"
   }


**Python 3**

One can use the 
`cryptoconditions package <https://github.com/bigchaindb/cryptoconditions>`_
to generate an Ed25519 key pair, by using the function
``ed25519_generate_key_pair()``.
The cryptoconditions package, in turn, uses the
`PyNaCl package <https://pypi.python.org/pypi/PyNaCl>`_,
a Python binding to the Networking and Cryptography (NaCl) library.

.. code:: python

   from cryptoconditions import crypto

   # Generate a tuple of two bytes objects, each with Base58 encoding
   keypair_as_bytes = crypto.ed25519_generate_key_pair()

   # Decode the above tuple to a tuple of two Python 3 str objects (Unicode)
   keypair = tuple([k.decode() for k in keypair_as_bytes])

Note that the ``bytes.decode()`` method assumes a UTF-8 encoding by default.

Why does the above code work?
The bytes objects in ``keypair_as_bytes`` were already encoded in Base58,
and if you convert a Base58 sequence (i.e. a sequence of basic ASCII characters)
to a Unicode sequence (UTF-8), then the result will be the same sequence
of ASCII characters.
