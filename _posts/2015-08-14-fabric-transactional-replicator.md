---
layout: post
title: Fabric Transactional Replicator
date: 2015-08-15 12:00:00
categories: csharp fabirc
---

## Fabric's Transactional Replicator

[Serivce Fabric](http://azure.microsoft.com/en-us/campaigns/service-fabric/) has such a huge feature set that it's hard to describe what it is succinctly while still capturing the features which separate it from its predecessors. The feature which really sets Fabric apart from alternatives is its replication system. Fabric's replication system provides you with the building blocks to create distributed databases (eg, [DocDB](http://azure.microsoft.com/en-us/services/documentdb/)) and other stateful systems (eg, [Event Hubs](http://azure.microsoft.com/en-us/services/event-hubs/)).

### The Transactional Replicator
One of the state providers provided by Fabric is `TransactionalReplicator`. `TransactionalReplicator` is an extensible state provider which replicates transactions. Each transaction contains a series of operations on underlying state providers such as the built-in `IRelibleDictionary` or `IReliableQueue` or a user-specified state provider. Each operation contains a pair of `byte[]` describing how to redo/apply and undo the operation.

When you add this together, you get a system which lets you robustly replicate transactions which can involve multiple data structures.

Under the hood, the replicator maintains a persistent transaction log which is replicated using distributed consensus (a Paxos variant).

If you want to create your own database which can take part in these transactions, you need to implement the `IStateProvider2` and `IReliableState` interfaces. Your database method would take a `Transaction` in an argument and call `AddOperation` to add the redo and corresponding undo operation for the method.


Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
