Transactions
============

A transaction is a JSON object
with the following top-level keys (also called names or fields),
all of which are required:

.. code-block:: json

    {
        "id": "<ID of the transaction>",
        "version": "<Transaction schema version number>",
        "inputs": ["<List of inputs>"],
        "outputs": ["<List of outputs>"],
        "operation": "<String>",
        "asset": {"<Asset model; see below>"},
        "metadata": {"<Arbitrary transaction metadata>"}
    }

You may wonder where the transaction signatures are.
They're in the inputs.


Keys
----

id
^^

The transaction ID and also the hash of the transaction (loosely speaking).
It's a string.
Here's how one computes the ``id`` using Python:

1. Build a Python dictionary containing ``version``, ``inputs``, ``outputs``, ``operation``, ``asset``, ``metadata`` and their values.
2. In each of the inputs, replace the value of each ``fulfillment`` with ``null``.
3. Serialize that dictionary as described in :ref:`the page about dictionary serialization <Serialization>`.
4. Compute the SHA3-256 hash of that to get the transaction ID. See :ref:`the page about computing hashes <Computing Hashes>`.


version
^^^^^^^

The version-number of the transaction schema.
It's a string.
In version 1.0 of the IPDB Protocol,
the only allowed value of ``"version"`` is ``"1.0"``.


inputs
^^^^^^

A list of transaction inputs.
Each input spends/transfers a previous output by satisfying/fulfilling
the crypto-conditions on that output.
A CREATE transaction must have exactly one input (i.e. == 1).
A TRANSFER transaction must have at least one input (i.e. ≥ 1).
See :ref:`the page about transaction inputs <Transaction Inputs>`.


outputs
^^^^^^^

A list of transaction outputs.
Each output indicates the crypto-conditions which must be satisfied
by anyone wishing to spend/transfer that output.
It also indicates the number of shares of the asset tied to that output.
See :ref:`the page about transaction outputs <Transaction Outputs>`.


operation
^^^^^^^^^

A string indicating what kind of transaction this is,
and how it should be validated.
The allowed values are ``"CREATE"``, ``"TRANSFER"`` and ``"GENESIS"``
(but there should only be one transaction whose operation is ``"GENESIS"``:
the one in the GENESIS block).

.. note::

   The ``"GENESIS"`` transaction might be deprecated in future versions
   of the protocol.


asset
^^^^^

A JSON object for the asset associated with the transaction.
(A transaction can only be associated with one asset.)
See :ref:`the page about assets <Assets>`.


metadata
^^^^^^^^

User-provided transaction metadata.
It can be any valid JSON object, or ``null``.


Examples
--------

Here's an example transaction:

.. code-block:: json

    {
        "id": "0e7a9a9047fdf39eb5ead7170ec412c6bffdbe8d7888966584b4014863e03518",
        "version": "1.0",
        "inputs": [
            {
                "fulfillment": "pGSAIL3aH9oajqk9K5LERXeCNtT-rm_saYXg6IIjlBfWNCLOgUAVgaGMUNF4rKVWeFmGphwJls45cZxttqa-9UKfSGOlLS_80dwsfa3hIo9dC00ojV1xeOGR6AAxU7BIyhJ3j6sH",
                "fulfills": null,
                "owners_before": [
                    "Dn6xz1F9Toy5qbaZUASsvTAUB78HEHs5YPrx6Pd5yu5P"
                ]
            }
        ],
        "outputs": [
            {
                "amount": "1",
                "condition": {
                    "uri": "ni:///sha-256;CNXDAYaEJD1l0hO21ZpLIdjrWZIeE2V9xxuNcZ10Lo8?fpt=ed25519-sha-256&cost=131072",
                    "details": {
                        "public_key": "Dn6xz1F9Toy5qbaZUASsvTAUB78HEHs5YPrx6Pd5yu5P",
                        "type": "ed25519-sha-256"
                    }
                },
                "public_keys": [
                    "Dn6xz1F9Toy5qbaZUASsvTAUB78HEHs5YPrx6Pd5yu5P"
                ]
            }
        ],
        "operation": "CREATE",
        "asset": {
            "data": {
                "time": "09:01:01 10/30/17 CET",
                "type": "test asset"
            }
        },
        "metadata": null
    }


There are more example transactions
in :ref:`the HTTP API docs <HTTP API>`
and the docs of various drivers.
