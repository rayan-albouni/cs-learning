# Entity Framework Core

[← Back to Main](../README.md) | [Previous: Yield and Iterator Blocks ←](10-yield-iterator-blocks.md) | [Next: Unit Testing Pitfalls →](12-unit-testing-pitfalls.md)

---

<details>
<summary>Q48. Tracking vs AsNoTracking — which is faster?</summary>

Answer:
- `AsNoTracking` is typically faster for read-only queries.

Takeaway:
- Use `AsNoTracking` for read-only queries.
</details>

<details>
<summary>Q49. Lazy loading proxies — why can serialization explode?</summary>

Answer:
- Lazy loading proxies can cause circular reference graphs and unexpected database queries.

Takeaway:
- Avoid lazy loading in serialization boundaries; prefer eager loading or DTOs.
</details>

<details>
<summary>Q50. Context lifetime and thread safety — can DbContext be shared?</summary>

Answer:
- DbContext is not thread-safe.

Takeaway:
- Scope DbContext appropriately (e.g., per request in web apps).
</details>

<details>
<summary>Q51. N+1 problem and AsSplitQuery — when to use?</summary>

Answer:
- `AsSplitQuery` executes separate queries per include to avoid cartesian explosion.

Takeaway:
- Use `AsSplitQuery` for complex graphs.
</details>

<details>
<summary>Q52. Client evaluation removal — why does query throw?</summary>

Answer:
- EF Core 3+ forbids client evaluation in Where and other key clauses.

Takeaway:
- Ensure predicates are translatable.
</details>

<details>
<summary>Q53. Concurrency tokens — how to handle DbUpdateConcurrencyException?</summary>

Answer:
- Concurrency token mismatch throws. You may reload, merge changes, retry, or abort.

Takeaway:
- Design for retries and conflict resolution.
</details>

<details>
<summary>Q54. FromSqlInterpolated vs FromSqlRaw — SQL injection risk?</summary>

Answer:
- Using string interpolation with FromSqlRaw is unsafe.

Takeaway:
- Always parameterize SQL with FromSqlInterpolated.
</details>

---

[← Back to Main](../README.md) | [Previous: Yield and Iterator Blocks ←](10-yield-iterator-blocks.md) | [Next: Unit Testing Pitfalls →](12-unit-testing-pitfalls.md)
