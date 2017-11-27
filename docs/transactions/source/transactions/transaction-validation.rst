Transaction Validation
======================

If a transaction satisfies the constraints listed below,
then it is considered valid.
The process of checking those constraintst is called transaction validation.


JSON Schema Validation
----------------------

When checking the validity of a transaction,
an IPDB-compliant server
must check it against a formal schema
defined in a set of `JSON Schema <http://json-schema.org/>`_ files.
Those files can be found
in the `ipdb/ipdb-protocol repository on GitHub
<https://github.com/ipdb/ipdb-protocol>`_,
in the ``tx_schema/`` directory.

Tip 1: There's a nice explanation of JSON Schema in the website
`"Understanding JSON Schema"
<https://spacetelescope.github.io/understanding-json-schema/index.html>`_.

Tip 2: Python 3 code for checking a transaction against those JSON Schema files
can be found in the `source code of BigchainDB Server
<https://github.com/bigchaindb/bigchaindb>`_.
At the time of writing, it was in the file
``bigchaindb/common/schema/__init__.py``.


Other Constraints
-----------------

TODO
