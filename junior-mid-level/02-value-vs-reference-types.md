# Value vs Reference Types

[← Back to Main](../README.md) | [Previous: Variables, Data Types, and Type Inference ←](01-variables-data-types-type-inference.md) | [Next: Control Flow →](03-control-flow.md)

---

<details>
<summary>Q7. Value copy vs reference — what prints?</summary>

```csharp
struct Point { public int X; }
class RefPoint { public int X; }

var p = new Point { X = 1 };
var rp = new RefPoint { X = 1 };

var p2 = p;
var rp2 = rp;

p2.X = 2;
rp2.X = 2;

Console.WriteLine($"{p.X}, {rp.X}");
```

Question:
- What prints and why?

Answer:
- Prints `1, 2`. Struct assignment copies by value; class assignment copies the reference (both variables refer to same object).

Takeaway:
- Understand value vs reference semantics to reason about mutations.
</details>

<details>
<summary>Q8. Passing structs to methods — is it a copy?</summary>

```csharp
struct S { public int V; }
void Inc(S s) { s.V++; }

var s0 = new S { V = 1 };
Inc(s0);
Console.WriteLine(s0.V);
```

Question:
- What prints?

Answer:
- Prints `1`. Parameter is passed by value (copy). To mutate caller state, use `ref`.

Takeaway:
- Value types are copied by default. Use `ref` for in-place modification.
</details>

<details>
<summary>Q9. Boxing with object — memory behavior?</summary>

```csharp
int i = 42;
object o = i;   // boxing
i = 43;
Console.WriteLine(o); // ?
```

Question:
- What prints and why?

Answer:
- Prints `42`. Boxing creates a copy on the heap; later changes to `i` don't affect the boxed copy.

Takeaway:
- Boxing copies the value. Minimize boxing in performance-sensitive paths.
</details>

<details>
<summary>Q10. Nullable value types — HasValue?</summary>

```csharp
int? a = null;
int? b = 0;
Console.WriteLine($"{a.HasValue}, {b.HasValue}");
```

Question:
- What prints?

Answer:
- `False, True`. `0` is a valid value; null means no value.

Takeaway:
- `Nullable<T>` differentiates between null and default value of T.
</details>

<details>
<summary>Q11. Reference vs value equality basics</summary>

```csharp
var a = new int[] { 1 };
var b = new int[] { 1 };
Console.WriteLine(a == b);                 // ?
Console.WriteLine(Equals(a, b));           // ?
Console.WriteLine(ReferenceEquals(a, b));  // ?
```

Answer:
- `a == b` is False for arrays (reference comparison).
- `Equals(a, b)` is also reference equality for arrays, so False.
- `ReferenceEquals(a, b)` is False.

Takeaway:
- Arrays don't override equality to value semantics. For sequence equality, use `Enumerable.SequenceEqual`.
</details>

<details>
<summary>Q12. Strings: reference vs value equality</summary>

```csharp
string s1 = "abc";
string s2 = new string(new[] { 'a','b','c' });
Console.WriteLine(s1 == s2);
Console.WriteLine(object.ReferenceEquals(s1, s2));
```

Answer:
- `s1 == s2` is True (value equality).
- `ReferenceEquals(s1, s2)` is typically False (different instances), unless interning occurs.

Takeaway:
- Strings override `==` to mean value equality. Use `ReferenceEquals` only for identity checks, not content.
</details>

---

[← Back to Main](../README.md) | [Previous: Variables, Data Types, and Type Inference ←](01-variables-data-types-type-inference.md) | [Next: Control Flow →](03-control-flow.md)
