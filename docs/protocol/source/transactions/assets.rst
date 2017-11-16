Assets
======

In a CREATE transaction, the value of ``"asset"`` must be either ``null`` or a JSON document containing exactly one key-value pair: the key must be ``"data"`` and the value can be any valid JSON document. For example:

.. code-block:: json

    {
        "data": {
                    "desc": "Gold-inlay bookmark owned by Xavier Bellomat Dickens III",
                    "xbd_collection_id": 1857
                }
    }

Please note that the meaning of a "valid JSON object" may
depend on the implementation; see the page about
:ref:`implementation-specific deviations <Implementation-Specific Deviations>`.

In a TRANSFER transaction, the value of ``"asset"`` must be either ``null`` or a JSON document containing exactly one key-value pair: the key must be ``"id"`` and the value must contain a transaction ID (i.e. a SHA3-256 hash: the ID of the CREATE transaction which created the asset, which also serves as the asset ID). For example:

.. code-block:: json

    {
        "id": "38100137cea87fb9bd751e2372abb2c73e7d5bcf39d940a5516a324d9c7fb88d"
    }
