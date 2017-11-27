Versioning and What's Included
==============================

The IPDB Protocol will have several versions.

- Version numbers have the form X.Y.Z.
- Version numbers are assigned in accordance
  with `Semantic Versioning <https://semver.org/>`_.
- *Supported* version numbers start at 2.0.0 (not 1).
- "Version N" means "Version N.0.0".
- "Version N.0" means "Version N.0.0".

The following things are included in version 2
of the IPDB Protocol specification:

- (TODO: Remove? Postpone?) :ref:`Server Requirements`
- (TODO: Remove? Postpone?) :ref:`Driver Requirements`
- Version 2.0 of the transaction model,
  as described in the :ref:`Transactions` section
- Version 2.0 of the JSON Schema files for transactions.
  (The version number is in the filename, e.g. ``transaction_v2.0.yaml``.)
  Those files can be found in the ``tx_schema/`` directory of the
  `ipdb/ipdb-protocol repository on GitHub
  <https://github.com/ipdb/ipdb-protocol>`_
- The :ref:`Common Operations` when constructing and validating transactions


Versions, Git Branches and ReadTheDocs
--------------------------------------

The ipdb/ipdb-protocol repository handles versioning, Git branches,
Git tags, and ReadTheDocs
using the same process/workflow as bigchaindb/bigchaindb.
See the `RELEASE_PROCESS.md file
<https://github.com/bigchaindb/bigchaindb/blob/master/RELEASE_PROCESS.md>`_
in that repository.

- The dev branch is ``master``
- Every minor version X.Y has its own branch
- Every release (X.Y.Z) corresponds to a Git tag on a particular commit
  in a minor version branch
- The file named ``version.py`` is in this repository's root directory,
  not ``bigchaindb/version.py``.
