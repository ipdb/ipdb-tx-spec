Conditions
==========

A condition is a JSON object with a particular schema,
as outlined in this page.
A condition must contain the following top-level JSON keys
(also called names or fields):

.. code-block:: json

   {
       "details": {"<Subcondition object>"},
       "uri": "<URI string>"
   }

The ``uri`` is explained later. First we specify the contents
of a subcondition object.


Subcondition Objects
--------------------

A subcondition (object) is a JSON object with a particular schema.
In this version of the IPDB Protocol, there are two possible subcondition types:

1. ED25519-SHA-256
2. THRESHOLD-SHA-256

Those names are from the
`crypto-conditions specification (spec) 
<https://tools.ietf.org/html/draft-thomas-crypto-conditions-03>`_,
which is part of the Interledger Protocol (ILP).
(It calls them "crypto-condition types.")
The crypto-conditions spec includes other types,
but the above two types are the only ones used by the IPDB Protocol.

.. note::

   This version of the IPDB Protocol conforms to versions 02 and 03
   of the crypto-conditions spec. (The parts that it uses didn't change
   from version 02 to 03.)


Type 1: ED25519-SHA-256
^^^^^^^^^^^^^^^^^^^^^^^

A subcondition of type ED25519-SHA-256 is a JSON object.
It must contain the following top-level JSON keys
(also called names or fields):

.. code-block:: json

   {
      "type": "ed25519-sha-256",
      "public_key": "HFp773FH21sPFrn4y8wX3Ddrkzhqy4La4cQLfePT2vz7"
   }

The ``type`` must be the string ``"ed25519-sha-256"``.

The ``public_key`` must be a valid public key (string).
See the page about :ref:`cryptographic keys and signatures 
<Cryptographic Keys & Signatures>`.

One can fulfill a (sub)condition of this type
by signing a message with the private key corresponding
to the given public key.


Type 2: THRESHOLD-SHA-256
^^^^^^^^^^^^^^^^^^^^^^^^^

A subcondition of type THRESHOLD-SHA-256 is a JSON object.
It must contain the following top-level JSON keys
(also called names or fields):

.. code-block:: json

   {
      "type": "threshold-sha-256",
      "threshold": "<An integer>",
      "subconditions": ["<List of subcondition objects>"]
   }

The ``type`` must be the string ``"threshold-sha-256"``.

The ``threshold`` must be an *integer* *m* between 1 and the number
of objects in the ``subconditions`` list (*n*). It's *not a string*.

The ``subconditions`` must be a list of one or more
subcondition objects (*n* objects). Note the recursive definition:
a threshold subcondition contains a list of subconditions,
some of which may be subconditions of type THRESHOLD-SHA-256.

One can fulfill a (sub)condition of this type
by fulfilling *m* of the *n* subconditions.
Because of that, it's also called an *m*-of-*n* threshold condition.


The URI
-------

If you want to generate a correct condition URI string,
then you should consult the
`crypto-conditions spec (version 03) 
<https://tools.ietf.org/html/draft-thomas-crypto-conditions-03>`_
or use `an existing implementation of crypto-conditions 
<https://github.com/rfcs/crypto-conditions#implementations>`_.

There is some example Python 3 code
for calculating condition URI strings below.


AND & OR Conditions
-------------------

An *m*-of-*n* threshold condition can be thought of
as a logic gate with n Boolean inputs,
where the output is TRUE if, and only if,
*m* or more of the inputs are TRUE.
Therefore:

* 1-of-*n* is the same as a logical OR of all the inputs
* *n*-of-*n* is the same as a logical AND of all the inputs


More Complex Conditions
-----------------------

The (single) output of a threshold condition can be used
as one of the inputs to *another* threshold condition.
That means you can combine threshold conditions
to build complex expressions such as ``(x OR y) AND (2 of {a, b, c})``.

.. image:: /_static/Conditions_Circuit_Diagram.png


Cost of a Condition
-------------------

