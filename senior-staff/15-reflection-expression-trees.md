# Reflection, Expression Trees, Dynamic Invocation

[← Back to Main](../README.md) | [Previous: Span/Memory and Performance ←](14-span-memory-performance.md) | [Next: Compiler/JIT Optimizations →](16-compiler-jit-optimizations.md)

---

<details>
<summary>Q66. DynamicInvoke vs CreateDelegate — performance difference?</summary>

Answer:
- `CreateDelegate` with strongly-typed delegate is much faster.

Takeaway:
- Cache strongly-typed delegates for repeated invocation.
</details>

<details>
<summary>Q67. Expression.Compile caching — why cache?</summary>

Answer:
- Compilation is relatively expensive.

Takeaway:
- Reuse compiled delegates for performance.
</details>

<details>
<summary>Q68. BindingFlags for private members — what's needed?</summary>

Answer:
- Need `BindingFlags.NonPublic | BindingFlags.Instance`.

Takeaway:
- Always specify binding flags when accessing non-public members.
</details>

---

[← Back to Main](../README.md) | [Previous: Span/Memory and Performance ←](14-span-memory-performance.md) | [Next: Compiler/JIT Optimizations →](16-compiler-jit-optimizations.md)
