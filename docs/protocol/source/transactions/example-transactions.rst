Example Transactions
====================

Here's an example CREATE transaction as JSON:

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
