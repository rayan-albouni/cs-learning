# Nullable Reference Types and Nullability Operators

[← Back to Main](../README.md) | [Previous: Generics ←](04-generics.md) | [Next: Exception Handling →](06-exception-handling.md)

---

<details>
<summary>Q24. Null-forgiving operator (!) — what does it do?</summary>

```csharp
string? s = GetString();
Console.WriteLine(s!.Length); // compile-time OK?
```

Answer:
- The `!` suppresses nullability warnings for the expression. It does nothing at runtime.
- If `s` is null at runtime, this will throw `NullReferenceException`.

Takeaway:
- Use `!` sparingly and only when you have logically proven non-null.
</details>

<details>
<summary>Q25. Null-coalescing assignment (??=) — when is it invoked?</summary>

```csharp
Dictionary<string, string>? cache = null;
(cache ??= new()).Add("k", "v"); // ?
Console.WriteLine(cache.Count);
```

Answer:
- If `cache` is null, it's assigned to `new Dictionary<...>()` and then `.Add` is called. Prints `1`.

Takeaway:
- `??=` is convenient for lazy initialization of references.
</details>

<details>
<summary>Q26. Null-conditional operator chaining — what's the return?</summary>

```csharp
class A { public B? B { get; set; } }
class B { public string? S { get; set; } }
A? a = null;
Console.WriteLine(a?.B?.S?.ToUpper() ?? "none");
```

Answer:
- Safe navigation returns `null` at first step; `??` coalesces to `"none"`. Prints `none`.

Takeaway:
- Chain `?.` and finish with `??` to provide defaults.
</details>

---

[← Back to Main](../README.md) | [Previous: Generics ←](04-generics.md) | [Next: Exception Handling →](06-exception-handling.md)
