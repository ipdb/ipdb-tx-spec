Versioning
==========

The IPDB Transaction Spec is versioned as follows:

- Version numbers have the form X.Y.Z.
- Version numbers are assigned in accordance
  with `Semantic Versioning <https://semver.org/>`_.
- *Supported* version numbers start at 2.0.0 (not 1).
- "Version N" means "Version N.0.0".
- "Version N.0" means "Version N.0.0".


Versions, Git Branches and ReadTheDocs
--------------------------------------

The Git repository for the IPDB Transaction Spec
handles versioning, Git branches, Git tags, and ReadTheDocs
using the same process/workflow as the bigchaindb/bigchaindb repository.
See the `RELEASE_PROCESS.md file
<https://github.com/bigchaindb/bigchaindb/blob/master/RELEASE_PROCESS.md>`_
in that repository.

- The dev branch is ``master``.
- Every minor version X.Y has its own branch ``X.Y``.
- Every release (X.Y.Z) corresponds to a Git tag ``vX.Y.Z``
  on a particular commit
  in a minor version branch.

There is one minor difference: the file named ``version.py``
is in this repository's *root directory*, not ``bigchaindb/version.py``.
