# Interop and unsafe

[← Back to Main](../README.md) | [Previous: Compiler/JIT Optimizations ←](16-compiler-jit-optimizations.md) | [Next: Attributes and Metadata →](18-attributes-metadata.md)

---

<details>
<summary>Q72. stackalloc and Span — how to use safely?</summary>

```csharp
Span<byte> buf = stackalloc byte[128];
```

Answer:
- Safe within method; ref struct ensures no escape.

Takeaway:
- Use stackalloc for tiny, short-lived buffers.
</details>

<details>
<summary>Q73. fixed and string — what does fixed do?</summary>

Answer:
- `fixed` pins memory to prevent GC moves.

Takeaway:
- Pin sparingly; use Span and safe APIs where possible.
</details>

<details>
<summary>Q74. delegate* unmanaged function pointers — when to prefer?</summary>

Answer:
- Function pointers avoid delegate allocations and can be faster for interop/hot paths.

Takeaway:
- Function pointers are powerful but unsafe.
</details>

---

[← Back to Main](../README.md) | [Previous: Compiler/JIT Optimizations ←](16-compiler-jit-optimizations.md) | [Next: Attributes and Metadata →](18-attributes-metadata.md)
