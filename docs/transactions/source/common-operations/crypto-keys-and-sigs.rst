Cryptographic Keys & Signatures
===============================

The IPDB Transaction Spec uses
the `Ed25519 <https://ed25519.cr.yp.to/>`_ public-key signature system for:

- generating public/private key pairs (also called verifying/signing key pairs),
- calculting signatures, and
- verifying signatures.

Ed25519 is an instance of the
`Edwards-curve Digital Signature Algorithm (EdDSA) <https://en.wikipedia.org/wiki/EdDSA>`_.
There's more information about EdDSA and Ed25519 in 
`RFC 8032 <https://tools.ietf.org/html/rfc8032>`_.

When representing public/private keys as strings
(e.g. inside :ref:`transactions <Transactions>`),
they must be represented with a
`Base58 encoding <https://en.wikipedia.org/wiki/Base58>`_.

.. warning::

   There is no standard for Base58 encoding.
   The meaning of Base58 varies from library to library.
   The Base58 encoding used by the IPDB Transaction Spec
   is that which is implemented by
   `the Python package named base58 <https://pypi.python.org/pypi/base58>`_,
   which is the same as what's used for Bitcoin addresses.

Here's an example Ed25519 public/private key pair:

.. code-block:: json

   {
      "public": "9WYFf8T65bv4S8jKU8wongKPD4AmMZAwvk1absFDbYLM",
      "private": "3x7MQpPq8AEUGEuzAxSVHjU1FhLWVQJKFNNkvHhJPGCX"
   }

To sign a message, one uses a private key, i.e.
signature = signing_function(message, private_key).
To verify a signature, one needs the corresponding public key, i.e.
signature_is_valid = verifying_function(message, public_key, signature).
The signing_function and verifying_function are provided
by the Ed25519 signature system.
The reference implementation of Ed25519 is in the 
`Networking and Cryptography Library (NaCl) <https://nacl.cr.yp.to/>`_,
but there are
`many other implementations <https://ianix.com/pub/ed25519-deployment.html>`_.

When representing calculated *signatures* as strings (e.g. inside blocks),
they must be represented with a
`Base58 encoding <https://en.wikipedia.org/wiki/Base58>`_.
Here's an example signature:

.. code-block:: json

   "8Z6GJFLSvHmWVqN4dJHshcamNR3cYMwsk9bKScjd32ZgMEtbVSrujHDqrPpdyzBo3tpdse4N4YHXZGXdHfjZZhH"

The keys and signatures that go into
:ref:`outputs <Outputs>` and :ref:`inputs <Inputs>`
follow the 
`crypto-conditions specification
<https://tools.ietf.org/html/draft-thomas-crypto-conditions-03>`_.
However, the IPDB Transaction Spec only allows for Ed25519 keys and signatures.
(It doesn't allow for the RSA ones which are also
part of the crypto-conditions specification.)
The Ed25519 functions used to generate keys, calculate signatures,
and verify signatures are the *same* across the IPDB Transaction Spec.
Those calculations aren't done differently inside inputs or outputs.


**Example Python 3 Code**

The Python package `BigchainDB <https://pypi.python.org/pypi/BigchainDB>`_
is a Python 3 reference implementation
of an IPDB-compliant server. Its source code is in the 
`bigchaindb/bigchaindb <https://github.com/bigchaindb/bigchaindb/>`_
repository on GitHub.
There you can see how it generates public/private key pairs,
calculates signatures, and verifies signatures: it uses the
`cryptoconditions package <https://github.com/bigchaindb/cryptoconditions>`_.
The cryptoconditions package, in turn, uses the
`PyNaCl package <https://pypi.python.org/pypi/PyNaCl>`_,
a Python binding to `libsodium <https://github.com/jedisct1/libsodium>`_,
which is a fork of the Networking and Cryptography library. 


Computing the Signature of an Associative Array
-----------------------------------------------

There's an IPDB-standard way to compute the signature
of an :term:`associative array`.
We've called that function ``sig_of_aa()`` elsewhere in this documentation.
It takes two inputs: an associative array ``d`` and a ``private_key``.
It returns a signature string as output. Here is what that function must do:

#. Convert ``d`` to a standard Unicode JSON string. See the page about
   :ref:`JSON serialization and deserialization <JSON Serialization & Deserialization>`.
   Call the resulting string ``d_json``.
#. Convert ``d_json`` to bytes (i.e. a sequence of bytes). See the page about
   :ref:`converting strings to bytes <Converting Strings to Bytes>`.
   Call the resulting bytes ``d_bytes``.
#. Calculate the Ed25519 signature of ``d_bytes`` using the given ``private_key``.
