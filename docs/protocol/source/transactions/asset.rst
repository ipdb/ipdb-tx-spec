Asset
=====

In a CREATE transaction,
an asset can be the equivalent of :term:`null` or
an :term:`associative array` containing exactly one key-value pair.
The key must be ``"data"``
and the value can be any valid associative array.
Here's a JSON example:

.. code-block:: json

   {
      "data": {
                 "desc": "Gold-inlay bookmark owned by Xavier Bellomat Dickens III",
                 "xbd_collection_id": 1857
              }
   }

The meaning of a "valid associative array" may
depend on the implementation; see the page about
:ref:`implementation-specific deviations <Implementation-Specific Deviations>`.

In a TRANSFER transaction,
an asset can be the equivalent of :term:`null` or
an :term:`associative array` containing exactly one key-value pair.
The key must be ``"id"``
and the value must be a 64-character hex string:
a :ref:`transaction ID <Transaction ID>`.
Here's a JSON example:

.. code-block:: json

   {
      "id": "38100137cea87fb9bd751e2372abb2c73e7d5bcf39d940a5516a324d9c7fb88d"
   }
