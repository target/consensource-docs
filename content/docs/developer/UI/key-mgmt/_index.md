---
title: "Key Management"
weight: 2
description: >
  Overview of how private and public keys are managed in the UI
---

### Private & Public Key Overview

ConsenSource utilizes [_public-key cryptography_](https://www.blockchain-council.org/blockchain/how-does-blockchain-use-public-key-cryptography/). Public keys are stored _on-chain_ and accessible to all nodes in the network, and a user's private key is stored in an encrypted format _off-chain_.

Whenever a user creates a transaction, the contents of the payload are serialized, signed with the user's private key, and committed with the transaction. This signature can later be decrypted with the user's public key to verify that the contents of the payload are unchanged, and to prove that a given user created a transaction.

**Private and Public keys** in ConsenSource are generated with [**secp256k1** using the **ECDSA** algorithm](http://www.secg.org/sec2-v2.pdf). When storing private keys in a database, the [SJCL encyrption library](https://bitwiseshiftleft.github.io/sjcl/) is used.

#### Sawtooth Docs

- [Creating Private and Public Keys](https://sawtooth.hyperledger.org/docs/core/releases/1.0/_autogen/txn_submit_tutorial.html?highlight=secp256k1#creating-private-and-public-keys)

### Key Management

#### Browser Storage

Browser storage is used to store the public and private keys of a user.

_Local Storage_:

- Stores a User object containing the following fields
  - username, public_key, name, email, encrypted_private_key

_Session Storage_:

- Stores the _decrypted_ private key

#### User Creation

When filling out the Sign Up form, a User object is created along with a corresponding Agent in order to store the hashed password and encrypted private key. The Agent is stored _on-chain_, but the User is stored _off-chain_.

The User is fetched from the database when signing in to ConsenSource. The User object is then saved to _local storage_, and the decrypted private key is saved to _session storage_.

{{<alert>}}
<b>Takeaway</b><br/>
A _User_ is an off-chain object that is used to store the hashed password and encrypted private key of a user.
{{</alert>}}

The diagram below goes into more detail on the user creation process.

{{< imgproc user-creation Fill "2100x1200" >}}
User creation workflow in ConsenSource
{{< /imgproc >}}
