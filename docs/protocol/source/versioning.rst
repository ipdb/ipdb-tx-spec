Versioning and What's Included
==============================

The IPDB Protocol will have several versions.
Version numbers start at 1.0 and increment in steps of 1.0,
i.e. 1.0, 2.0, 3.0, etc.
Version numbers may be written with or without the ".0",
depending on the context.

The following things are currently included in version N.0
of the IPDB Protocol specification:

- :ref:`Server Requirements`
- :ref:`Driver Requirements`
- Version N of the :ref:`HTTP API`
- Version N of the :ref:`Event Stream API`
- Version N.0 of the transaction model,
  as described in the :ref:`Transactions` section
- Versions 1.0 through N.0 of the JSON Schema files for transactions.
  (The version number is in the filename, e.g. ``transaction_v1.0.yaml``.)
  Those files can be found in the ``tx_schema/`` directory of the
  `ipdb/ipdb-protocol repository on GitHub
  <https://github.com/ipdb/ipdb-protocol>`_
- :ref:`Common Operations`


Versions, Git Branches and ReadTheDocs
--------------------------------------

There are many ways to use Git, Git Branches, and ReadTheDocs.
In *this* Git repository (ipdb/ipdb-protocol on GitHub):

- Each version has its own Git branch named after the version it covers,
  e.g. ``1.0``, ``2.0``, ``3.0``, etc.
- The default branch on GitHub should be the name
  of the latest stable version (e.g. ``4.0``).
- It's okay to have future, in-development branches for future versions.
  Just be careful to merge into the correct branch!
- The ``master`` branch was used in the early days but it 
  was used to create the ``1.0`` branch
  and then it was abandoned. Don't delete the ``master`` branch.
- ReadTheDocs offers docs from the last commit on each *branch*,
  not specific Git tags (which are created when one does a release with GitHub).
- The "latest" (default) docs on ReadTheDocs should be the latest stable branch/version.
