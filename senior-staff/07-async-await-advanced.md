# Async/Await: Deadlocks, ConfigureAwait, ValueTask, IAsyncEnumerable

[← Back to Main](../README.md) | [Previous: Exception Handling ←](06-exception-handling.md) | [Next: Concurrency & Synchronization →](08-concurrency-synchronization.md)

---

<details>
<summary>Q30. Deadlock with .Result on UI/ASP.NET context — what happens?</summary>

Answer:
- Potential deadlock: the continuation tries to post back to the captured SynchronizationContext, which is blocked waiting for `.Result`.

Fix:
- Use `await` all the way, or `ConfigureAwait(false)` inside library code.

Takeaway:
- Never block on async in context-bound threads.
</details>

<details>
<summary>Q31. ConfigureAwait(false) — when and why?</summary>

Answer:
- After `ConfigureAwait(false)`, continuation resumes on a thread pool thread, not the original context.

Takeaway:
- Use `ConfigureAwait(false)` in library code that doesn't need a context.
</details>

<details>
<summary>Q32. async void — where do exceptions go?</summary>

Answer:
- `async void` exceptions go to the synchronization context and can crash the process.

Takeaway:
- Avoid `async void` except for event handlers.
</details>

<details>
<summary>Q33. ValueTask pitfalls — can you await twice?</summary>

Answer:
- You must not await a non-Task-backed ValueTask twice.

Takeaway:
- Use ValueTask for performance-sensitive paths; otherwise, prefer Task.
</details>

<details>
<summary>Q35. await foreach cancellation — how to pass token?</summary>

```csharp
await foreach (var item in GetStreamAsync(ct)) { }

static async IAsyncEnumerable<int> GetStreamAsync([EnumeratorCancellation] CancellationToken ct)
{
    yield return 1;
}
```

Takeaway:
- Use `[EnumeratorCancellation]` on a token parameter for IAsyncEnumerable.
</details>

---

[← Back to Main](../README.md) | [Previous: Exception Handling ←](06-exception-handling.md) | [Next: Concurrency & Synchronization →](08-concurrency-synchronization.md)
