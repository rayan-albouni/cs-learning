# Unit Testing Pitfalls

[← Back to Main](../README.md) | [Previous: Entity Framework Core ←](11-entity-framework-core.md) | [Next: Struct vs Class, Boxing →](13-struct-class-boxing.md)

---

<details>
<summary>Q55. async void test — why is it bad?</summary>

Answer:
- The framework can't await `async void`; test may finish before completion.

Fix:
- Test methods should return Task for async.

Takeaway:
- `async void` cannot be awaited.
</details>

<details>
<summary>Q56. Mocking EF Core async queries — what's required?</summary>

Answer:
- `ToListAsync` relies on IAsyncQueryProvider.

Takeaway:
- Use EF Core's InMemory provider or proper async-capable mocks.
</details>

<details>
<summary>Q57. Floating point assertions — why do tests flake?</summary>

```csharp
Assert.That(0.1 + 0.2, Is.EqualTo(0.3).Within(1e-12));
```

Takeaway:
- For double/float, assert with tolerances.
</details>

<details>
<summary>Q58. Time-dependent tests — how to avoid DateTime.Now?</summary>

Answer:
- Inject an IClock or use `TimeProvider` (.NET 8).

Takeaway:
- Abstract time for deterministic tests.
</details>

---

[← Back to Main](../README.md) | [Previous: Entity Framework Core ←](11-entity-framework-core.md) | [Next: Struct vs Class, Boxing →](13-struct-class-boxing.md)
