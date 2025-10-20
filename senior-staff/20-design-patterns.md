# Design Patterns (GoF + DI/Repository/CQRS)

[← Back to Main](../README.md) | [Previous: Design Principles (SOLID) ←](19-design-principles-solid.md)

---

<details>
<summary>Q82. Singleton — thread-safe and testable?</summary>

```csharp
class Singleton
{
    private static readonly Lazy<Singleton> _i = new(() => new Singleton());
    public static Singleton Instance => _i.Value;
    private Singleton() {}
}
```

Answer:
- Thread-safe lazy singleton. But singletons can be hard to test.

Takeaway:
- Use DI container singletons for testability.
</details>

<details>
<summary>Q83. Factory Method vs Abstract Factory — when to use?</summary>

Answer:
- Factory Method: let subclasses decide which product to create.
- Abstract Factory: provide families of related objects.

Takeaway:
- Use Factory Method for single product customization.
</details>

<details>
<summary>Q84. Strategy vs State — difference in intent?</summary>

Answer:
- Strategy: interchangeable algorithms set from outside.
- State: behavior changes with internal state transitions.

Takeaway:
- Strategy is about selection; State is about transitions.
</details>

<details>
<summary>Q85. Decorator vs Proxy — which is which?</summary>

Answer:
- Decorator adds behavior transparently.
- Proxy controls access.

Takeaway:
- Choose Decorator to add responsibilities; Proxy to control access.
</details>

<details>
<summary>Q86. Repository pattern — when is it an anti-pattern?</summary>

Answer:
- If your repository just mirrors DbSet and loses EF features, it can be counterproductive.

Takeaway:
- Consider using DbContext directly with rich queries.
</details>

<details>
<summary>Q87. CQRS — can you mix reads and writes?</summary>

Answer:
- Separate read models and write models.

Takeaway:
- Use CQRS where read/write requirements differ significantly.
</details>

<details>
<summary>Q88. Dependency Injection — service lifetimes pitfalls?</summary>

Answer:
- Injecting scoped into singleton causes captured scope bugs.

Takeaway:
- Understand Singleton/Scoped/Transient interactions.
</details>

---

[← Back to Main](../README.md) | [Previous: Design Principles (SOLID) ←](19-design-principles-solid.md)
