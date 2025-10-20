# Span/Memory and Performance Types

[← Back to Main](../README.md) | [Previous: Struct vs Class, Boxing ←](13-struct-class-boxing.md) | [Next: Reflection and Expression Trees →](15-reflection-expression-trees.md)

---

<details>
<summary>Q63. Span cannot escape — why won't this compile?</summary>

```csharp
Span<int> Make()
{
    Span<int> s = stackalloc int[10];
    return s; // error
}
```

Answer:
- `Span<T>` is a ref struct and cannot escape the method stack frame.

Takeaway:
- Ref-like types are stack-only.
</details>

<details>
<summary>Q64. Capturing Span in lambda — allowed?</summary>

Answer:
- Not allowed: ref struct cannot be captured by lambdas.

Takeaway:
- Don't capture ref structs.
</details>

<details>
<summary>Q65. String creation with spans — why is this fast?</summary>

```csharp
string s = string.Create(5, 0, (span, state) => span.Fill('x'));
```

Answer:
- `string.Create` allocates once and writes directly into the string's buffer.

Takeaway:
- Use `string.Create` and spans for high-performance string building.
</details>

---

[← Back to Main](../README.md) | [Previous: Struct vs Class, Boxing ←](13-struct-class-boxing.md) | [Next: Reflection and Expression Trees →](15-reflection-expression-trees.md)
