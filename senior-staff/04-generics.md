# Generics: Constraints, Variance, Reification

[← Back to Main](../README.md) | [Previous: Delegates, Events, Closures ←](03-delegates-events-closures.md) | [Next: Nullable Reference Types →](05-nullable-reference-types.md)

---

<details>
<summary>Q19. IList&lt;object&gt; from List&lt;string&gt; — does this compile?</summary>

```csharp
List<string> list = new();
IList<object> olist = list; // ?
```

Answer:
- Does not compile. `IList<T>` is invariant.

Explanation:
- Variance applies to interfaces/delegates with out/in annotations. `IEnumerable<out T>` is covariant; `IList<T>` is not.

Takeaway:
- Use `IEnumerable<object>` or `IReadOnlyList<object>` for covariance where applicable.
</details>

<details>
<summary>Q20. Covariant and contravariant interfaces — which converts?</summary>

```csharp
IEnumerable<string> a = new List<string>();
IEnumerable<object> b = a; // ?
Action<object> x = o => { };
Action<string> y = x; // ?
```

Answer:
- `IEnumerable<string>` to `IEnumerable<object>` is allowed: covariance (`out T`).
- `Action<object>` to `Action<string>` is allowed: contravariance (`in T`).

Takeaway:
- Covariance: you can use a more derived type where a base is expected for outputs.
- Contravariance: you can use a less derived type where a more derived is expected for inputs.
</details>

<details>
<summary>Q21. Constraints: struct vs unmanaged vs notnull — what compiles?</summary>

Answer:
- Only `where T : unmanaged` guarantees no references, allowing pointers and `sizeof(T)`.
- `struct` allows managed fields, so not safe for pointers.
- `notnull` is a reference-type nullability constraint.

Takeaway:
- Use `unmanaged` when you need blittable types for interop and low-level operations.
</details>

<details>
<summary>Q22. Reified generics — can you get typeof(T) at runtime?</summary>

```csharp
static Type GetT<T>() => typeof(T);
Console.WriteLine(GetT<int>());
Console.WriteLine(GetT<string>());
```

Answer:
- Yes. .NET generics are reified. Output includes `System.Int32`, `System.String`.

Takeaway:
- Unlike Java type erasure, .NET keeps generic type parameters at runtime.
</details>

<details>
<summary>Q23. default(T) under constraints — what's default?</summary>

```csharp
static T Make<T>() => default;
Console.WriteLine(Make<int>());     // ?
Console.WriteLine(MakeRef<string>() == null); // ?
```

Answer:
- `Make<int>()` => `0`.
- `MakeRef<string>()` => `null` (prints `True`).

Takeaway:
- default(T) is 0-initialized for value types, null for reference types.
</details>

---

[← Back to Main](../README.md) | [Previous: Delegates, Events, Closures ←](03-delegates-events-closures.md) | [Next: Nullable Reference Types →](05-nullable-reference-types.md)
