# Changes from v1.0 to v2.0

## Hash-then-Fulfill to Fulfill-then-Hash

In v1.0, the transaction ID was computed first, by hashing the transaction:

- with no "id" key/value pair included at all
- with all fulfillment values set to `null`

Fulfillments (signatures) were then computed using "id" set to the already-computed transaction ID.

In v2.0, all signatures (inputs) are computed first, and the message-to-sign includes an "id" key/value pair in which the value is `null`. The transaction ID is just the hash of the *fulfilled* transaction (with "id" value set to `null`).

For the reasoning, see [bigchaindb/bigchaindb#1891](https://github.com/bigchaindb/bigchaindb/issues/1891).

## A Change in the Message-to-Sign

In v1.0, when computing the `fulfillment` for each input, the message-to-sign was the *same* for each one (and it included the transaction ID).

In v2.0, when computing the `fulfillment` for each input, the message-to-sign *differs* for each one (and uses a transaction `"id"` value of `null`). See the spec for the details.

For the reasoning, see [bigchaindb/bigchaindb#1937](https://github.com/bigchaindb/bigchaindb/pull/1937).