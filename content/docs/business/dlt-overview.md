---
title: "Distributed Ledger Technology (DLT) Overview"
weight: 3
description: 
    An overview of Distributed Ledger Technology/Blockchain and how it is leveraged in ConsenSource
---

## What Is A Blockchain?

A blockchain is a network in which a record of transactions are maintained across several computers that are linked in a peer-to-peer network. All nodes in this network reach consensus on what the state of the ledger should look like before adding to it. This enforces a level of  trust and transparency regarding the authenticity and immutability of data within the ledger.

Blockchains are a particular type of distributed ledger technology (DLT), and as such, we have elected to use the broader term, DLT, as we further develop ConsenSource.

<iframe width="560" height="315" src="https://www.youtube.com/embed/3xGLc-zz9cA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### What Is A Permissioned Blockchain?

Blockchains can be further subdivided into two categories - _permissionless_ and _permissioned_. 

**Permissionless systems**, such as Bitcoin and Ethereum, allow anybody to join the network. This approach works great in an open environment such as peer-to-peer cash, but it has two main drawbacks - _efficiency_ and _control_. 

In a permsionless system it must be assumed that all actors on the network are potentially malicious. This requires a more costly consensus algorithm, such as [Proof-of-Work](https://en.bitcoin.it/wiki/Proof_of_work), in order to verify that transactions are authentic. The second drawback is lack of control over which actors are allowed to join the network. For many enterprise use cases, such as ConsenSource, there is a need to limit the network to only relevant actors.

**Permissioned systems** aim to solve these two problems of efficiency and control. ConsenSource uses [Hyperledger Sawtooth](https://www.hyperledger.org/projects/sawtooth), an enterprise DLT, to provide an additional layer of security and access control by allowing for _on-chain governance_. 

In practice, this means that all members of the network must reach consensus around which new actors are allowed to join the network, the types of actions they can perform, and more. A permissioned DLT lets ConsenSource use a more efficient consensus algorithm ([more info here - todo](test.com)) because there is a base level of trust between participants in the network.

{{< alert title="Takeaway">}}
ConsenSource uses a _permissioned_ DLT to provide both greater efficiency, and control, of the network.
{{< /alert >}}

## Why Use DLT?

### Benefits

- _Enhanced Trust_
  - In a centralized database solution, all parties must trust the owner of the database - in situations with potentially untrusted participants, this is often not practical.
    DLT enforces trust by enabling all parties to share an immutable and complete record of all transactions that occur in the network.
- _Distributed Responsibility_
  - Rather than a single party bearing the burden of maintaing and running a centralized database, DLT allows multiple parties to collaboratively maintain a ledger of transactions.
- _Improved Efficiency_
  - The process of manually reconciling multiple ledgers, between multiple parties, is time consuming, expensive, and error prone. By sharing a single digital ledger, all parties recieve more 
    accurate data while performing less work.

### Tradeoffs

- _Blockchain is an emerging technology_
  - As such, the platforms and tools in the blockchain ecosystem are changing rapidly, and many developers have little experience with the technology.
  - The _Hyperledger Sawtooth_ platform supports a wide variety of programming languages for smart contract development, making it easier for new developers to utilize their existing skills.
- _Reaching consensus is time consuming_
  - The network much reach consensus regarding the validity of all transactions in the network. This takes time, and is slower than a traditional database.
  - ConsenSource utilizes an efficient consensus algorithm to reduce network latency.
- _Multiple parties need to run validator node_
  - The premise of blockchain technology is built around multiple parties running nodes that validate transactions, rather than a single entity managing and maintaining the network.
  - This enforces transparency and trust around the validity of all transactions. ConsenSource has [documentation](/consensource-docs/docs/developer/getting-started/) on running a node in multiple environments.

## Why Sawtooth?

<iframe width="560" height="315" src="https://www.youtube.com/embed/3BVUiPQuv-w?start=28" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
