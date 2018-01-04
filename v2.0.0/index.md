<a href="index.html#document-index" class="icon icon-home">The IPDB Transaction Spec version 2.0.0</a>
2.0

-   <a href="index.html#document-introduction" class="reference internal">Introduction</a>
-   <a href="index.html#document-versioning" class="reference internal">Versioning</a>
    -   <a href="index.html#versions-git-branches-and-readthedocs" class="reference internal">Versions, Git Branches and ReadTheDocs</a>
-   <a href="index.html#document-transaction-components/index" class="reference internal">Transaction Components</a>
    -   <a href="index.html#document-transaction-components/transaction-id" class="reference internal">Transaction ID</a>
    -   <a href="index.html#document-transaction-components/version" class="reference internal">Version</a>
    -   <a href="index.html#document-transaction-components/inputs" class="reference internal">Inputs</a>
    -   <a href="index.html#document-transaction-components/outputs" class="reference internal">Outputs</a>
    -   <a href="index.html#document-transaction-components/conditions" class="reference internal">Conditions</a>
    -   <a href="index.html#document-transaction-components/operation" class="reference internal">Operation</a>
    -   <a href="index.html#document-transaction-components/asset" class="reference internal">Asset</a>
    -   <a href="index.html#document-transaction-components/metadata" class="reference internal">Metadata</a>
-   <a href="index.html#document-construct-a-tx" class="reference internal">How to Construct a Transaction</a>
-   <a href="index.html#document-common-operations/index" class="reference internal">Common Operations</a>
    -   <a href="index.html#document-common-operations/json-serialization" class="reference internal">JSON Serialization &amp; Deserialization</a>
    -   <a href="index.html#document-common-operations/string-to-bytes" class="reference internal">Converting Strings to Bytes</a>
    -   <a href="index.html#document-common-operations/crypto-hashes" class="reference internal">Cryptographic Hashes</a>
    -   <a href="index.html#document-common-operations/crypto-keys-and-sigs" class="reference internal">Cryptographic Keys &amp; Signatures</a>
-   <a href="index.html#document-transaction-validation" class="reference internal">Transaction Validation</a>
    -   <a href="index.html#how-validation-code-decides-which-version-to-use" class="reference internal">How Validation Code Decides Which Version to Use</a>
    -   <a href="index.html#json-schema-validation" class="reference internal">JSON Schema Validation</a>
    -   <a href="index.html#other-constraints" class="reference internal">Other Constraints</a>
-   <a href="index.html#document-example-transactions" class="reference internal">Example Transactions</a>
-   <a href="index.html#document-implementation-deviations" class="reference internal">Implementation-Specific Deviations</a>
    -   <a href="index.html#bigchaindb-server" class="reference internal">BigchainDB Server</a>
-   <a href="index.html#document-ownership" class="reference internal">A Note about Owners</a>
-   <a href="index.html#document-glossary" class="reference internal">Glossary</a>

