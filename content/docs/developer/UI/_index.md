---
title: "User Interface"
weight: 8
---

{{< alert>}}
[View the ConsenSource UI on Github](https://github.com/target/consensource-ui/tree/master)
{{< /alert >}}

## Overview

The ConsenSource UI allows users to accomplist two primary tasks:

1. _Submit transactions to the ConsenSource network that update the state of the blockchain_
2. _Discover and monitor certification data, and organizations involved in the certification process_

In order to submit transactions to the network, users are represented on the blockchain as an _agent_. Agents can act on behalf of an _organization_.

Passwords and private keys are _not stored on the blockchain_ - only the [agent](https://github.com/target/consensource-common/blob/master/protos/agent.proto) assosciated to a user is stored on-chain.

Agents are created for a user during the sign up process. The admin of an organization can add or remove an agent at any point. All transactions that are submitted to ConsenSource must have an assosciated agent and organization.

An organization is representative of one of four distinct persona types:

- [Retailers/Brands](user-personas/retailer)
- [Factories](user-personas/factory)
- [Certifying Bodies](user-personas/cert-body)
- [Standards Bodies](user-personas/standards-body)

Each of the personas has a distinct role in the certification lifecycle, and can only perform a specific set of actions that will update the state of the blockchain. For example, an agent that is a member of a factory organization cannot approve or deny certifications.

The organization that a user belongs to determines the set of components that they are able to interact with in the UI. For example, a Retailer/Brand is able to favorite a list of factories that they do business with, but a factory cannot favorite other factories.

<b>Read the guides below to learn more about the ConsenSource UI.</b>

### ConsenSource UI Guides
