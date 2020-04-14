---
title: "Transaction & Batch Building"
weight: 3
description: >
  Explanation of the transaction & batch building processes
---

### Transactions & Batches In Sawtooth

In Hyperledger Sawtooth, the state of the blockchain can only be modified through **transactions**. As a user interacts with the application, transactions are constructed. The contents of a transaction are signed with the user's _private key_ in an irreversible, one-way hashing function ([secp256k1 ECDSA](https://sawtooth.hyperledger.org/docs/core/releases/1.0/_autogen/txn_submit_tutorial.html?highlight=secp256k1#creating-private-and-public-keys)).

The resulting signature can be decoded with the user's _public key_. This decoding process is used to verify that a transaction was not modified after being signed by the user.

Transactions are then wrapped into a **batch**. Batches are the atomic unit of change in Sawtooth - either all of the transactions in a batch are committed together, or the entire batch is rejected. A batch is sent from the UI to the API, which is then forwarded to a Sawtooth validator node that verifies the signature and payload of each transaction before updating the blockchain state.

{{< alert>}}
<b>Takeaway</b><br/>
The state of the ConsenSource blockchain is updated by _transactions_ wrapped in a _batch_ that get sent to a validator node. The UI constructs and signs transactions on behalf of a user.
{{< /alert>}}

The diagram below provides an overview of the sturcture of transactions and batches in Sawtooth.

{{< imgproc txn-batch-overview Fill "500x400" >}}
Transaction and Batch relationship
{{< /imgproc >}}

### Building and Submitting Transactions

#### Transaction Family

The ConsenSource Transaction Family is designed to model the existing business processes that occur when a factory receives a new certification. View the full specification at the link below.

**[Transaction Processor & Family Documentation](/consensource-docs/docs/developer/txn-processor/)**

<!-- #### Signing Transactions -->

### Additional Sawtooth Docs

- [Transactions and Batches](https://sawtooth.hyperledger.org/docs/core/releases/1.0/architecture/transactions_and_batches.html#transactions-and-batches)
- [Addressing & Namespace Design](https://sawtooth.hyperledger.org/docs/core/releases/1.0/app_developers_guide/address_and_namespace.html#address-and-namespace-design)
- [Building and Submitting Transactions](https://sawtooth.hyperledger.org/docs/core/nightly/1-1/_autogen/txn_submit_tutorial.html#building-and-submitting-transactions)
