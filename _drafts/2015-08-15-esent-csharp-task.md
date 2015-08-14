---
published: false
---

## Quick Tip: Using ManagedEsent with C# Tasks

Recently I've been playing around with [ESENT](https://en.wikipedia.org/wiki/Extensible_Storage_Engine) via [ManagedEsent](https://managedesent.codeplex.com/).
ESENT is an embedded database which comes with every version of Windows and is the basis of Active Directory, Exchange, and a bunch more.

ESENT tries to protect developers from multi-threaded access to a session by locking each session to a thread. Like most C# developers, I'm using `Task`s liberally. Tasks don't have thread affinity. So the thread may change between the start of a transaction and the time it's either committed or aborted. This can be a massive pain when working with Tasks in ESENT.

Thankfully, ESENT gives us an out: you can opt-out of the per-thread protection and instead specify your own context object which must be set when a `Session` is accessed from a given thread.

Using it is fairly straight forwards:
* Before calling ESENT, set the session context using [`JetSetSessionContext`](https://msdn.microsoft.com/en-us/library/microsoft.isam.esent.interop.api.jetsetsessioncontext.aspx).
* When you're done, reset the session context using [`JetResetSessionContext`](https://msdn.microsoft.com/en-us/library/microsoft.isam.esent.interop.api.jetresetsessioncontext.aspx).

Example:
```c#
try
{
    using (var tx = new ESENT.Transaction(table.Session))
    {
        // Set the session context.
        ESENT.Api.JetSetSessionContext(table.Session, table.Context);
        // This just calls the appropriate ESENT APIs.
        var result = table.Get(key);
        tx.Commit(ESENT.CommitTransactionGrbit.None);
        return result;
    }
}
finally
{
    // Reset the session context.
    ESENT.Api.JetResetSessionContext(table.Session);
}
```

I've associated the session and context with each other using some `table` object to make things easier to keep straight.
`Context` is an `IntPtr` which I allocate using `GCHandle.Alloc` and later `Free` during clean up.

[@reubenbond](https://twitter.com/reubenbond)