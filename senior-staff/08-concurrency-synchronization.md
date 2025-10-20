# Concurrency & Synchronization Primitives

[← Back to Main](../README.md) | [Previous: Async/Await Advanced ←](07-async-await-advanced.md) | [Next: LINQ Advanced →](09-linq-advanced.md)

---

<details>
<summary>Q36. lock sugar and exceptions — can you throw inside lock?</summary>

Answer:
- Yes. The compiler emits try/finally with `Monitor.Enter/Exit`. The lock is always released.

Takeaway:
- `lock` is exception-safe.
</details>

<details>
<summary>Q37. lock(this) — why is it risky?</summary>

Answer:
- External code can also lock on your publicly accessible instance and cause deadlocks.

Takeaway:
- Lock on a private readonly object dedicated for locking.
</details>

<details>
<summary>Q38. SemaphoreSlim WaitAsync — best practices?</summary>

```csharp
var sem = new SemaphoreSlim(1,1);
await sem.WaitAsync();
try { /* work */ }
finally { sem.Release(); }
```

Takeaway:
- Always pair `WaitAsync` with `Release` in a finally.
</details>

<details>
<summary>Q39. ConcurrentDictionary GetOrAdd — called multiple times?</summary>

Answer:
- The value factory may be invoked multiple times depending on race timing.

Takeaway:
- Minimize side effects inside factory and rely on idempotency.
</details>

<details>
<summary>Q40. Interlocked operations — memory fence?</summary>

Answer:
- Interlocked operations are full memory fences.

Takeaway:
- Use Interlocked for atomic ops and memory barriers.
</details>

---

[← Back to Main](../README.md) | [Previous: Async/Await Advanced ←](07-async-await-advanced.md) | [Next: LINQ Advanced →](09-linq-advanced.md)