When you create a condition, you can calculate its
`cost <https://tools.ietf.org/html/draft-thomas-crypto-conditions-03#section-7.2.2>`_,
an estimate of the resources that would be required to validate the fulfillment.
For example, the cost of one ED25519-SHA-256 condition is 131072.

An implementation of an IPDB server may choose
to put an upper limit on the complexity of each condition,
either directly by setting a maximum allowed cost,
or indirectly by setting a maximum allowed transaction size.


Example Conditions
------------------

The Simplest Possible Condition
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The simplest possible condition is one
with a single ED25519-SHA-256 signature (sub)condition.
Here's an example:

.. code-block:: json

   {
       "details": {
           "type": "ed25519-sha-256",
           "public_key": "HFp773FH21sPFrn4y8wX3Ddrkzhqy4La4cQLfePT2vz7"
       },
       "uri": "ni:///sha-256;at0MY6Ye8yvidsgL9FrnKmsVzX0XrNNXFmuAPF4bQeU?fpt=ed25519-sha-256&cost=131072"
   }

**Example Python 3 Code to Compute the Condition URI**

.. code-block:: python

   import base58
   from cryptoconditions import Ed25519Sha256

   pubkey = 'HFp773FH21sPFrn4y8wX3Ddrkzhqy4La4cQLfePT2vz7'

   # Convert pubkey to a bytes representation (a Python 3 bytes object)
   pubkey_bytes = base58.b58decode(pubkey)

   # Construct the condition object
   ed25519 = Ed25519Sha256(public_key=pubkey_bytes)

   # Compute the condition uri (string)
   uri = ed25519.condition_uri
   # uri should be:
   # 'ni:///sha-256;at0MY6Ye8yvidsgL9FrnKmsVzX0XrNNXFmuAPF4bQeU?fpt=ed25519-sha-256&cost=131072'


A 2-of-2 Condition
^^^^^^^^^^^^^^^^^^

Here's an example 2-of-2 condition:

.. code-block:: json

   {
       "details": {
           "type": "threshold-sha-256",
           "threshold": 2,
           "subconditions": [
               {
                   "public_key": "5ycPMinRx7D7e6wYXLNLa3TCtQrMQfjkap4ih7JVJy3h",
                   "type": "ed25519-sha-256"
               },
               {
                   "public_key": "9RSas2uCxR5sx1rJoUgcd2PB3tBK7KXuCHbUMbnH3X1M",
                   "type": "ed25519-sha-256"
                }
            ]       
        },
        "uri": "ni:///sha-256;zr5oThl2kk6613WKGFDg-JGu00Fv88nXcDcp6Cyr0Vw?fpt=threshold-sha-256&cost=264192&subtypes=ed25519-sha-256"
   }

**Example Python 3 Code to Compute the Condition URI**

.. code-block:: python

   import base58
   from cryptoconditions import Ed25519Sha256, ThresholdSha256

   pubkey1 = '5ycPMinRx7D7e6wYXLNLa3TCtQrMQfjkap4ih7JVJy3h'
   pubkey2 = '9RSas2uCxR5sx1rJoUgcd2PB3tBK7KXuCHbUMbnH3X1M'

   # Convert pubkeys to bytes representations (Python 3 bytes objects)
   pubkey1_bytes = base58.b58decode(pubkey1)
   pubkey2_bytes = base58.b58decode(pubkey2)

   # Construct the condition object
   ed25519_1 = Ed25519Sha256(public_key=pubkey1_bytes)
   ed25519_2 = Ed25519Sha256(public_key=pubkey2_bytes)
   threshold_sha256 = ThresholdSha256(threshold=2)
   threshold_sha256.add_subfulfillment(ed25519_1)
   threshold_sha256.add_subfulfillment(ed25519_2)

   # Compute the condition uri (string)
   uri = threshold_sha256.condition.serialize_uri()
   # uri should be:
   # 'ni:///sha-256;zr5oThl2kk6613WKGFDg-JGu00Fv88nXcDcp6Cyr0Vw?fpt=threshold-sha-256&cost=264192&subtypes=ed25519-sha-256'


To change it into a 1-of-2 condition, just change the value of ``threshold`` to 1
and recompute the condition URI.
