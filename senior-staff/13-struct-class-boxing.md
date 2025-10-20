# Struct vs Class, Boxing/Unboxing, in/out/ref

[← Back to Main](../README.md) | [Previous: Unit Testing Pitfalls ←](12-unit-testing-pitfalls.md) | [Next: Span/Memory and Performance →](14-span-memory-performance.md)

---

<details>
<summary>Q59. Boxing via interface — does this allocate?</summary>

```csharp
struct S { public int X; }
IComparable c = 5; // boxing
```

Answer:
- Yes, boxing occurs.

Takeaway:
- Avoid boxing in hot paths.
</details>

<details>
<summary>Q60. Mutating a struct property — why doesn't it stick?</summary>

Answer:
- For auto-property returns by value, mutations affect a temporary copy.

Takeaway:
- Structs copy by value; be careful with mutability through properties.
</details>

<details>
<summary>Q61. readonly struct and defensive copies — when do they occur?</summary>

Answer:
- The compiler may copy to avoid mutation through certain contexts.

Takeaway:
- Use `readonly` members to reduce hidden copies.
</details>

<details>
<summary>Q62. ref, out, in — what's allowed?</summary>

```csharp
void M(in int x, ref int y, out int z) { z = x + y; y++; }
```

Answer:
- `in` is readonly by reference; `ref` allows reading/writing; `out` must be assigned.

Takeaway:
- Prefer `in` for large readonly structs.
</details>

---

[← Back to Main](../README.md) | [Previous: Unit Testing Pitfalls ←](12-unit-testing-pitfalls.md) | [Next: Span/Memory and Performance →](14-span-memory-performance.md)
