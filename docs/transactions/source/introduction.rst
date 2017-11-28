Introduction
============

This is the documentation of the IPDB Transaction Spec (specification).
It formally specifies what's in an IPDB transaction,
how to construct a valid transaction, and how to check if a transaction is valid.

The following things are included in the IPDB Transaction Spec:

- The transaction model (structure and schema).
- The JSON Schema files for transactions.
  (The version number is in the filename, e.g. ``transaction_v2.0.yaml``.)
  Those files can be found in the ``tx_schema/`` directory of the
  `ipdb/ipdb-tx-spec repository on GitHub
  <https://github.com/ipdb/ipdb-tx-spec>`_
- The process to construct a valid transaction.
- The :ref:`Common Operations` used when constructing and validating transactions.
- The constraints that must be satisfied for a transaction to be valid,
  i.e. the transaction validation rules.

