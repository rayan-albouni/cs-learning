# Compiler/JIT Optimizations

[← Back to Main](../README.md) | [Previous: Reflection and Expression Trees ←](15-reflection-expression-trees.md) | [Next: Interop and unsafe →](17-interop-unsafe.md)

---

<details>
<summary>Q69. Bounds check elimination — when does it happen?</summary>

Answer:
- JIT can hoist bounds checks in simple loops.

Takeaway:
- Write simple loops for the JIT to optimize well.
</details>

<details>
<summary>Q70. AggressiveInlining attribute — should you use it?</summary>

Answer:
- It's a hint; the JIT may still ignore it.

Takeaway:
- Trust the JIT; use inlining hints sparingly.
</details>

<details>
<summary>Q71. volatile keyword — what does it guarantee?</summary>

Answer:
- `volatile` prevents certain reordering. It does not make compound operations atomic.

Takeaway:
- Use `volatile` for simple flags; use Interlocked or locks for compound operations.
</details>

---

[← Back to Main](../README.md) | [Previous: Reflection and Expression Trees ←](15-reflection-expression-trees.md) | [Next: Interop and unsafe →](17-interop-unsafe.md)
