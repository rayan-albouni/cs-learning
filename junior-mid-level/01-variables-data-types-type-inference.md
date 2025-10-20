# Variables, Data Types, and Type Inference

[← Back to Main](../README.md) | [Next: Value vs Reference Types →](02-value-vs-reference-types.md)

---

<details>
<summary>Q1. var type inference — what is the type?</summary>

```csharp
var x = 5;
var y = 5.0;
var z = '5';
Console.WriteLine($"{x.GetType().Name}, {y.GetType().Name}, {z.GetType().Name}");
```

Question:
- What types are inferred for x, y, and z, and what prints?

Answer:
- x is int, y is double, z is char.
- Prints: Int32, Double, Char

Explanation:
- `var` is compile-time type inference. The assigned expression determines the static type.

Takeaway:
- Use `var` when the type is obvious from the right-hand side or not critical to readability.
</details>

<details>
<summary>Q2. default literals and target typing — does this compile?</summary>

```csharp
var a = default(int);
var b = default(string);
var c = default; // ?
```

Question:
- Which lines compile, and what values are assigned?

Answer:
- First two compile: `a == 0`, `b == null`.
- `var c = default;` does not compile because there is no target type for the default literal.

Takeaway:
- `default` without a target type is invalid; use `default(T)` or an explicit cast.
</details>

<details>
<summary>Q3. Decimal vs double — what's the output?</summary>

```csharp
double dx = 0.1 + 0.2;
decimal m = 0.1m + 0.2m;
Console.WriteLine(dx == 0.3);
Console.WriteLine(m == 0.3m);
```

Question:
- Which comparisons are true?

Answer:
- `dx == 0.3` is False (binary floating-point precision).
- `m == 0.3m` is True (decimal is base-10 friendly).

Takeaway:
- Use `decimal` for financial calculations; double is binary floating-point and non-exact for many decimals.
</details>

<details>
<summary>Q4. Implicit numeric conversions — which compile?</summary>

```csharp
int i = 100;
long l = i;        // ok
float f = i;       // ok (lossy possible)
int i2 = l;        // ?
double d = f;      // ok
```

Question:
- Which assignment fails to compile and why?

Answer:
- `int i2 = l;` fails because narrowing conversions require an explicit cast.

Takeaway:
- Widening conversions are implicit; narrowing conversions require casts. Be careful with potential data loss.
</details>

<details>
<summary>Q5. Constant expressions — what's allowed?</summary>

```csharp
const int A = 10;
const int B = A + 5;       // ok
var rand = new Random();
const int C = rand.Next(); // ?
```

Question:
- Does the last line compile?

Answer:
- No. `const` requires compile-time constants. `Random.Next()` is runtime.

Takeaway:
- Use `const` for compile-time constants only; otherwise use `readonly` fields for runtime immutability.
</details>

<details>
<summary>Q6. Explicit casts vs Convert — what's printed?</summary>

```csharp
double d = 9.7;
int a = (int)d;
int b = Convert.ToInt32(d);
Console.WriteLine($"{a}, {b}");
```

Question:
- What prints and why?

Answer:
- Prints `9, 10`. Cast truncates; `Convert.ToInt32` rounds to nearest even (MidpointRounding.ToEven by default), resulting in 10 here.

Takeaway:
- Casting truncates toward zero; Convert performs rounding. Choose deliberately.
</details>

---

[← Back to Main](../README.md) | [Next: Value vs Reference Types →](02-value-vs-reference-types.md)
