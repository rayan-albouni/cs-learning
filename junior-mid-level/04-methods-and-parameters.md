# Methods and Parameters (ref/out/in), Return Values

[← Back to Main](../README.md) | [Previous: Control Flow ←](03-control-flow.md) | [Next: Classes, Structs, and Basic OOP →](05-classes-structs-basic-oop.md)

---

<details>
<summary>Q19. Method overloading basics — which overload is chosen?</summary>

```csharp
void M(int x) => Console.WriteLine("int");
void M(double x) => Console.WriteLine("double");
M(5);
M(5.0);
M(5f); // float
```

Answer:
- Prints `int`, `double`, `double`. `float` converts to double as the best match.

Takeaway:
- Overload resolution prefers the best conversion; beware ambiguous overloads.
</details>

<details>
<summary>Q20. ref parameter — does it modify the caller's variable?</summary>

```csharp
void Inc(ref int x) => x++;
int a = 1;
Inc(ref a);
Console.WriteLine(a);
```

Answer:
- Prints `2`. `ref` passes by reference, enabling in-place modification.

Takeaway:
- Use `ref` when in-place updates are necessary and clear to the caller.
</details>

<details>
<summary>Q21. out parameter — what must you do?</summary>

```csharp
bool TryParse(string s, out int value)
{
    // value++; // ?
    value = 0;
    return int.TryParse(s, out value);
}
```

Answer:
- `out` parameters must be definitely assigned before any return. You cannot read from an `out` parameter before assignment.

Takeaway:
- With `out`, the callee must assign; the caller need not initialize.
</details>

<details>
<summary>Q22. in parameter — what's allowed?</summary>

```csharp
void Sum(in int x, in int y)
{
    // x++; // not allowed
    Console.WriteLine(x + y);
}
```

Answer:
- `in` is a by-ref, read-only parameter; you cannot assign to it in the method.

Takeaway:
- `in` avoids copying for large structs and guarantees read-only semantics.
</details>

<details>
<summary>Q23. Return by value vs by ref — basics</summary>

```csharp
int[] arr = {1,2,3};
ref int RefToSecond(int[] a) => ref a[1];

ref int r = ref RefToSecond(arr);
r = 42;
Console.WriteLine(arr[1]);
```

Answer:
- Prints `42`. Returning by `ref` exposes a reference to the caller.

Takeaway:
- Ref returns are powerful but require care; they expose internal storage.
</details>

<details>
<summary>Q24. Named and optional parameters — which call is valid?</summary>

```csharp
void Greet(string name = "World", string prefix = "Hello")
{
    Console.WriteLine($"{prefix}, {name}!");
}

Greet();
Greet("Alice");
Greet(prefix: "Hi");
Greet(prefix: "Hi", name: "Bob");
```

Answer:
- All are valid. Outputs:
  - Hello, World!
  - Hello, Alice!
  - Hi, World!
  - Hi, Bob!

Takeaway:
- Optional parameters and named arguments improve call-site readability; avoid ambiguity with overloads.
</details>

---

[← Back to Main](../README.md) | [Previous: Control Flow ←](03-control-flow.md) | [Next: Classes, Structs, and Basic OOP →](05-classes-structs-basic-oop.md)
