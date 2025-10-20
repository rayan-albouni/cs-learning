# Attributes and Metadata

[← Back to Main](../README.md) | [Previous: Interop and unsafe ←](17-interop-unsafe.md) | [Next: Design Principles (SOLID) →](19-design-principles-solid.md)

---

<details>
<summary>Q75. Attribute parameters must be constants — what fails?</summary>

```csharp
const string A = "x";
string B = "y";
[Obsolete(A)] // ok
// [Obsolete(B)] // fails
```

Answer:
- Attribute arguments must be compile-time constants.

Takeaway:
- Use consts for attribute arguments.
</details>

<details>
<summary>Q76. Conditional attribute — does method get called?</summary>

```csharp
[Conditional("TRACE")]
static void Log(string s) => Console.WriteLine(s);
```

Answer:
- If TRACE is not defined, calls to Log are omitted entirely.

Takeaway:
- Conditional methods are compiled out when symbol is not defined.
</details>

---

[← Back to Main](../README.md) | [Previous: Interop and unsafe ←](17-interop-unsafe.md) | [Next: Design Principles (SOLID) →](19-design-principles-solid.md)
