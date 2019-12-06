---
title: About ConsenSource
linkTitle: About
menu:
  main:
    weight: 10
---

{{% blocks/cover title="About ConsenSource" height="auto" %}}

The ConsenSource application leverages Distributed Ledger Technology (DLT)  to help retailers, factories, certifying bodies 
and standards bodies increase transparency and efficiency around the certification process.

For many retailers, a commom pain point in the sourcing process is the discovery and verification of the certification claims for a given factory. If a retailer wishes to only do business with factories that have a given environmental certification, where can they find a filtered list of only the factories that are certified? Furthermore, _how can they trust that the certification is valid_?

In practice, each retailer ends up independently verifying these claims with the ceritfying body that performed the original audit. This process is time-consuming, expensive, and error prone. 
{{% /blocks/cover %}}

{{% blocks/section type="section" color="primary" %}}
# How ConsenSource Can Help

<br />

ConsenSource solves this problem of trust and authenticity within the certification process using _Distributed Ledger Technology (DLT)_. The application serves as a common platform to verify and display the certifications and audit data between retailers, factories, certifying bodies, and standards bodies.

DLT allows all partners to share ownership of this data. Each partner is tasked with the duty of owning their data and keeping this information up to date, shifting the onus of data accuracy to the relevant party.
Each participant in the network plays a unique role. 

- **Standards bodies** create standards and accredit certifying bodies. 

- **Certifying bodies** are then able to audit and certify factories against the standards they are accredited for.

- **Factories** are able to request these certifications and in-person audits, so that certifying bodies are able to issue certificates based on in-person, off-chain audit results. 

- **Retailers** are able to view and trust the resulting data from these interactions as well as general contact and location information on each of these entities.
{{% /blocks/section %}}

{{% blocks/section type="section" color="white" %}}
## Who Should Use ConsenSource?

<br />

<table>
  <tr>
   <td><strong>Retailers</strong>
   </td>
   <td>
    <ul>
      <li>
        Reduction of overhead associated with tracking and maintaining vendor certification status and factory profile details
      </li>
      <li>
        Avoidance of negative brand reputation impact by helping achieve and measure towards sustainability goals
      </li>
      <li>
        Additional visibility to new vendors with applicable certifications and history
      </li>
    </ul> 
   </td>
  </tr>
  <tr>
   <td><strong>Factories</strong>
   </td>
   <td>
    <ul>
      <li>
        Easy to request certifications resulting in reduction of time locating validated certifying bodies and auditors
      </li>
      <li>
        Potential for open competition drives audit costs down, or keeps audit costs in check
      </li>
    </ul> 
   </td>
  </tr>
  <tr>
   <td><strong>Certifying Bodies</strong>
   </td>
   <td>
    <ul>
      <li>
        Potential for increased business due to visibility to any factory interested in certification
      </li>
      <li>
         Provides a low cost platform for sharing certification status
      </li>
    </ul> 
   </td>
  </tr>
  <tr>
   <td><strong>Standards Bodies</strong>
   </td>
   <td>
    <ul>
      <li>
        Ability to promote their standards to a wide range of industries, factories, and those requiring standards to further their mission
      </li>
    </ul> 
   </td>
  </tr> 
</table>
{{% /blocks/section %}}

{{% blocks/cover title="Distributed Ledger Technology (Blockchain) Overview" height="auto" %}}

A blockchain is a network in which a record of transactions are maintained across several computers that are linked in a peer-to-peer network. All nodes in this network reach consensus on what the state of the ledger should look like before adding to it. This enforces a level of  trust and transparency regarding the authenticity and immutability of data within the ledger.

Blockchains are a particular type of distributed ledger technology (DLT), and as such, we have elected to use the broader term, DLT, as we further develop ConsenSource.

<br />

<div>
  <iframe width="560" height="315" src="https://www.youtube.com/embed/3xGLc-zz9cA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

<br />
{{% /blocks/cover %}}

{{% blocks/section type="section" color="primary" %}}
## What Is A Permissioned Blockchain?

<br />

Blockchains can be further subdivided into two categories - _permissionless_ and _permissioned_. 

**Permissionless systems**, such as Bitcoin and Ethereum, allow anybody to join the network. This approach works great in an open environment such as peer-to-peer cash, but it has two main drawbacks - _efficiency_ and _control_. 

In a permsionless system it must be assumed that all actors on the network are potentially malicious. This requires a more costly consensus algorithm, such as [Proof-of-Work](https://en.bitcoin.it/wiki/Proof_of_work), in order to verify that transactions are authentic. The second drawback is lack of control over which actors are allowed to join the network. For many enterprise use cases, such as ConsenSource, there is a need to limit the network to only relevant actors.

**Permissioned systems** aim to solve these two problems of efficiency and control. ConsenSource uses [Hyperledger Sawtooth](https://www.hyperledger.org/projects/sawtooth), an enterprise DLT, to provide an additional layer of security and access control by allowing for _on-chain governance_. 

In practice, this means that all members of the network must reach consensus around which new actors are allowed to join the network, the types of actions they can perform, and more. A permissioned DLT lets ConsenSource use a more efficient consensus algorithms ([more info here - todo](test.com)) because there is a base level of trust between participants in the network.

<br />

#### ConsenSource uses a _permissioned_ DLT to provide both enhanced privacy and efficiency on the network.

{{% /blocks/section %}}

{{% blocks/section type="section" color="white" %}}
## Why Use Distributed Ledger Technology?

<br />

### Benefits

- _Enhanced trust_
  - In a centralized database solution, all parties must trust the owner of the database - in situations with potentially untrusted participants, this is often not practical.
    DLT enforces trust by enabling all parties to share an immutable and complete record of all transactions that occur in the network.
- _Distributed responsibility_
  - Rather than a single party bearing the burden of maintaing and running a centralized database, DLT allows multiple parties to collaboratively maintain a ledger of transactions.
- _Improved efficiency_
  - The process of manually reconciling multiple ledgers, between multiple parties, is time consuming, expensive, and error prone. By sharing a single digital ledger, all parties recieve more accurate data while performing less work.

### Tradeoffs

- _Blockchain is an emerging technology_
  - As such, the platforms and tools in the blockchain ecosystem are changing rapidly, and many developers have little experience with the technology.
  - The _Hyperledger Sawtooth_ platform supports a wide variety of programming languages for smart contract development, making it easier for new developers to utilize their existing skills.
- _Reaching consensus is time consuming_
  - The network much reach consensus regarding the validity of all transactions in the network. This takes time, and is slower than a traditional database.
  - ConsenSource utilizes an efficient consensus algorithm to reduce network latency.
- _Multiple parties need to run validator node_
  - The premise of blockchain technology is built around multiple parties running nodes that validate transactions, rather than a single entity managing and maintaining the network.
  - This enforces transparency and trust around the validity of all transactions. ConsenSource has [documentation](docs/getting-started/) on running a node in multiple environments.

<br />

## Why Use Hyperledger Sawtooth?

<br />

<iframe width="560" height="315" src="https://www.youtube.com/embed/3BVUiPQuv-w?start=28" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br />



