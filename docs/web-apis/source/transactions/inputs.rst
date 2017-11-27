Inputs
======

A list/array/tuple of transaction inputs.

Each input spends/transfers a previous output by satisfying/fulfilling
the crypto-conditions on that output.
A CREATE transaction must have exactly one input (i.e. == 1).
A TRANSFER transaction must have at least one input (i.e. ≥ 1).

There's a high-level overview of transaction inputs and outputs
in `the BigchainDB root docs page about transaction concepts 
<https://docs.bigchaindb.com/en/latest/transaction-concepts.html>`_.
The IPDB Protocol is modelled around "assets,"
and transaction *inputs* and *outputs*
are the mechanism by which control of an asset
(or shares of an asset) is transferred.
Amounts of an asset are encoded in the outputs of a transaction,
and each output may be spent separately.
To spend an output, the output's ``condition`` must be met
by an ``input`` that provides a corresponding ``fulfillment``.
Each output may be spent at most once, by a single input.
Note that any asset associated with an output holding an amount greater than one
is considered a divisible asset that may be split up in future transactions.

An input can be implemented as an :term:`associative array`
in almost any programming language (e.g. as a dictionary in Python).
It has the following basic structure:

.. code-block:: bash

   {
      "fulfills": {
         "transaction_id": "<ID of the transaction containing the output being spent>",
         "output_index": index
      },
      "owners_before": ["<The public_keys list in the output being spent>"],
      "fulfillment": "<String that fulfills the condition in the output being spent>"
   }


The Keys in a Transaction Input
-------------------------------

**fulfills**

If the transaction is a TRANSFER transaction,
then this is like a pointer to the :ref:`output <Outputs>` being spent/transferred.
More specifically, it's an :term:`associative array` with two key/value pairs:

- ``transaction_id`` is the ID of the transaction where the output is located. It's a string.
- ``output_index`` is the index of the output being spent. It's an integer, *not a string*. Example values are ``0``, ``1`` and ``12`` (*not* ``"0"`` or any other string).

An example is:

.. code-block:: json

    {
        "transaction_id": "107ec21f4c53cd2a934941010437ac74882161bcbefdfd7664268823fc347996",
        "output_index": 0
    }

If the transaction is a CREATE transaction,
then the value of ``fulfills`` must be
the equivalent of :term:`null` in your programming language,
because there is no other transaction output that it's transferring/spending.


**owners_before**

If the transaction is a CREATE transaction,
then this is a list of public keys (strings):
the *issuers* of the asset in question.
Those issuers must sign the CREATE transaction,
i.e. they must compute the ``fulfillment`` string using their private keys.

If the transaction is a TRANSFER transaction,
then this list must agree with the
``public_keys`` list in the :ref:`output <Outputs>`
being transferred/spent.

.. note::

   See the :ref:`note about "owners" <A Note about Owners>`.


**fulfillment**

If the transaction is a CREATE transaction,
then the fulfillment must fulfill
an implicit *n*-of-*n* signature condition,
i.e. one signature from each of the *n* ``owners_before``.
If the transaction is a TRANSFER transaction,
then the fulfillment must fulfill the condition
in the :ref:`output <Outputs>` that is being transferred/spent.
The page about :ref:`conditions <Conditions>` explains how
to construct a condition object.

The specifics of how to compute a fulfillment
for a condition (and the associated fulfillment string)
are given in the crypto conditions spec.
Consult the
`crypto-conditions spec (version 03) 
<https://tools.ietf.org/html/draft-thomas-crypto-conditions-03>`_
or use `an existing implementation of crypto-conditions 
<https://github.com/rfcs/crypto-conditions#implementations>`_.

The page about :ref:`how to construct a transaction
<How to Construct a Transaction>` gives more details,
including a link to example Python code.

Here's an example fulfillment string:

.. code-block:: json

   "pGSAIDgbT-nnN57wgI4Cx17gFHv3UB_pIeAzwZCk10rAjs9bgUDxyNnXMl-5PFgSIOrN7br2Tz59MiWe2XY0zlC7LcN52PKhpmdRtcr7GR1PXuTfQ9dE3vGhv7LHn6QqDD6qYHYM"

.. note::

   The basic steps to compute a fulfillment string are:

   #. Construct the fulfillment as per the crypto-conditions spec.
   #. Encode the fulfillment to bytes using the `ASN.1 Distinguished Encoding Rules (DER) <http://www.itu.int/ITU-T/recommendations/rec.aspx?rec=12483&lang=en>`_.
   #. Encode the resulting bytes using "base64url" (*not* typical base64) as per `RFC 4648, Section 5 <https://tools.ietf.org/html/rfc4648#section-5>`_.
