How to Construct a Transaction
==============================

This page lists the steps to construct a valid transaction.

#. Set a variable named ``version`` to a :ref:`valid version value
   <Version>`.
#. Set a variable named ``operation`` to a :ref:`valid operation value
   <Operation>`.
#. Set a variable named ``asset`` to a :ref:`valid asset <Asset>`.
#. Set a variable named ``metadata`` to a :ref:`valid metadata
   <Metadata>`.
#. Generate or get all the required :ref:`public keys <Cryptographic Keys & Signatures>`.
#. Construct a list/array/tuple named ``outputs`` of all the :ref:`outputs <Outputs>`
   that should be in the transaction.
   (Note: Each output includes a :ref:`condition <Conditions>`.)
#. Construct a list/array/tuple named ``unfulfilled_inputs``
   of all the :ref:`inputs <Inputs>`
   that should be in the transaction.
   All fulfillment strings should be set to
   the equivalent of :term:`null` in your programming language.
   (We're building an "unfulfilled transaction" first.)
#. Construct an :term:`associative array` named ``unfulfilled_tx`` of the form
   (based on JSON syntax):

.. code-block:: bash

   {
      "id": null,
      "version": version,
      "inputs": unfulfilled_inputs,
      "outputs": outputs,
      "operation": operation,
      "asset": asset,
      "metadata": metadata
    }

Note how ``unfulfilled_tx`` includes a key-value pair for the ``"id"`` key.
The value must be your programming language's equivalent of :term:`null`.

We will now convert ``unfulfilled_tx`` to something
that can be signed:

#. :ref:`Convert unfulfilled_tx to an IPDB-standard JSON string
   <JSON Serialization & Deserialization>` named ``utx_json``.
#. :ref:`Convert utx_json to bytes <Converting Strings to Bytes>`.
   Call the result ``utx_bytes``.

``utx_bytes`` can be signed to compute all signatures and fulfillment strings.

#. Create ``inputs`` as a deep copy of ``unfulfilled_inputs``.
#. For each input in ``inputs``,
   fulfill the associated crypto-condition
   `using an implementation of crypto-conditions
   <https://github.com/rfcs/crypto-conditions#implementations>`_.
   You will need ``utx_bytes`` and one or more private keys
   (which are used to sign ``utx_bytes``).
   The end result is usually some kind of fulfilled condition object.
   Compute the fulfillment string of that fulfilled condition object, and
   put that as the value of ``"fulfillment"`` for the input in question.
#. Construct a new :term:`associative array` ``tx``
   by making a deep copy of ``unfulfilled_tx``.
#. In ``tx``, change the value of ``"inputs"`` to the just-computed
   ``inputs`` (an array of fulfilled inputs).
#. :ref:`Compute the transaction ID of tx <Transaction ID>`.
   Call it ``computed_id``.
#. In ``tx``, change the value of ``"id"`` to ``computed_id``.

The final result (``tx``) is a valid fulfilled transaction
(in the form of an associative array).
To put it in the body of an HTTP POST request,
you'll have to :ref:`convert it to a JSON string
<JSON Serialization & Deserialization>`.


**Example Python Code**

The documentation of the BigchainDB Python Driver has a page titled
`"Handcrafting Transactions"
<https://docs.bigchaindb.com/projects/py-driver/en/latest/handcraft.html>`_
which shows how to do all of the above
in Python (using a Python implementation of crypto-conditions).
