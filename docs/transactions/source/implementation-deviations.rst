Implementation-Specific Deviations
==================================

Some implementations of IPDB-compliant servers
or drivers deviate from the IPDB Transaction Spec.
This page summarizes those deviations.


BigchainDB Server
-----------------

`BigchainDB Server <https://github.com/bigchaindb/bigchaindb>`_
is an IPDB-compliant server implemented in Python.

It allows :ref:`operation <Operation>` to have the value ``"GENESIS"``,
but only for transactions in the GENESIS block.


BigchainDB Server with MongoDB
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

When BigchainDB Server is used *with MongoDB*,
it inherits some quirks from MongoDB:

- All key names (e.g. anywhere in the JSON documents stored
  in ``asset.data`` or ``metadata``):

   - must not begin with ``$``
   - must not contain ``.``
   - must not contain the `null character 
     <https://en.wikipedia.org/wiki/Null_character>`_ (Unicode code point U+0000)

- If there's a key named ``"language"``
  (e.g. anywhere in the JSON documents stored
  in ``asset.data`` or ``metadata``),
  then its value must be one of the `supported values (language codes)
  <https://docs.mongodb.com/manual/reference/text-search-languages/>`_,
  because MongoDB uses that to guide its full text search.
  Moreover, BigchainDB Server only allows the language codes
  supported by *MongoDB Community Edition* (not MongoDB Enterprise).
