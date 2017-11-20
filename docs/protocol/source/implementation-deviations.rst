Implementation-Specific Deviations
==================================

Some software implementations of IPDB servers or drivers deviate
from the IPDB Protocol. This page summarizes those deviations.


BigchainDB Server
-----------------

`BigchainDB Server <https://github.com/bigchaindb/bigchaindb>`_
is a Python implementation of an IPDB server.

When checking the validity of a transaction,
BigchainDB Server checks it against a formal schema
defined in a set of `JSON Schema <http://json-schema.org/>`_ files.
The BigchainDB Server documentation contains the
`full text of those JSON Schema files
<https://docs.bigchaindb.com/projects/server/en/master/appendices/tx-yaml-files.html>`_.


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
