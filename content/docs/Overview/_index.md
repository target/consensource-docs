---
title: Overview
weight: 1
description: An introduction to ConsenSource and Distributed Ledger Technology (DLT)
---

{{< alert >}}
The ConsenSource application  leverages Distributed Ledger Technology (DLT)  to help retailers, factories, certifying bodies 
and standards bodies increase transparency and efficiency around the certification process.
{{< /alert >}}

### The Problem

For many retailers, a commom pain point in the sourcing process is the discovery and verification of the certification claims for a given factory. If a retailer wishes to only do business with factories that have a given environmental certification, where can they find a filtered list of only the factories that are certified? Furthermore, _how can they trust that the certification is valid_?

In practice, each retailer ends up independently verifying these claims with the ceritfying body that performed the original audit. This process is time-consuming, expensive, and error prone. 

### How ConsenSource Can Help

ConsenSource solves this problem using _Distributed Ledger Technology (DLT)_. The application serves as a common platform to verify and display the certifications and audit data between retailers, factories, certifying bodies, and standards bodies.

DLT allows all partners to share ownership of this data. Each partner is tasked with the duty of owning their data and keeping this information up to date, shifting the onus of data accuracy to the relevant party.
Each participant in the network plays a unique role. 

**Standards bodies** create standards and accredit certifying bodies. 

**Certifying bodies** are then able to audit and certify factories against the standards they are accredited for.

**Factories** are able to request these certifications and in-person audits, so that certifying bodies are able to issue certificates based on in-person, off-chain audit results. 

**Retailers** are able to view and trust the resulting data from these interactions as well as general contact and location information on each of these entities.

### Who Should Use ConsenSource?

**Retailers, Factories, Certifying Bodies and Standards Bodies**

- Retailers
  - Benefit 1
  - Benefit 2
- Factories
  - Benefit 1
  - Benefit 2
- Certifying Bodies
  - Benefit 1
  - Benefit 2
- Standards Bodies
  - Benefit 1
  - Benefit 2
  
## Distributed Ledger Technology (DLT) Overview

### What Is A Blockchain?

A blockchain is a network in which a record of transactions are maintained across several computers that are linked in a peer-to-peer network. All nodes in this network reach consensus on what the state of the ledger should look like before adding to it. This enforces a level of  trust and transparency regarding the authenticity and immutability of data within the ledger.

Blockchains are a particular type of distributed ledger technology (DLT), and as such, we have elected to use the broader term, DLT, as we further develop ConsenSource.

<iframe width="560" height="315" src="https://www.youtube.com/embed/3xGLc-zz9cA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### What Is A Permissioned Blockchain?

Blockchains can be further subdivided into two categories - _permissionless_ and _permissioned_. 

**Permissionless systems**, such as Bitcoin and Ethereum, allow anybody to join the network. This approach works great in an open environment such as peer-to-peer cash, but it has two main drawbacks - _efficiency_ and _control_. 

In a permsionless system it must be assumed that all actors on the network are potentially malicious. This requires a more costly consensus algorithm, such as [Proof-of-Work](https://en.bitcoin.it/wiki/Proof_of_work), in order to verify that transactions are authentic. The second drawback is lack of control over which actors are allowed to join the network. For many enterprise use cases, such as ConsenSource, there is a need to limit the network to only relevant actors.

**Permissioned systems** aim to solve these two problems of efficiency and control. ConsenSource uses [Hyperledger Sawtooth](https://www.hyperledger.org/projects/sawtooth), an enterprise DLT, to provide an additional layer of security and access control by allowing for _on-chain governance_. 

In practice, this means that all members of the network must reach consensus around which new actors are allowed to join the network, the types of actions they can perform, and more. A permissioned DLT lets ConsenSource use a more efficient consensus algorithms ([more info here - todo](test.com)) because there is a base level of trust between participants in the network.

{{< alert title="Takeaway">}}
ConsenSource uses a _permissioned_ DLT to provide both greater efficiency, and control, of the network.
{{< /alert >}}

### Why Use DLT?

#### Benefits

- _Enhanced Trust_
  - In a centralized database solution, all parties must trust the owner of the database - in situations with potentially untrusted participants, this is often not practical.
    DLT enforces trust by enabling all parties to share an immutable and complete record of all transactions that occur in the network.
- _Distributed Responsibility_
  - Rather than a single party bearing the burden of maintaing and running a centralized database, DLT allows multiple parties to collaboratively maintain a ledger of transactions.
- _Improved Efficiency_
  - The process of manually reconciling multiple ledgers, between multiple parties, is time consuming, expensive, and error prone. By sharing a single digital ledger, all parties recieve more 
    accurate data while performing less work.

#### Tradeoffs

### Why Sawtooth?

<iframe width="560" height="315" src="https://www.youtube.com/embed/3BVUiPQuv-w?start=28" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Examples

- [Walmart/IBM Food Trust](https://www.ibm.com/blockchain/solutions/food-trust)
- [Supply Chain Provenance With Hyperledger Sawtooth](https://sawtooth.hyperledger.org/examples/seafood.html)
