How to Construct a Transaction
==============================

This page lists the steps to construct a valid transaction.

One begins by constructing an "unfulfilled transaction."
It's the thing-that-gets-signed when computing all the
cryptographic signatures needed
to construct a valid "fulfilled transaction."

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
   (We're building the unfulfilled transaction first.)
#. Compute the :ref:`transaction ID <Transaction ID>`
   and store it in a variable named ``id``.
#. Construct an :term:`associative array` named ``tx1`` of the form
   (based on JSON syntax):

.. code-block:: bash

   {
      "id": id,
      "version": version,
      "inputs": unfulfilled_inputs,
      "outputs": outputs,
      "operation": operation,
      "asset": asset,
      "metadata": metadata
    }

The result (``tx1``) is the :term:`associative array` form
of an "unfulfilled transaction."
We use that to construct the associated fulfilled transaction:

#. :ref:`Convert tx1 to an IPDB Protocol standard JSON string
   <JSON Serialization & Deserialization>` named ``tx1_json``.
#. :ref:`Convert tx1_json to bytes <Converting Strings to Bytes>`.
   Call the result ``tx1_bytes``.
#. Create ``inputs`` as a deep copy of ``unfulfilled_inputs``.
#. For each input in ``inputs``,
   fulfill the associated crypto condition
   `using an implementation of crypto conditions
   <https://github.com/rfcs/crypto-conditions#implementations>`_.
   You will need ``tx1_bytes`` and one or more private keys
   (which are used to sign ``tx1_bytes``).
   The end result is usually some kind of fulfilled condition object.
   Compute the fulfillment string of that fulfilled condition object, and
   put that as the value of ``"fulfillment"`` for the input in question.
#. Construct ``tx2`` by making a deep copy of ``tx1``
   and setting the value of the ``"inputs"`` key to ``inputs``.

The result (``tx2``) is the :term:`associative array` form
of a valid fulfilled transaction.
To put it in the body of an HTTP POST request,
you'll have to :ref:`convert it to a JSON string
<JSON Serialization & Deserialization>`.


**Example Python Code**

The documentation of the BigchainDB Python Driver has a page titled
`"Handcrafting Transactions"
<https://docs.bigchaindb.com/projects/py-driver/en/latest/handcraft.html>`_
which shows how to do all of the above
in Python (using a Python implementation of crypto conditions).
