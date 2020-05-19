---
title: "State Delta Subscriber"
weight: 6
---

{{< alert>}}
[View the ConsenSource SDS on Github](https://github.com/target/consensource-sds/tree/master)
{{< /alert >}}

The goal of the State Delta Export (SDS) service is to provide a mechanism for exporting on-chain state values from a validator to an external data store. This allows applications to efficiently query their state values. 

### Sawtooth Overview
Sawooth provides an [event subscription](https://sawtooth.hyperledger.org/docs/core/nightly/1-1/app_developers_guide/about_event_subscriptions.html) service that allows clients to subscribe to two types of events:

1. Block commits
2. State Delta events (at a particular address range)

Sawtooth sends these events whenever the validator’s state is updated. State Delta events contain the raw state data at the updated addresses. 

### SDS Implementation

In ConsenSource, the SDS subscribes to all block commits and all state delta events in the ConsenSource namespace (`3d0111`). 

When a *block commit event* is emitted from a validator, the block ID and the block num are parsed and stored in the reporting DB.

When a *state delta event* is emitted from a validator, the data is parsed into the respective protobuf message, the message is converted into a valid object, and the object is stored in the reporting DB. 

By subscribing to these two events, the SDS provides a mechanism for other services, such as the REST API, to query an up-to-date database without needing to make more expensive queries to a validator node.

### Tradeoffs

This off-chain state access comes at the expense of relying on a single validator for state updates. 

By relying on a secondary data source instead of a validator node, this puts the application at risk of having stale data, or a forked state if the validator that is supplying the state updates comes out of consensus or is disconnected from the network. 

### Reporting Database Structure

<!-- TODO: Update the DB Schema diagram with latest schema -->
<!-- ![DB Schema](https://github.com/target/consensource-docs/blob/master/content/docs/developer/Application%20Developer's%20Guide/SDS/ConsenSource_DBSchema.png?raw=true) -->

The reporting database has three types of tables: 

- sawtooth-core tables 
  - Blocks
- Domain-specific state tables 
  - Agents, Assertions, Organizations, Authorizations, Certificates, CertificateData
- Application tables 
  - Tables supporting off-chain application components, such as User info and Auth

The reporting database should follow the [Type 2 of Slowly Changing Dimensions](https://en.wikipedia.org/wiki/Slowly_changing_dimension#Type_2:_add_new_row) data management pattern, so that fork resolution is easy.

### Start & End Block Nums

In a blockchain there is no way to "delete" information. To indicate that a value is state is still valid, the concept of starting and ending block numbers is used.

Tables that include state data have additional columns of *start_block_num* and *end_block_num*. The ​start_block_num​ and ​end_block_num​ columns specify the block range in which that state value is set or exists. Values that are valid as of the current block have ​end_block_num​ set to `MAX_UINT_64`.

> A state value is considered valid if the `end_block_num` is greater than or equal to the current block. 

### Primary Keys

Because there may be state objects in the database that refer to the same object in state at different block heights (with different block numbers), the primary key for each object cannot be the same as the object’s natural key in blockchain state. Instead, use a sequence ID column or another unique ID scheme as the primary key. For better query performance, it is recommended to create indexes on the natural key of the entry and the end_block_num.

### Fork resolution

The following pseudocode demonstrates the fork resolution process:

```
resolve_fork(existing_block):
  for table in domain_tables:
    delete from table where start_block_num >= existing_block.block_num
    update table set end_block_num = MAX_UINT_64 \
      where end_block_num >= existing_block.block_num
  delete from block where block_num >= existing_block.block_num
```
