---
published: false
---

## Fabric's Transactional Replicator

[Serivce Fabric](http://azure.microsoft.com/en-us/campaigns/service-fabric/) has such a huge feature set that it's hard to describe what it is succinctly while still capturing the features which separate it from its predecessors. The feature which really sets Fabric apart from alternatives is its replication system. Fabric's replication system provides you with the building blocks to create distributed databases (eg, [DocDB](http://azure.microsoft.com/en-us/services/documentdb/)) and other stateful systems (eg, [Event Hubs](http://azure.microsoft.com/en-us/services/event-hubs/)).

### The Transactional Replicator
One of the state providers provided by Fabric is `TransactionalReplicator`. `TransactionalReplicator` is an extensible state provider which replicates transactions. Each transaction contains a series of operations on underlying state providers such as the built-in `IRelibleDictionary<TKey,TValue>` or `IReliableQueue<T>` or a user-specified state provider. Each operation contains a pair of `byte[]` describing how to redo/apply and undo the operation.


Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