** [The IPDB Transaction Spec version 2.0.0](index.html#document-index)
-   [Docs](index.html#document-index) »
-   The IPDB Transaction Spec version 2.0.0 2.0.0 documentation
-   

------------------------------------------------------------------------

The IPDB Transaction Spec<a href="#the-ipdb-transaction-spec" class="headerlink" title="Permalink to this headline">¶</a>
=========================================================================================================================

<span id="document-introduction"></span>
Introduction<a href="#introduction" class="headerlink" title="Permalink to this headline">¶</a>
-----------------------------------------------------------------------------------------------

This is the documentation of the IPDB Transaction Spec (specification). It formally specifies what’s in an IPDB transaction, how to construct a valid transaction, and how to check if a transaction is valid.

The following things are included in the IPDB Transaction Spec:

-   The transaction model (structure and schema).
-   The JSON Schema files for transactions. (The version number is in the filename, e.g. `transaction_v2.0.yaml`.) Those files can be found in the `tx_schema/` directory of the <a href="https://github.com/ipdb/ipdb-tx-spec" class="reference external">ipdb/ipdb-tx-spec repository on GitHub</a>
-   The process to construct a valid transaction.
-   The <a href="index.html#common-operations" class="reference internal"><span class="std std-ref">Common Operations</span></a> used when constructing and validating transactions.
-   The constraints that must be satisfied for a transaction to be valid, i.e. the transaction validation rules.

<span id="document-versioning"></span>
Versioning<a href="#versioning" class="headerlink" title="Permalink to this headline">¶</a>
-------------------------------------------------------------------------------------------

The IPDB Transaction Spec is versioned as follows:

-   Version numbers have the form X.Y.Z.
-   Version numbers are assigned in accordance with <a href="https://semver.org/" class="reference external">Semantic Versioning</a>.
-   *Supported* version numbers start at 2.0.0 (not 1).
-   “Version N” means “Version N.0.0”.
-   “Version N.0” means “Version N.0.0”.

### Versions, Git Branches and ReadTheDocs<a href="#versions-git-branches-and-readthedocs" class="headerlink" title="Permalink to this headline">¶</a>

The Git repository for the IPDB Transaction Spec handles versioning, Git branches, Git tags, and ReadTheDocs using the same process/workflow as the bigchaindb/bigchaindb repository. See the <a href="https://github.com/bigchaindb/bigchaindb/blob/master/RELEASE_PROCESS.md" class="reference external">RELEASE_PROCESS.md file</a> in that repository.

-   The dev branch is `master`.
-   Every minor version X.Y has its own branch `X.Y`.
-   Every release (X.Y.Z) corresponds to a Git tag `vX.Y.Z` on a particular commit in a minor version branch.

There is one minor difference: the file named `version.py` is in this repository’s *root directory*, not `bigchaindb/version.py`.

<span id="document-transaction-components/index"></span>
Transaction Components<a href="#transaction-components" class="headerlink" title="Permalink to this headline">¶</a>
-------------------------------------------------------------------------------------------------------------------

A transaction can be implemented as an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> in almost any programming language (e.g. as a dictionary in Python). It has the following basic structure:

    {
       "id": id,
       "version": version,
       "inputs": inputs,
       "outputs": outputs,
       "operation": operation,
       "asset": asset,
       "metadata": metadata
     }

You may wonder where the transaction signatures are. They’re in the inputs.

<span id="document-transaction-components/transaction-id"></span>
### Transaction ID<a href="#transaction-id" class="headerlink" title="Permalink to this headline">¶</a>

The ID of a transaction is the SHA3-256 hash of the transaction, loosely speaking. It’s a string. An example is:

`"0e7a9a9047fdf39eb5ead7170ec412c6bffdbe8d7888966584b4014863e03518"`

Here are the steps to compute it.

1.  Construct an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> named `d` of the form (based on JSON syntax):

    {
       "id": null,
       "version": version,
       "inputs": inputs,
       "outputs": outputs,
       "operation": operation,
       "asset": asset,
       "metadata": metadata
     }

Note how `d` includes a key-value pair for the `"id"` key. The value must be your programming language’s equivalent of <a href="index.html#term-null" class="reference internal"><span class="xref std std-term">null</span></a>.

1.  Compute `id = hash_of_aa(d)`. There’s pseudocode for the `hash_of_aa()` function on <a href="index.html#computing-the-hash-of-an-associative-array" class="reference internal"><span class="std std-ref">the page about cryptographic hashes</span></a>. The result (`id`) is a string: the transaction ID.

<span id="document-transaction-components/version"></span>
### Version<a href="#version" class="headerlink" title="Permalink to this headline">¶</a>

The version indicates the transaction validation rules to be used when validating the transaction, i.e. the rules associated with that version of the IPDB Transaction Spec. It must be a string.

For example, if the value is `"2.0"`, then the transaction will be validated according the <a href="index.html#transaction-validation" class="reference internal"><span class="std std-ref">transaction validation rules</span></a> of version 2.0.0 of the IPDB Transaction Spec.

To indicate version 2, the only allowed value is `"2.0"` (not `"2"` or `"2.0.0"`).

<span id="document-transaction-components/inputs"></span>
### Inputs<a href="#inputs" class="headerlink" title="Permalink to this headline">¶</a>

A list/array/tuple of transaction inputs.

Each input spends/transfers a previous output by satisfying/fulfilling the crypto-conditions on that output. A CREATE transaction must have exactly one input (i.e. == 1). A TRANSFER transaction must have at least one input (i.e. ≥ 1).

There’s a high-level overview of transaction inputs and outputs in <a href="https://docs.bigchaindb.com/en/latest/transaction-concepts.html" class="reference external">the BigchainDB root docs page about transaction concepts</a>. The IPDB Protocol is modelled around “assets,” and transaction *inputs* and *outputs* are the mechanism by which control of an asset (or shares of an asset) is transferred. Amounts of an asset are encoded in the outputs of a transaction, and each output may be spent separately. To spend an output, the output’s `condition` must be met by an `input` that provides a corresponding `fulfillment`. Each output may be spent at most once, by a single input. Note that any asset associated with an output holding an amount greater than one is considered a divisible asset that may be split up in future transactions.

An input can be implemented as an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> in almost any programming language (e.g. as a dictionary in Python). It has the following basic structure:

    {
       "fulfills": {
          "transaction_id": "<ID of the transaction containing the output being spent>",
          "output_index": index
       },
       "owners_before": ["<The public_keys list in the output being spent>"],
       "fulfillment": "<String that fulfills the condition in the output being spent>"
    }

#### The Keys in a Transaction Input<a href="#the-keys-in-a-transaction-input" class="headerlink" title="Permalink to this headline">¶</a>

**fulfills**

If the transaction is a TRANSFER transaction, then this is like a pointer to the <a href="index.html#outputs" class="reference internal"><span class="std std-ref">output</span></a> being spent/transferred. More specifically, it’s an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> with two key/value pairs:

-   `transaction_id` is the ID of the transaction where the output is located. It’s a string.
-   `output_index` is the index of the output being spent. It’s an integer, *not a string*. Example values are `0`, `1` and `12` (*not* `"0"` or any other string).

An example is:

    {
        "transaction_id": "107ec21f4c53cd2a934941010437ac74882161bcbefdfd7664268823fc347996",
        "output_index": 0
    }

If the transaction is a CREATE transaction, then the value of `fulfills` must be the equivalent of <a href="index.html#term-null" class="reference internal"><span class="xref std std-term">null</span></a> in your programming language, because there is no other transaction output that it’s transferring/spending.

**owners\_before**

If the transaction is a CREATE transaction, then this is a list of public keys (strings): the *issuers* of the asset in question. Those issuers must sign the CREATE transaction, i.e. they must compute the `fulfillment` string using their private keys.

If the transaction is a TRANSFER transaction, then this list must agree with the `public_keys` list in the <a href="index.html#outputs" class="reference internal"><span class="std std-ref">output</span></a> being transferred/spent.

Note

See the <a href="index.html#a-note-about-owners" class="reference internal"><span class="std std-ref">note about “owners”</span></a>.

**fulfillment**

If the transaction is a CREATE transaction, then the fulfillment must fulfill an implicit *n*-of-*n* signature condition, i.e. one signature from each of the *n* `owners_before`. If the transaction is a TRANSFER transaction, then the fulfillment must fulfill the condition in the <a href="index.html#outputs" class="reference internal"><span class="std std-ref">output</span></a> that is being transferred/spent. The page about <a href="index.html#conditions" class="reference internal"><span class="std std-ref">conditions</span></a> explains how to construct a condition object.

The specifics of how to compute a fulfillment for a condition (and the associated fulfillment string) are given in the crypto conditions spec. Consult the <a href="https://tools.ietf.org/html/draft-thomas-crypto-conditions-03" class="reference external">crypto-conditions spec (version 03)</a> or use <a href="https://github.com/rfcs/crypto-conditions#implementations" class="reference external">an existing implementation of crypto-conditions</a>.

The page about <a href="index.html#how-to-construct-a-transaction" class="reference internal"><span class="std std-ref">how to construct a transaction</span></a> gives more details, including a link to example Python code.

Here’s an example fulfillment string:

    "pGSAIDgbT-nnN57wgI4Cx17gFHv3UB_pIeAzwZCk10rAjs9bgUDxyNnXMl-5PFgSIOrN7br2Tz59MiWe2XY0zlC7LcN52PKhpmdRtcr7GR1PXuTfQ9dE3vGhv7LHn6QqDD6qYHYM"

Note

The basic steps to compute a fulfillment string are:

1.  Construct the fulfillment as per the crypto-conditions spec.
2.  Encode the fulfillment to bytes using the <a href="http://www.itu.int/ITU-T/recommendations/rec.aspx?rec=12483&amp;lang=en" class="reference external">ASN.1 Distinguished Encoding Rules (DER)</a>.
3.  Encode the resulting bytes using “base64url” (*not* typical base64) as per <a href="https://tools.ietf.org/html/rfc4648#section-5" class="reference external">RFC 4648, Section 5</a>.

<span id="document-transaction-components/outputs"></span>
### Outputs<a href="#outputs" class="headerlink" title="Permalink to this headline">¶</a>

A list/array/tuple of transaction outputs.

Each output indicates the crypto-conditions which must be satisfied by anyone wishing to spend/transfer that output. It also indicates the number of shares of the asset tied to that output.

An output can be implemented as an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> in almost any programming language (e.g. as a dictionary in Python). It has the following basic structure:

    {
       "condition": condition,
       "public_keys": ["<List of all public keys associated with the condition object>"],
       "amount": "<Number of shares of the asset (an integer in a string)>"
    }

#### The Keys in a Transaction Output<a href="#the-keys-in-a-transaction-output" class="headerlink" title="Permalink to this headline">¶</a>

**condition**

The <a href="index.html#conditions" class="reference internal"><span class="std std-ref">page about conditions</span></a> explains the contents of a condition.

**public\_keys**

A list of public keys (strings) that should be consistent with the public keys in the `condition`.

**amount**

The number of shares of the asset associated with the output in question. It’s an integer converted to a string, e.g. `"7"`.

In a TRANSFER transaction, the sum of the output amounts must be the same as the sum of the outputs that it transfers (i.e. the sum of the input amounts). For example, if a TRANSFER transaction has two outputs, one with `"amount": "2"` and one with `"amount": "3"`, then the sum of the outputs is 5 and so the sum of the outputs-being-transferred must also be 5.

<span id="document-transaction-components/conditions"></span>
### Conditions<a href="#conditions" class="headerlink" title="Permalink to this headline">¶</a>

A condition can be implemented as an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> in almost any programming language (e.g. as a dictionary in Python). It has the following basic structure:

    {
        "details": subcondition,
        "uri": "<Condition URI string>"
    }

#### Subconditions<a href="#subconditions" class="headerlink" title="Permalink to this headline">¶</a>

A subcondition can be implemented as an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a>. In this version of the IPDB Transaction Spec, there are two possible subcondition types:

1.  ED25519-SHA-256
2.  THRESHOLD-SHA-256

Those names are from the <a href="https://tools.ietf.org/html/draft-thomas-crypto-conditions-03" class="reference external">crypto-conditions specification (spec)</a>, which is part of the Interledger Protocol (ILP). (It calls them “crypto-condition types.”) The crypto-conditions spec includes other types, but the above types are the only ones used (currently).

Note

This version of the IPDB Transaction Spec conforms to versions 02 and 03 of the crypto-conditions spec. (The parts that it uses didn’t change from version 02 to 03.)

##### Type 1: ED25519-SHA-256<a href="#type-1-ed25519-sha-256" class="headerlink" title="Permalink to this headline">¶</a>

A subcondition of type ED25519-SHA-256 can be implemented as an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a>. Here’s a JSON example:

    {
       "type": "ed25519-sha-256",
       "public_key": "HFp773FH21sPFrn4y8wX3Ddrkzhqy4La4cQLfePT2vz7"
    }

The `type` must be the string `"ed25519-sha-256"`.

The `public_key` must be a valid public key (string). See the page about <a href="index.html#cryptographic-keys-signatures" class="reference internal"><span class="std std-ref">cryptographic keys and signatures</span></a>.

One can fulfill a (sub)condition of this type by signing a message with the private key corresponding to the given public key.

##### Type 2: THRESHOLD-SHA-256<a href="#type-2-threshold-sha-256" class="headerlink" title="Permalink to this headline">¶</a>

A subcondition of type THRESHOLD-SHA-256 can be implemented as an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a>. It has the following basic structure:

    {
       "type": "threshold-sha-256",
       "threshold": small_int,
       "subconditions": ["<List of subcondition objects>"]
    }

The `type` must be the string `"threshold-sha-256"`.

The `threshold` must be an *integer* *m* between 1 and the number of objects in the `subconditions` list (*n*). It’s *not a string*.

The `subconditions` must be a list of one or more subconditions (*n* associative arrays). Note the recursive definition: a threshold subcondition contains a list of subconditions, some of which may be subconditions of type THRESHOLD-SHA-256.

One can fulfill a (sub)condition of this type by fulfilling *m* of the *n* subconditions. Because of that, it’s also called an *m*-of-*n* threshold condition. An *m*-of-*n* threshold condition can be thought of as a logic gate with n Boolean inputs, where the output is TRUE if, and only if, *m* or more of the inputs are TRUE. Therefore:

-   1-of-*n* is the same as a logical OR of all the inputs, e.g. `1 of {x, y, z} == x OR y OR z`
-   *n*-of-*n* is the same as a logical AND of all the inputs. e.g. `3 of {x, y, z} == x AND y AND z`

#### The URI<a href="#the-uri" class="headerlink" title="Permalink to this headline">¶</a>

If you want to generate a correct condition URI string, then you should consult the <a href="https://tools.ietf.org/html/draft-thomas-crypto-conditions-03" class="reference external">crypto-conditions spec (version 03)</a> or use <a href="https://github.com/rfcs/crypto-conditions#implementations" class="reference external">an existing implementation of crypto-conditions</a>.

There is some example Python 3 code for calculating condition URI strings below.

#### More Complex Conditions<a href="#more-complex-conditions" class="headerlink" title="Permalink to this headline">¶</a>

The (single) output of a threshold condition can be used as one of the inputs to *another* threshold condition. That means you can combine threshold conditions to build complex expressions such as `(x OR y) AND (2 of {a, b, c})`.

![](_images/Conditions_Circuit_Diagram.png)

#### Cost of a Condition<a href="#cost-of-a-condition" class="headerlink" title="Permalink to this headline">¶</a>

When you create a condition, you can calculate its <a href="https://tools.ietf.org/html/draft-thomas-crypto-conditions-03#section-7.2.2" class="reference external">cost</a>, an estimate of the resources that would be required to validate the fulfillment. For example, the cost of one ED25519-SHA-256 condition is 131072.

An implementation of an IPDB server may choose to put an upper limit on the complexity of each condition, either directly by setting a maximum allowed cost, or indirectly by setting a maximum allowed transaction size.

#### Example Conditions<a href="#example-conditions" class="headerlink" title="Permalink to this headline">¶</a>

##### The Simplest Possible Condition<a href="#the-simplest-possible-condition" class="headerlink" title="Permalink to this headline">¶</a>

The simplest possible condition is one with a single ED25519-SHA-256 signature (sub)condition. Here’s a JSON example:

    {
        "details": {
            "type": "ed25519-sha-256",
            "public_key": "HFp773FH21sPFrn4y8wX3Ddrkzhqy4La4cQLfePT2vz7"
        },
        "uri": "ni:///sha-256;at0MY6Ye8yvidsgL9FrnKmsVzX0XrNNXFmuAPF4bQeU?fpt=ed25519-sha-256&cost=131072"
    }

**Example Python 3 Code to Compute the Condition URI**

    import base58
    from cryptoconditions import Ed25519Sha256

    pubkey = 'HFp773FH21sPFrn4y8wX3Ddrkzhqy4La4cQLfePT2vz7'

    # Convert pubkey to a bytes representation (a Python 3 bytes object)
    pubkey_bytes = base58.b58decode(pubkey)

    # Construct the condition object
    ed25519 = Ed25519Sha256(public_key=pubkey_bytes)

    # Compute the condition uri (string)
    uri = ed25519.condition_uri
    # uri should be:
    # 'ni:///sha-256;at0MY6Ye8yvidsgL9FrnKmsVzX0XrNNXFmuAPF4bQeU?fpt=ed25519-sha-256&cost=131072'

##### A 2-of-2 Condition<a href="#a-2-of-2-condition" class="headerlink" title="Permalink to this headline">¶</a>

Here’s an example 2-of-2 condition (JSON):

    {
        "details": {
            "type": "threshold-sha-256",
            "threshold": 2,
            "subconditions": [
                {
                    "public_key": "5ycPMinRx7D7e6wYXLNLa3TCtQrMQfjkap4ih7JVJy3h",
                    "type": "ed25519-sha-256"
                },
                {
                    "public_key": "9RSas2uCxR5sx1rJoUgcd2PB3tBK7KXuCHbUMbnH3X1M",
                    "type": "ed25519-sha-256"
                 }
             ]
         },
         "uri": "ni:///sha-256;zr5oThl2kk6613WKGFDg-JGu00Fv88nXcDcp6Cyr0Vw?fpt=threshold-sha-256&cost=264192&subtypes=ed25519-sha-256"
    }

**Example Python 3 Code to Compute the Condition URI**

    import base58
    from cryptoconditions import Ed25519Sha256, ThresholdSha256

    pubkey1 = '5ycPMinRx7D7e6wYXLNLa3TCtQrMQfjkap4ih7JVJy3h'
    pubkey2 = '9RSas2uCxR5sx1rJoUgcd2PB3tBK7KXuCHbUMbnH3X1M'

    # Convert pubkeys to bytes representations (Python 3 bytes objects)
    pubkey1_bytes = base58.b58decode(pubkey1)
    pubkey2_bytes = base58.b58decode(pubkey2)

    # Construct the condition object
    ed25519_1 = Ed25519Sha256(public_key=pubkey1_bytes)
    ed25519_2 = Ed25519Sha256(public_key=pubkey2_bytes)
    threshold_sha256 = ThresholdSha256(threshold=2)
    threshold_sha256.add_subfulfillment(ed25519_1)
    threshold_sha256.add_subfulfillment(ed25519_2)

    # Compute the condition uri (string)
    uri = threshold_sha256.condition.serialize_uri()
    # uri should be:
    # 'ni:///sha-256;zr5oThl2kk6613WKGFDg-JGu00Fv88nXcDcp6Cyr0Vw?fpt=threshold-sha-256&cost=264192&subtypes=ed25519-sha-256'

To change it into a 1-of-2 condition, just change the value of `threshold` to 1 and recompute the condition URI.

<span id="document-transaction-components/operation"></span>
### Operation<a href="#operation" class="headerlink" title="Permalink to this headline">¶</a>

The operation indicates the type/kind of transaction, and how it should be validated. It must be a string. The allowed values are `"CREATE"` and `"TRANSFER"`.

Note

Some implementations may allow other values, but maybe only internally. For example, BigchainDB Server allows the value `"GENESIS"`. See <a href="index.html#implementation-specific-deviations" class="reference internal"><span class="std std-ref">the page about implementation-specific deviations</span></a>.

<span id="document-transaction-components/asset"></span>
### Asset<a href="#asset" class="headerlink" title="Permalink to this headline">¶</a>

In a CREATE transaction, an asset can be the equivalent of <a href="index.html#term-null" class="reference internal"><span class="xref std std-term">null</span></a> or an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> containing exactly one key-value pair. The key must be `"data"` and the value can be any valid associative array. Here’s a JSON example:

    {
       "data": {
                  "desc": "Gold-inlay bookmark owned by Xavier Bellomat Dickens III",
                  "xbd_collection_id": 1857
               }
    }

The meaning of a “valid associative array” may depend on the implementation; see the page about <a href="index.html#implementation-specific-deviations" class="reference internal"><span class="std std-ref">implementation-specific deviations</span></a>.

In a TRANSFER transaction, an asset can be the equivalent of <a href="index.html#term-null" class="reference internal"><span class="xref std std-term">null</span></a> or an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> containing exactly one key-value pair. The key must be `"id"` and the value must be a 64-character hex string: a <a href="index.html#transaction-id" class="reference internal"><span class="std std-ref">transaction ID</span></a>. Here’s a JSON example:

    {
       "id": "38100137cea87fb9bd751e2372abb2c73e7d5bcf39d940a5516a324d9c7fb88d"
    }

<span id="document-transaction-components/metadata"></span>
### Metadata<a href="#metadata" class="headerlink" title="Permalink to this headline">¶</a>

User-provided transaction metadata.

It can be any valid <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a>, or the equivalent of <a href="index.html#term-null" class="reference internal"><span class="xref std std-term">null</span></a> in your programming language. The meaning of a “valid associative array” may depend on the implementation; see the page about <a href="index.html#implementation-specific-deviations" class="reference internal"><span class="std std-ref">implementation-specific deviations</span></a>. Here’s a JSON example:

    {
       "timestamp": "1510850314",
       "conditions": "blustery and humid",
       "coordinates": {"x": "45", "y": "19"}
    }

<span id="document-construct-a-tx"></span>
How to Construct a Transaction<a href="#how-to-construct-a-transaction" class="headerlink" title="Permalink to this headline">¶</a>
-----------------------------------------------------------------------------------------------------------------------------------

This page lists the steps to construct a valid transaction.

1.  Set a variable named `version` to a <a href="index.html#version" class="reference internal"><span class="std std-ref">valid version value</span></a>.

2.  Set a variable named `operation` to a <a href="index.html#operation" class="reference internal"><span class="std std-ref">valid operation value</span></a>.

3.  Set a variable named `asset` to a <a href="index.html#asset" class="reference internal"><span class="std std-ref">valid asset</span></a>.

4.  Set a variable named `metadata` to a <a href="index.html#metadata" class="reference internal"><span class="std std-ref">valid metadata</span></a>.

5.  Generate or get all the required <a href="index.html#cryptographic-keys-signatures" class="reference internal"><span class="std std-ref">public keys</span></a>.

6.  Construct a list/array/tuple named `outputs` of all the <a href="index.html#outputs" class="reference internal"><span class="std std-ref">outputs</span></a> that should be in the transaction. (Note: Each output includes a <a href="index.html#conditions" class="reference internal"><span class="std std-ref">condition</span></a>.)

7.  Construct a list/array/tuple named `unfulfilled_inputs` of all the <a href="index.html#inputs" class="reference internal"><span class="std std-ref">inputs</span></a> that should be in the transaction. All fulfillment strings should be set to the equivalent of <a href="index.html#term-null" class="reference internal"><span class="xref std std-term">null</span></a> in your programming language. (We’re building an “unfulfilled transaction” first.)

8.  Construct an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> named `unfulfilled_tx` of the form (based on JSON syntax):

    >     {
    >         "id": null,
    >         "version": version,
    >         "inputs": unfulfilled_inputs,
    >         "outputs": outputs,
    >         "operation": operation,
    >         "asset": asset,
    >         "metadata": metadata
    >       }
    >
    > Note how `unfulfilled_tx` includes a key-value pair for the `"id"` key. The value must be your programming language’s equivalent of <a href="index.html#term-null" class="reference internal"><span class="xref std std-term">null</span></a>.

9.  <a href="index.html#json-serialization-deserialization" class="reference internal"><span class="std std-ref">Convert unfulfilled_tx to an IPDB-standard JSON string</span></a> named `utx_json`.

10. Create `inputs` as a deep copy of `unfulfilled_inputs`.

11. For each input in `inputs`:

    > 1.  If `fulfills` is <a href="index.html#term-null" class="reference internal"><span class="xref std std-term">null</span></a>, let `string1 = utx_json`, otherwise let `string1 = utx_json + output_tx_id + output_index` where `output_tx_id` is the transaction ID of the output that this input fulfills and `+` means concatenate the strings.
    > 2.  <a href="index.html#converting-strings-to-bytes" class="reference internal"><span class="std std-ref">Convert string1 to bytes</span></a> and call the result `bytes1`.
    > 3.  <a href="index.html#cryptographic-hashes" class="reference internal"><span class="std std-ref">Compute the SHA3-256 hash</span></a> of `bytes1` and leave the result as bytes (i.e. don’t convert to a hex string). Call the result `bytes_to_sign`.
    > 4.  fulfill the associated crypto-condition <a href="https://github.com/rfcs/crypto-conditions#implementations" class="reference external">using an implementation of crypto-conditions</a>. You will need `bytes_to_sign` and one or more private keys (which are used to sign `bytes_to_sign`). The end result is usually some kind of fulfilled condition object. Compute the fulfillment string of that fulfilled condition object, and put that as the value of `"fulfillment"` for the input in question.

12. Construct a new <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> `tx` by making a deep copy of `unfulfilled_tx`.

13. In `tx`, change the value of `"inputs"` to the just-computed `inputs` (an array of fulfilled inputs).

14. <a href="index.html#transaction-id" class="reference internal"><span class="std std-ref">Compute the transaction ID of tx</span></a>. Call it `computed_id`.

15. In `tx`, change the value of `"id"` to `computed_id`.

The final result (`tx`) is a valid fulfilled transaction (in the form of an associative array). To put it in the body of an HTTP POST request, you’ll have to <a href="index.html#json-serialization-deserialization" class="reference internal"><span class="std std-ref">convert it to a JSON string</span></a>.

**Example Python Code**

The documentation of the BigchainDB Python Driver has a page titled <a href="https://docs.bigchaindb.com/projects/py-driver/en/latest/handcraft.html" class="reference external">“Handcrafting Transactions”</a> which shows how to do all of the above in Python (using a Python implementation of crypto-conditions).

<span id="document-common-operations/index"></span>
Common Operations<a href="#common-operations" class="headerlink" title="Permalink to this headline">¶</a>
---------------------------------------------------------------------------------------------------------

<span id="document-common-operations/json-serialization"></span>
### JSON Serialization & Deserialization<a href="#json-serialization-deserialization" class="headerlink" title="Permalink to this headline">¶</a>

In the IPDB Transaction Spec, “JSON serialization” is the standard process to convert an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a> (such as a Python dict) to a standard Unicode JSON string. “JSON deserialization” is the reverse. In the IPDB Transaction Spec, some constraints are imposed on the JSON string:

-   All keys must be strings
-   The separator after each key is `:` with no spaces on either side.
-   The separator after each value (except the last one) is `,` with no spaces on either side and no newline or carriage return.
-   The string is Unicode (not just ASCII)
-   In the JSON string, all keys are sorted by key name (because associative arrays don’t have an implicit order, but we need there to be only *one* JSON string associated with a given associative array)

There are several JSON standards, notably RFC 7159 and ECMA-404.

For JSON serialization and deserialization, the IPDB Transaction Spec follows what RapidJSON does. <a href="https://github.com/Tencent/rapidjson" class="reference external">RapidJSON</a> is a fast C++ JSON library. According to <a href="http://rapidjson.org/" class="reference external">the RapidJSON documentation</a>, “RapidJSON should be in full compliance with RFC7159/ECMA-404, with optional support of relaxed syntax.”

Most common programming languages have one or more libraries/packages which can do the same as RapidJSON.

**Example Python 3 Code**

There’s a Python 3 wrapper around RapidJSON called <a href="https://github.com/python-rapidjson/python-rapidjson" class="reference external">python-rapidjson</a>. In Python, associative arrays are implemented as dictionaries. To convert a dictionary to a standard Unicode JSON string (Python 3 <a href="https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str" class="reference external">str object</a>, standard in the sense of the IPDB Transaction Spec):

    import rapidjson

    # input_dict is a dictionary
    json_str = rapidjson.dumps(input_dict,
                               skipkeys=False,
                               ensure_ascii=False,
                               sort_keys=True)

-   `skipkeys=False` ensures all keys are strings. If they’re not, the serialization will fail.
-   `ensure_ascii=False` allows non-ASCII Unicode characters to pass through into the resulting Python 3 <a href="https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str" class="reference external">str object</a>.
-   `sort_keys=True` ensures the JSON output is sorted by key.

The python-rapidjson documentation has a <a href="https://python-rapidjson.readthedocs.io/en/latest/dumps.html" class="reference external">page about the dumps() function</a>.

To deserialize a standard Unicode JSON string to a dictionary:

    new_dict = rapidjson.loads(json_str)

<span id="document-common-operations/string-to-bytes"></span>
### Converting Strings to Bytes<a href="#converting-strings-to-bytes" class="headerlink" title="Permalink to this headline">¶</a>

Most common programming languages have some way to convert a Unicode string to bytes. To do that, one must specify the encoding; in the case of the IPDB Transaction Spec, the encoding must be <a href="https://en.wikipedia.org/wiki/UTF-8" class="reference external">UTF-8</a>.

**Example Python 3 Code**

If `example_string` is a Python 3 <a href="https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str" class="reference external">str object</a> (an immutable sequence of Unicode code points), then it can be converted to a Python 3 <a href="https://docs.python.org/3/library/stdtypes.html#bytes-objects" class="reference external">bytes object</a> using:

    example_bytes = example_str.encode()

The Python 3 <a href="https://docs.python.org/3/library/stdtypes.html#str.encode" class="reference external">str.encode() method</a> assumes a UTF-8 encoding by default.

<span id="document-common-operations/crypto-hashes"></span>
### Cryptographic Hashes<a href="#cryptographic-hashes" class="headerlink" title="Permalink to this headline">¶</a>

#### IPDB-Standard Hashes<a href="#ipdb-standard-hashes" class="headerlink" title="Permalink to this headline">¶</a>

When computing a cryptographic hash (such as the <a href="index.html#transaction-id" class="reference internal"><span class="std std-ref">Transaction ID</span></a>), and *not* falling back to some other protocol (such as crypto-conditions) to specify how the hash should be computed, the computed hash must be a NIST-standard SHA3-256 hash.

Warning

During the finalization of SHA3, NIST changed the delimiter suffix from 0x01 to 0x06. Older SHA3 libraries might use the old suffix, so make sure you use an up-to-date library when calculating SHA3-256 hashes.

A SHA3-256 hash can be represented as a sequence of 256 bits, 32 bytes, or many other ways. When representing SHA3-256 hashes as strings (e.g. inside transactions), they must be represented with a hexadecimal encoding: a sequence of hexadecimal digits (0–9 and a–f). Every byte can be represented by two hexadecimal digits so the hexadecimal string should have 64 characters. An example is:

`"ee788e85a9b5ae9aa9af4fe71458e8b3b72d2e0f290f3e6bc0bdaa262b60a860"`

**Example Python 3 Code**

Install the package pysha3 1.0 or greater (from PyPI). It’s a wrapper around <a href="https://github.com/gvanas/KeccakCodePackage" class="reference external">the optimized Keccak Code Package (KCP)</a>. The following code snippet calculates the SHA3-256 hash of the input `json_bytes` (a Python 3 <a href="https://docs.python.org/3/library/stdtypes.html#bytes-objects" class="reference external">bytes object</a>) and then converts it to a hexadecimal string (a Python 3 <a href="https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str" class="reference external">str object</a>).

    import sha3

    # json_bytes is a bytes object
    hash_as_hex_string = sha3.sha3_256(json_bytes).hexdigest()

Note: `sha3.sha3_256(json_bytes)` is an intermediate object of class ‘\_pysha3.sha3\_256’.

#### Computing the Hash of an Associative Array<a href="#computing-the-hash-of-an-associative-array" class="headerlink" title="Permalink to this headline">¶</a>

There’s an IPDB-standard way to compute the hash of an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a>. We’ve called that function `hash_of_aa()` elsewhere in this documentation. It takes an associative array `d` as input and returns a string as output. Here is what that function must do:

1.  Convert `d` to a standard Unicode JSON string. See the page about <a href="index.html#json-serialization-deserialization" class="reference internal"><span class="std std-ref">JSON serialization and deserialization</span></a>. Call the resulting string `d_json`.
2.  Convert `d_json` to bytes (i.e. a sequence of bytes). See the page about <a href="index.html#converting-strings-to-bytes" class="reference internal"><span class="std std-ref">converting strings to bytes</span></a>. Call the resulting bytes `d_bytes`.
3.  Compute the SHA3-256 hash of `d_bytes` as outlined at the top of this page, and represent the hash as a hexadecimal string.

<span id="document-common-operations/crypto-keys-and-sigs"></span>
### Cryptographic Keys & Signatures<a href="#cryptographic-keys-signatures" class="headerlink" title="Permalink to this headline">¶</a>

The IPDB Transaction Spec uses the <a href="https://ed25519.cr.yp.to/" class="reference external">Ed25519</a> public-key signature system for:

-   generating public/private key pairs (also called verifying/signing key pairs),
-   calculting signatures, and
-   verifying signatures.

Ed25519 is an instance of the <a href="https://en.wikipedia.org/wiki/EdDSA" class="reference external">Edwards-curve Digital Signature Algorithm (EdDSA)</a>. There’s more information about EdDSA and Ed25519 in <a href="https://tools.ietf.org/html/rfc8032" class="reference external">RFC 8032</a>.

When representing public/private keys as strings (e.g. inside transactions), they must be represented with a <a href="https://en.wikipedia.org/wiki/Base58" class="reference external">Base58 encoding</a>.

Warning

There is no standard for Base58 encoding. The meaning of Base58 varies from library to library. The Base58 encoding used by the IPDB Transaction Spec is that which is implemented by <a href="https://pypi.python.org/pypi/base58" class="reference external">the Python package named base58</a>, which is the same as what’s used for Bitcoin addresses.

Here’s an example Ed25519 public/private key pair:

    {
       "public": "9WYFf8T65bv4S8jKU8wongKPD4AmMZAwvk1absFDbYLM",
       "private": "3x7MQpPq8AEUGEuzAxSVHjU1FhLWVQJKFNNkvHhJPGCX"
    }

To sign a message, one uses a private key, i.e. signature = signing\_function(message, private\_key). To verify a signature, one needs the corresponding public key, i.e. signature\_is\_valid = verifying\_function(message, public\_key, signature). The signing\_function and verifying\_function are provided by the Ed25519 signature system. The reference implementation of Ed25519 is in the <a href="https://nacl.cr.yp.to/" class="reference external">Networking and Cryptography Library (NaCl)</a>, but there are <a href="https://ianix.com/pub/ed25519-deployment.html" class="reference external">many other implementations</a>.

When representing calculated *signatures* as strings (e.g. inside blocks), they must be represented with a <a href="https://en.wikipedia.org/wiki/Base58" class="reference external">Base58 encoding</a>. Here’s an example signature:

    "8Z6GJFLSvHmWVqN4dJHshcamNR3cYMwsk9bKScjd32ZgMEtbVSrujHDqrPpdyzBo3tpdse4N4YHXZGXdHfjZZhH"

The keys and signatures that go into <a href="index.html#outputs" class="reference internal"><span class="std std-ref">outputs</span></a> and <a href="index.html#inputs" class="reference internal"><span class="std std-ref">inputs</span></a> follow the <a href="https://tools.ietf.org/html/draft-thomas-crypto-conditions-03" class="reference external">crypto-conditions specification</a>. However, the IPDB Transaction Spec only allows for Ed25519 keys and signatures. (It doesn’t allow for the RSA ones which are also part of the crypto-conditions specification.) The Ed25519 functions used to generate keys, calculate signatures, and verify signatures are the *same* across the IPDB Transaction Spec. Those calculations aren’t done differently inside inputs or outputs.

**Example Python 3 Code**

The Python package <a href="https://pypi.python.org/pypi/BigchainDB" class="reference external">BigchainDB</a> is a Python 3 reference implementation of an IPDB-compliant server. Its source code is in the <a href="https://github.com/bigchaindb/bigchaindb/" class="reference external">bigchaindb/bigchaindb</a> repository on GitHub. There you can see how it generates public/private key pairs, calculates signatures, and verifies signatures: it uses the <a href="https://github.com/bigchaindb/cryptoconditions" class="reference external">cryptoconditions package</a>. The cryptoconditions package, in turn, uses the <a href="https://pypi.python.org/pypi/PyNaCl" class="reference external">PyNaCl package</a>, a Python binding to <a href="https://github.com/jedisct1/libsodium" class="reference external">libsodium</a>, which is a fork of the Networking and Cryptography library.

#### Computing the Signature of an Associative Array<a href="#computing-the-signature-of-an-associative-array" class="headerlink" title="Permalink to this headline">¶</a>

There’s an IPDB-standard way to compute the signature of an <a href="index.html#term-associative-array" class="reference internal"><span class="xref std std-term">associative array</span></a>. We’ve called that function `sig_of_aa()` elsewhere in this documentation. It takes two inputs: an associative array `d` and a `private_key`. It returns a signature string as output. Here is what that function must do:

1.  Convert `d` to a standard Unicode JSON string. See the page about <a href="index.html#json-serialization-deserialization" class="reference internal"><span class="std std-ref">JSON serialization and deserialization</span></a>. Call the resulting string `d_json`.
2.  Convert `d_json` to bytes (i.e. a sequence of bytes). See the page about <a href="index.html#converting-strings-to-bytes" class="reference internal"><span class="std std-ref">converting strings to bytes</span></a>. Call the resulting bytes `d_bytes`.
3.  Calculate the Ed25519 signature of `d_bytes` using the given `private_key`.

<span id="document-transaction-validation"></span>
Transaction Validation<a href="#transaction-validation" class="headerlink" title="Permalink to this headline">¶</a>
-------------------------------------------------------------------------------------------------------------------

If a transaction satisfies the constraints (or rules) listed below, then it is considered valid. The process of checking those constraints is called transaction validation.

Each version X.Y.Z of the IPDB Transaction Spec may have different constraints. That is, the constraints may change from one version to the next.

### How Validation Code Decides Which Version to Use<a href="#how-validation-code-decides-which-version-to-use" class="headerlink" title="Permalink to this headline">¶</a>

When given a transaction to validate, validation code should check the value of `"version"` inside the transaction. The valid values are those with a corresponding set of JSON Schema files (which can be found in the `tx_schema` directory of the <a href="https://github.com/ipdb/ipdb-tx-spec" class="reference external">IPDB Transaction Spec repository</a>). If `"version"` doesn’t have a valid value, then the transaction is invalid. Otherwise, the transaction should be validated according to the validation constraints described in that version of the IPDB Transaction Spec.

### JSON Schema Validation<a href="#json-schema-validation" class="headerlink" title="Permalink to this headline">¶</a>

JSON Schema Validation is done by checking the transaction against a formal <a href="http://json-schema.org/" class="reference external">JSON Schema</a> defined in a set of <a href="http://json-schema.org/" class="reference external">JSON Schema</a> files. Those files can be found in the `tx_schema/` directory of the <a href="https://github.com/ipdb/ipdb-tx-spec" class="reference external">IPDB Transaction Spec repository</a>.

Note

Tip 1: There’s a nice explanation of JSON Schema in the website <a href="https://spacetelescope.github.io/understanding-json-schema/index.html" class="reference external">“Understanding JSON Schema”</a>.

Tip 2: Python 3 code for checking a transaction against JSON Schema files can be found in the <a href="https://github.com/bigchaindb/bigchaindb" class="reference external">source code of BigchainDB Server</a>. At the time of writing, it was in the file `bigchaindb/common/schema/__init__.py`.

### Other Constraints<a href="#other-constraints" class="headerlink" title="Permalink to this headline">¶</a>

#### The output.amount Rule<a href="#the-output-amount-rule" class="headerlink" title="Permalink to this headline">¶</a>

For all outputs, once output.amount has been converted from a string to an integer, it must be between 1 and 9×10^18, inclusive. The reason for the upper bound is to keep amount within what a server can comfortably represent using a 64-bit signed integer, i.e. 9×10^18 is less than 2^63.

#### The Duplicate Transaction Rule<a href="#the-duplicate-transaction-rule" class="headerlink" title="Permalink to this headline">¶</a>

If a transaction is a duplicate of a previous transaction, then it’s invalid. A quick way to check that is by checking to see if a transaction with the same transaction ID is already stored.

#### The Transaction ID Rule<a href="#the-transaction-id-rule" class="headerlink" title="Permalink to this headline">¶</a>

The transaction ID (id) must be valid in that it must agree with the hash computed using the instructions given on <a href="index.html#transaction-id" class="reference internal"><span class="std std-ref">the page about transaction ID</span></a>.

#### The TRANSFER Transaction Rules<a href="#the-transfer-transaction-rules" class="headerlink" title="Permalink to this headline">¶</a>

If a transaction is a TRANSFER transaction:

1.  If an input attempts to fulfill an output that has already been fulfilled (i.e. spent or transferred) by a previous valid transaction, then the transaction is invalid. (You don’t have to check if the fulfillment string is valid.)
2.  If two or more inputs (in the transaction being validated) attempt to fulfill the *same* output, then the transaction is invalid. (You don’t have to check any fulfillment strings.)
3.  The sum of the amounts on the inputs must equal the sum of the amounts on the outputs. In other words, a TRANSFER transaction can’t create or destroy asset shares.
4.  For all inputs, if input.fulfills points to:  
    -   a transaction that doesn’t exist, then it’s invalid.
    -   a transaction that’s invalid, then it’s invalid. (This check may be skipped if invalid transactions are never kept.)
    -   a transaction output that doesn’t exist, then it’s invalid.
    -   a transaction with an asset ID that’s different from *this* transaction’s asset ID, then this transaction is invalid. (The asset ID of a CREATE transaction is the same as the transaction ID. The asset ID of a TRANSFER transaction is asset.id.)

Note: The first two rules prevent double spending.

#### The input.fulfillment Rule<a href="#the-input-fulfillment-rule" class="headerlink" title="Permalink to this headline">¶</a>

Regardless of whether the transaction is a CREATE or TRANSFER transaction: For all inputs, input.fulfillment must be valid. See <a href="index.html#inputs" class="reference internal"><span class="std std-ref">the page about inputs</span></a> for more details about what that means.

<span id="document-example-transactions"></span>
Example Transactions<a href="#example-transactions" class="headerlink" title="Permalink to this headline">¶</a>
---------------------------------------------------------------------------------------------------------------

You can find examples of BigchainDB transactions in:

-   <a href="https://docs.bigchaindb.com/projects/py-driver/en/latest/index.html" class="reference external">the Python driver documentation</a>
-   <a href="https://docs.bigchaindb.com/projects/js-driver/en/latest/index.html" class="reference external">the JavaScript driver documentation</a>
-   <a href="https://docs.bigchaindb.com/projects/server/en/latest/http-client-server-api.html" class="reference external">the docs about the HTTP API in the docs for BigchainDB Server</a>

Just remember to check the value of `"version"` in the transaction.

<span id="document-implementation-deviations"></span>
Implementation-Specific Deviations<a href="#implementation-specific-deviations" class="headerlink" title="Permalink to this headline">¶</a>
-------------------------------------------------------------------------------------------------------------------------------------------

Some implementations of IPDB-compliant servers or drivers deviate from the IPDB Transaction Spec. This page summarizes those deviations.

### BigchainDB Server<a href="#bigchaindb-server" class="headerlink" title="Permalink to this headline">¶</a>

<a href="https://github.com/bigchaindb/bigchaindb" class="reference external">BigchainDB Server</a> is an IPDB-compliant server implemented in Python.

It allows <a href="index.html#operation" class="reference internal"><span class="std std-ref">operation</span></a> to have the value `"GENESIS"`, but only for transactions in the GENESIS block.

#### BigchainDB Server with MongoDB<a href="#bigchaindb-server-with-mongodb" class="headerlink" title="Permalink to this headline">¶</a>

When BigchainDB Server is used *with MongoDB*, it inherits some quirks from MongoDB:

-   All key names (e.g. anywhere in the JSON documents stored in `asset.data` or `metadata`):

    > -   must not begin with `$`
    > -   must not contain `.`
    > -   must not contain the <a href="https://en.wikipedia.org/wiki/Null_character" class="reference external">null character</a> (Unicode code point U+0000)

-   If there’s a key named `"language"` (e.g. anywhere in the JSON documents stored in `asset.data` or `metadata`), then its value must be one of the <a href="https://docs.mongodb.com/manual/reference/text-search-languages/" class="reference external">supported values (language codes)</a>, because MongoDB uses that to guide its full text search. Moreover, BigchainDB Server only allows the language codes supported by *MongoDB Community Edition* (not MongoDB Enterprise).

<span id="document-ownership"></span>
A Note about Owners<a href="#a-note-about-owners" class="headerlink" title="Permalink to this headline">¶</a>
-------------------------------------------------------------------------------------------------------------

The public keys associated with an unspent output are sometimes called the “owners” of the associated shares in an asset, but the legal entities associated with those public keys may or may not be “owners” in any legal sense. The most that *can* be said is that those public keys are associated with the ability to fulfill the conditions on the output.

External contracts or other legal agreements may establish stronger interpretations in specific cases.

<span id="document-glossary"></span>
Glossary<a href="#glossary" class="headerlink" title="Permalink to this headline">¶</a>
---------------------------------------------------------------------------------------

associative array  
A collection of key/value (or name/value) pairs such that each possible key appears at most once in the collection. In JavaScript (and JSON), all objects behave as associative arrays with string-valued keys. In Python and .NET, associative arrays are called *dictionaries*. In Java and Go, they are called *maps*. In Ruby, they are called *hashes*. See also: Wikipedia’s articles for <a href="https://en.wikipedia.org/wiki/Associative_array" class="reference external">Associative array</a> and <a href="https://en.wikipedia.org/wiki/Comparison_of_programming_languages_(associative_array)" class="reference external">Comparison of programming languages (associative array)</a>

null  
`null` in JavaScript, Java and C\#. `None` in Python. `nil` in Ruby and Go. If it’s a value in an associative array and you convert it to a JSON string, it should convert to `null` (with no quotes around it).

------------------------------------------------------------------------

© Copyright 2017, IPDB Contributors.

Built with [Sphinx](http://sphinx-doc.org/) using a [theme](https://github.com/snide/sphinx_rtd_theme) provided by [Read the Docs](https://readthedocs.org).
