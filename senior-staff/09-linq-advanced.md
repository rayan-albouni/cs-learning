# LINQ: Deferred Execution, IEnumerable vs IQueryable

[← Back to Main](../README.md) | [Previous: Concurrency & Synchronization ←](08-concurrency-synchronization.md) | [Next: Yield and Iterator Blocks →](10-yield-iterator-blocks.md)

---

<details>
<summary>Q41. Deferred execution — when does exception throw?</summary>

Answer:
- Iterator blocks defer execution until enumeration; exceptions throw during enumeration, not at query creation.

Takeaway:
- LINQ over IEnumerable is deferred.
</details>

<details>
<summary>Q42. Multiple enumeration — why is this a problem?</summary>

Answer:
- `q` is re-enumerated; might cause performance or side effects problems.

Takeaway:
- Materialize once (ToList/ToArray) when needed multiple times.
</details>

<details>
<summary>Q43. IEnumerable vs IQueryable — where does filtering happen?</summary>

Answer:
- Keep expression trees translatable. Use `.AsEnumerable()` to switch to in-memory processing.

Takeaway:
- EF Core must translate or throw.
</details>

<details>
<summary>Q44. OrderBy stability — is LINQ OrderBy stable?</summary>

Answer:
- C# LINQ OrderBy is stable; items with equal keys preserve relative order.

Takeaway:
- OrderBy/ThenBy are stable in LINQ to Objects.
</details>

<details>
<summary>Q45. Distinct with custom equality — what's used?</summary>

```csharp
var s = new[] { "A", "a", "A" };
var d = s.Distinct(StringComparer.OrdinalIgnoreCase);
```

Answer:
- Uses provided comparer to identify distinct elements.

Takeaway:
- Distinct uses equality comparer semantics.
</details>

---

[← Back to Main](../README.md) | [Previous: Concurrency & Synchronization ←](08-concurrency-synchronization.md) | [Next: Yield and Iterator Blocks →](10-yield-iterator-blocks.md)
