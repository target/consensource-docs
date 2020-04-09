---
title: "State Delta Subscriber"
weight: 6
description: >
  Overview and examples of the ConsenSource State Delta Subscriber
---

{{< alert title="GitHub Link">}}
https://github.com/target/consensource/tree/master/state_delta_subscriber
{{< /alert >}}

The goal of State Delta Export is to provide a mechanism for exporting on-chain state values from a validator to an external data store. This allows applications to efficiently query their state values. The state delta export implements an event subscription client that subscribes to block commit events and Sawtooth state delta at specific addresses (in this case we will subscribe to all state delta events at the Certificate Registry namespace). Sawtooth sends these events whenever the validator’s state is updated. The events contain the raw state data at the updated addresses. The event subscription client processes the event data and uses it to update the reporting database, an off-chain copy of blockchain state. The REST API can query this database when a client needs to get information from the blockchain.

This off-chain state access comes at the expense of relying on a single validator for state updates, this puts the application at risk of having stale data or forked state if the validator supplying the state updates comes out of consensus or is disconnected from the network. Below are guidelines on how to structure the reporting DB so fork resolution is easy.

## Reporting Database Structure

![DB Schema](https://github.com/target/consensource-docs/blob/master/content/docs/developer/Application%20Developer's%20Guide/SDS/ConsenSource_DBSchema.png?raw=true)

The reporting DB has three types of tables: sawtooth-core tables (Blocks), domain-specific state tables (Agents, Organizations, Authorizations, Certificates, CertificateData), and application tables (tables supporting off-chain application components, such as Auth, in the schema above).
The reporting DB should follow the [Type 2 of Slowly Changing Dimensions](https://en.wikipedia.org/wiki/Slowly_changing_dimension#Type_2:_add_new_row) data management pattern, so that fork resolution is easy.

For use in a State Delta Export, a state table includes additional columns of start_block_num and end_block_num. The ​start_block_num​ and ​end_block_num​ columns specify the block range in which that state value is set or exists. Values that are valid as of the current block have ​end_block_num​ set to MAX_UINT_64.

Because there might be state objects in the database that refer to the same object in state at different block heights (with different block numbers), the primary key for each object cannot be the same as the object’s natural key in blockchain state. Instead, use a sequence ID column or another unique ID scheme as the primary key. For better query performance, we recommend creating indexes on the natural key of the entry and the end_block_num.

## Fork resolution

The following pseudocode demonstrates the fork resolution process:

```
resolve_fork(existing_block):
  for table in domain_tables:
    delete from table where start_block_num >= existing_block.block_num
    update table set end_block_num = MAX_UINT_64 \
      where end_block_num >= existing_block.block_num
  delete from block where block_num >= existing_block.block_num
```
