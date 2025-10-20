# The Ultimate .NET and C# Interview Questions

Audience: Junior, Mid-level, Senior, and Staff .NET engineers. Version assumptions: C# 12, .NET 8+.
## Junior and Mid-level Engineers
### Table of Contents

- Variables, Data Types, and Type Inference
- Value vs Reference Types
- Control Flow (if/else, switch, loops)
- Methods and Parameters (ref/out/in), Return Values
- Classes, Structs, and Basic OOP (Inheritance, Encapsulation, Polymorphism)
- Interfaces and Abstract Classes
- Collections and Arrays
- Exceptions and Basic Error Handling
- Strings and Basic Formatting/Interpolation
- Basic LINQ (Select, Where, First, Count, etc.)
- Simple async/await and Task Basics
- File I/O Fundamentals
- Simple Unit Testing

---

## Variables, Data Types, and Type Inference

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
<summary>Q3. Decimal vs double — what’s the output?</summary>

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
<summary>Q5. Constant expressions — what’s allowed?</summary>

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
<summary>Q6. Explicit casts vs Convert — what’s printed?</summary>

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

## Value vs Reference Types

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
- Prints `42`. Boxing creates a copy on the heap; later changes to `i` don’t affect the boxed copy.

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
- Arrays don’t override equality to value semantics. For sequence equality, use `Enumerable.SequenceEqual`.
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

## Control Flow (if/else, switch, loops)

<details>
<summary>Q13. if/else chaining — which branch executes?</summary>

```csharp
int x = 10;
if (x > 10) Console.Write("A");
else if (x == 10) Console.Write("B");
else Console.Write("C");
```

Answer:
- Prints `B`. Conditions are evaluated in order until one matches.

Takeaway:
- Order matters with if/else chains; place more specific conditions first.
</details>

<details>
<summary>Q14. switch expression basics — what prints?</summary>

```csharp
int n = 2;
string result = n switch
{
    1 => "one",
    2 => "two",
    _ => "other"
};
Console.WriteLine(result);
```

Answer:
- Prints `two`.

Takeaway:
- Switch expressions are concise and exhaustive. Use `_` for the default case.
</details>

<details>
<summary>Q15. for loop variable scope — what prints?</summary>

```csharp
for (int i = 0; i < 3; i++) { }
Console.WriteLine(i); // ?
```

Answer:
- Does not compile. `i` is scoped to the for loop.

Takeaway:
- Loop variables are local to the loop construct.
</details>

<details>
<summary>Q16. while vs do-while — how many prints?</summary>

```csharp
int i = 0;
while (i > 0) Console.WriteLine("W");
do { Console.WriteLine("D"); } while (i > 0);
```

Answer:
- Prints `D` once; while prints nothing. do-while executes the body at least once.

Takeaway:
- Use do-while when the body must run at least once before checking the condition.
</details>

<details>
<summary>Q17. break vs continue — what’s the output?</summary>

```csharp
for (int i = 0; i < 5; i++)
{
    if (i == 2) continue;
    if (i == 4) break;
    Console.Write(i);
}
```

Answer:
- Prints `013`. `continue` skips the rest of the loop for `i==2`; `break` exits when `i==4`.

Takeaway:
- `continue` skips current iteration; `break` exits the loop entirely.
</details>

<details>
<summary>Q18. Basic pattern matching with type — what prints?</summary>

```csharp
object o = 5;
if (o is int v) Console.WriteLine(v + 1);
```

Answer:
- Prints `6`. The `is` pattern both checks type and introduces a variable.

Takeaway:
- Pattern matching reduces casting boilerplate and improves readability.
</details>

---

## Methods and Parameters (ref/out/in), Return Values

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
<summary>Q20. ref parameter — does it modify the caller’s variable?</summary>

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
<summary>Q22. in parameter — what’s allowed?</summary>

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

## Classes, Structs, and Basic OOP

<details>
<summary>Q25. Constructor overloading — which one runs?</summary>

```csharp
class C
{
    public C() : this(0) { Console.WriteLine("Default"); }
    public C(int x) { Console.WriteLine($"C(int={x})"); }
}
var c = new C();
```

Answer:
- Prints:
  - C(int=0)
  - Default

Takeaway:
- Use constructor chaining to centralize initialization logic.
</details>

<details>
<summary>Q26. Encapsulation — property with backing field</summary>

```csharp
class Person
{
    private int _age;
    public int Age
    {
        get => _age;
        set => _age = value < 0 ? 0 : value;
    }
}
var p = new Person { Age = -5 };
Console.WriteLine(p.Age);
```

Answer:
- Prints `0`. Setter enforces invariant.

Takeaway:
- Encapsulation protects invariants; prefer properties over public fields.
</details>

<details>
<summary>Q27. Inheritance and virtual override — which method prints?</summary>

```csharp
class Base { public virtual void Speak() => Console.WriteLine("Base"); }
class Derived : Base { public override void Speak() => Console.WriteLine("Derived"); }

Base b = new Derived();
b.Speak();
```

Answer:
- Prints `Derived`. Virtual dispatch uses runtime type.

Takeaway:
- Polymorphism is enabled via virtual/override.
</details>

<details>
<summary>Q28. Method hiding with new — which method prints?</summary>

```csharp
class B { public void Show() => Console.WriteLine("B.Show"); }
class D : B { public new void Show() => Console.WriteLine("D.Show"); }

B b = new D();
D d = new D();
b.Show();
d.Show();
```

Answer:
- Prints:
  - B.Show
  - D.Show

Takeaway:
- `new` hides non-virtual members; dispatch is based on static type, not runtime.
</details>

<details>
<summary>Q29. Struct default constructor — what values?</summary>

```csharp
struct S
{
    public int X;
    public bool B;
}
var s = new S();
Console.WriteLine($"{s.X}, {s.B}");
```

Answer:
- Prints `0, False`. Value types are zero-initialized by default.

Takeaway:
- Structs have an implicit default constructor that zero-initializes fields.
</details>

<details>
<summary>Q30. Readonly auto-properties with init setters</summary>

```csharp
class Book
{
    public string Title { get; init; } = "Untitled";
}
var book = new Book { Title = "C# Basics" };
// book.Title = "X"; // ?
Console.WriteLine(book.Title);
```

Answer:
- Prints `C# Basics`. Attempting to set Title after object initialization is a compile error.

Takeaway:
- `init` enables immutable-by-convention properties settable during object initialization.
</details>

---

## Interfaces and Abstract Classes

<details>
<summary>Q31. Implementing an interface — which member must be provided?</summary>

```csharp
interface IGreeter { void Greet(string name); }
abstract class GreeterBase : IGreeter
{
    public abstract void Greet(string name);
}
class ConsoleGreeter : GreeterBase
{
    public override void Greet(string name) => Console.WriteLine($"Hello, {name}");
}
```

Question:
- Is this valid and why?

Answer:
- Yes. Interface contract is fulfilled by the derived concrete class via abstract override.

Takeaway:
- Abstract classes can partially implement interfaces; concrete subclasses must complete the contract.
</details>

<details>
<summary>Q32. Abstract class instantiation — allowed?</summary>

```csharp
abstract class Shape { public abstract double Area(); }
var s = new Shape(); // ?
```

Answer:
- Does not compile. Abstract classes cannot be instantiated.

Takeaway:
- Abstract classes define contracts + shared behavior; only concrete subclasses instantiate.
</details>

<details>
<summary>Q33. Explicit interface implementation — which method is called?</summary>

```csharp
interface I { void M(); }
class C : I
{
    void I.M() => Console.WriteLine("I.M");
    public void M() => Console.WriteLine("C.M");
}
var c = new C();
c.M();
((I)c).M();
```

Answer:
- Prints:
  - C.M
  - I.M

Takeaway:
- Explicit implementations are only accessible via the interface reference; useful to avoid name clashes.
</details>

<details>
<summary>Q34. Multiple interfaces with same member name</summary>

```csharp
interface IA { void M(); }
interface IB { void M(); }
class C : IA, IB
{
    public void M() => Console.WriteLine("C.M");
}
IA a = new C();
IB b = new C();
a.M(); b.M();
```

Answer:
- Both calls print `C.M`. Single public method can satisfy both interfaces if semantics match.

Takeaway:
- One implementation can satisfy multiple interface contracts with identical signatures.
</details>

<details>
<summary>Q35. Default interface methods — can they provide behavior?</summary>

```csharp
interface I
{
    void A();
    void B() => Console.WriteLine("Default B");
}
class C : I
{
    public void A() => Console.WriteLine("C.A");
}
I i = new C();
i.A(); i.B();
```

Answer:
- Prints:
  - C.A
  - Default B

Takeaway:
- Interfaces can provide default implementations (C# 8+); still use judiciously to keep contracts clear.
</details>

<details>
<summary>Q36. Sealed override in inheritance chain</summary>

```csharp
class B { public virtual void M() {} }
class D : B
{
    public sealed override void M() { }
}
class E : D
{
    // public override void M() {} // ?
}
```

Answer:
- `E` cannot override `M`; `sealed override` stops further overriding.

Takeaway:
- Use `sealed override` to lock down an override at a specific point in the hierarchy.
</details>

---

## Collections and Arrays

<details>
<summary>Q37. Array indexing — what prints?</summary>

```csharp
int[] a = { 10, 20, 30 };
Console.WriteLine($"{a[0]}, {a[^1]}");
```

Answer:
- Prints `10, 30`. `^1` is the last element (C# index-from-end).

Takeaway:
- Use `^` and ranges for expressive indexing in arrays and spans.
</details>

<details>
<summary>Q38. Range slicing — what’s the result?</summary>

```csharp
int[] a = { 1,2,3,4,5 };
int[] slice = a[1..4];
Console.WriteLine(string.Join(",", slice));
```

Answer:
- Prints `2,3,4`. Ranges are end-exclusive.

Takeaway:
- Ranges `[start..end)` are zero-based and end-exclusive.
</details>

<details>
<summary>Q39. List capacity vs Count — what changes?</summary>

```csharp
var list = new List<int>(capacity: 1);
list.Add(1);
list.Add(2);
Console.WriteLine($"{list.Count}, {list.Capacity >= 2}");
```

Answer:
- Prints `2, True`. Capacity grows automatically as needed.

Takeaway:
- List auto-resizes; pre-sizing can reduce reallocations for known sizes.
</details>

<details>
<summary>Q40. Dictionary key lookup — what prints?</summary>

```csharp
var dict = new Dictionary<string, int>
{
    ["a"] = 1
};
Console.WriteLine(dict.ContainsKey("a"));
Console.WriteLine(dict.TryGetValue("b", out var v));
Console.WriteLine(v);
```

Answer:
- Prints:
  - True
  - False
  - 0

Explanation:
- When TryGetValue fails, `out v` is set to default (0 for int).

Takeaway:
- Use `TryGetValue` to avoid exceptions and extra lookups.
</details>

<details>
<summary>Q41. Queue and Stack basics — what order?</summary>

```csharp
var q = new Queue<int>();
q.Enqueue(1); q.Enqueue(2); q.Enqueue(3);
Console.WriteLine(q.Dequeue()); // ?

var s = new Stack<int>();
s.Push(1); s.Push(2); s.Push(3);
Console.WriteLine(s.Pop()); // ?
```

Answer:
- Dequeue prints `1` (FIFO).
- Pop prints `3` (LIFO).

Takeaway:
- Queue is FIFO; Stack is LIFO. Choose based on access pattern.
</details>

<details>
<summary>Q42. foreach vs for — can you modify the collection?</summary>

```csharp
var list = new List<int> {1,2,3};
foreach (var n in list)
{
    // list.Add(4); // ?
    Console.Write(n);
}
```

Answer:
- Modifying the collection during `foreach` iteration throws `InvalidOperationException`.

Takeaway:
- Don’t structurally modify a collection while enumerating it. Collect changes and apply later.
</details>

---

## Exceptions and Basic Error Handling

<details>
<summary>Q43. Catching specific exception types — which catch runs?</summary>

```csharp
try
{
    int x = int.Parse("not-an-int");
}
catch (FormatException)
{
    Console.WriteLine("Format");
}
catch (Exception)
{
    Console.WriteLine("General");
}
```

Answer:
- Prints `Format`. More specific catch should appear before general catch.

Takeaway:
- Order catch blocks from most specific to most general.
</details>

<details>
<summary>Q44. finally block behavior — does it always run?</summary>

```csharp
try
{
    Console.WriteLine("Try");
}
finally
{
    Console.WriteLine("Finally");
}
```

Answer:
- Prints:
  - Try
  - Finally

Takeaway:
- `finally` runs whether or not an exception is thrown, including most early returns; avoid `Environment.FailFast` or process termination if cleanup is required.
</details>

<details>
<summary>Q45. Throwing your own exception — best practice?</summary>

```csharp
if (filePath is null)
    throw new ArgumentNullException(nameof(filePath));
```

Question:
- Why use `nameof` and specific exception types?

Answer:
- `nameof` reduces magic strings; specific exception types communicate intent and enable targeted handling.

Takeaway:
- Throw the most specific applicable exception and use `nameof` for parameter names.
</details>

<details>
<summary>Q46. Try-catch around minimal code</summary>

```csharp
try
{
    // many lines of code
    var content = File.ReadAllText("missing.txt");
    // many more lines
}
catch (IOException ex)
{
    Console.WriteLine(ex.Message);
}
```

Question:
- What’s a better structure?

Answer:
- Wrap only the risky call in try-catch or separate it into a method to avoid catching unrelated errors.

Takeaway:
- Keep try blocks small and focused on the operations that can throw the expected exceptions.
</details>

<details>
<summary>Q47. Re-throwing with `throw;` preserves stack</summary>

```csharp
try
{
    MightThrow();
}
catch (Exception)
{
    // log
    throw;
}
```

Answer:
- `throw;` rethrows with original stack trace intact.

Takeaway:
- Prefer `throw;` to preserve stack trace; avoid `throw ex;` unless you intentionally reset context.
</details>

---

## Strings and Basic Formatting/Interpolation

<details>
<summary>Q48. String interpolation — what prints?</summary>

```csharp
int x = 5;
Console.WriteLine($"x = {x}, x+1 = {x + 1}");
```

Answer:
- Prints `x = 5, x+1 = 6`.

Takeaway:
- Interpolation embeds expressions inside strings; prefer for clarity.
</details>

<details>
<summary>Q49. Verbatim strings and escaping</summary>

```csharp
var path1 = "C:\\temp\\file.txt";
var path2 = @"C:\temp\file.txt";
Console.WriteLine(path1 == path2);
```

Answer:
- Prints `True`. Verbatim strings `@""` avoid needing to escape backslashes.

Takeaway:
- Use verbatim strings for paths and multiline text; double quotes are escaped as `""`.
</details>

<details>
<summary>Q50. String.Format and standard numeric formats</summary>

```csharp
double n = 1234.567;
Console.WriteLine(string.Format("{0:F2}", n));
Console.WriteLine($"{n:N1}");
```

Answer:
- Prints:
  - 1234.57
  - 1,234.6 (culture-dependent thousands separator)

Takeaway:
- Use standard numeric format strings; remember they are culture-sensitive by default.
</details>

<details>
<summary>Q51. String immutability — what prints?</summary>

```csharp
string s = "a";
s += "b";
Console.WriteLine(s);
```

Answer:
- Prints `ab`. Each concatenation creates a new string.

Takeaway:
- Strings are immutable. For repeated concatenations, use `StringBuilder` or `string.Create` when performance matters.
</details>

<details>
<summary>Q52. Case-insensitive comparisons</summary>

```csharp
string a = "Hello";
string b = "hello";
Console.WriteLine(a.Equals(b, StringComparison.OrdinalIgnoreCase));
```

Answer:
- Prints `True`.

Takeaway:
- Always specify `StringComparison` to avoid culture-related surprises.
</details>

---

## Basic LINQ (Select, Where, First, Count, etc.)

<details>
<summary>Q53. Select and Where — what’s the output?</summary>

```csharp
var nums = new[] { 1,2,3,4,5 };
var evensTimesTwo = nums.Where(n => n % 2 == 0).Select(n => n * 2);
Console.WriteLine(string.Join(",", evensTimesTwo));
```

Answer:
- Prints `4,8`.

Takeaway:
- LINQ pipelines are readable and composable for simple queries.
</details>

<details>
<summary>Q54. First vs FirstOrDefault — difference?</summary>

```csharp
var empty = Array.Empty<int>();
// var x = empty.First(); // ?
var y = empty.FirstOrDefault();
Console.WriteLine(y);
```

Answer:
- `First()` would throw `InvalidOperationException`.
- `FirstOrDefault()` returns default(int) which is `0`.

Takeaway:
- Use `FirstOrDefault` when sequences may be empty, then check the result or use nullable types.
</details>

<details>
<summary>Q55. Count vs Any — performance choice?</summary>

```csharp
var items = Enumerable.Range(1, 10);
// bool hasAny = items.Count() > 0; // less efficient
bool hasAny = items.Any();          // efficient
Console.WriteLine(hasAny);
```

Answer:
- Both print `True`. `Any()` stops early; `Count()` may iterate fully for IEnumerable.

Takeaway:
- Prefer `Any()` to check non-emptiness of an IEnumerable.
</details>

<details>
<summary>Q56. ToList materialization prevents multiple enumeration side effects</summary>

```csharp
var query = Enumerable.Range(1, 3).Select(n => {
    Console.Write(n);
    return n;
});
var list = query.ToList(); // prints during enumeration
var sum = list.Sum();      // no extra prints
```

Answer:
- Prints `123` once.

Takeaway:
- Deferred sequences are re-enumerated each time; materialize when reusing.
</details>

<details>
<summary>Q57. OrderBy and ThenBy basics</summary>

```csharp
var people = new[]
{
    new { First="Alice", Last="Z" },
    new { First="Bob", Last="Z" },
    new { First="Bob", Last="A" },
};
var ordered = people.OrderBy(p => p.Last).ThenBy(p => p.First);
Console.WriteLine(string.Join(" | ", ordered.Select(p => $"{p.Last},{p.First}")));
```

Answer:
- Prints `A,Bob | Z,Alice | Z,Bob`.

Takeaway:
- `OrderBy` establishes primary key; `ThenBy` refines with secondary keys.
</details>

<details>
<summary>Q58. SelectMany flattens sequences</summary>

```csharp
var words = new[] { "a b", "c d" };
var parts = words.SelectMany(w => w.Split(' '));
Console.WriteLine(string.Join(",", parts));
```

Answer:
- Prints `a,b,c,d`.

Takeaway:
- `SelectMany` flattens nested sequences into a single sequence.
</details>

---

## Simple async/await and Task Basics

<details>
<summary>Q59. Basic await — what prints order?</summary>

```csharp
async Task Demo()
{
    Console.WriteLine("Start");
    await Task.Delay(50);
    Console.WriteLine("End");
}
await Demo();
```

Answer:
- Prints:
  - Start
  - End

Takeaway:
- `await` asynchronously yields control; execution resumes when the awaited task completes.
</details>

<details>
<summary>Q60. Task.Run to offload work</summary>

```csharp
int Compute() { Thread.Sleep(50); return 42; }
int result = await Task.Run(Compute);
Console.WriteLine(result);
```

Answer:
- Prints `42`. `Task.Run` executes CPU-bound work on a thread pool thread.

Takeaway:
- Use `Task.Run` to offload CPU-bound work; don’t wrap inherently asynchronous I/O with Task.Run unnecessarily.
</details>

<details>
<summary>Q61. Async method returning Task — exceptions</summary>

```csharp
async Task<int> F()
{
    await Task.Delay(10);
    throw new InvalidOperationException("boom");
}
try
{
    var x = await F();
}
catch (InvalidOperationException)
{
    Console.WriteLine("Caught");
}
```

Answer:
- Prints `Caught`. Exceptions in async methods are captured into the Task and rethrown on await.

Takeaway:
- Await to observe exceptions; don’t ignore tasks or use async void (except event handlers).
</details>

<details>
<summary>Q62. Multiple awaits in sequence — order?</summary>

```csharp
await Task.Delay(10);
await Task.Delay(10);
Console.WriteLine("Done");
```

Answer:
- Executes sequentially; total delay ~20ms plus overhead.

Takeaway:
- For independent operations, consider starting tasks concurrently and awaiting with `await Task.WhenAll(...)`.
</details>

<details>
<summary>Q63. Returning completed tasks — fast path</summary>

```csharp
Task<int> Return42() => Task.FromResult(42);
Console.WriteLine(await Return42());
```

Answer:
- Prints `42`. `Task.FromResult` returns an already-completed Task without allocation of a state machine.

Takeaway:
- Use `Task.FromResult` for synchronous, already-known results.
</details>

---

## File I/O Fundamentals

<details>
<summary>Q64. Reading and writing text files — basics</summary>

```csharp
string path = "hello.txt";
File.WriteAllText(path, "Hi");
string content = File.ReadAllText(path);
Console.WriteLine(content);
```

Answer:
- Prints `Hi`.

Takeaway:
- `File.WriteAllText` and `ReadAllText` are convenient for small text files.
</details>

<details>
<summary>Q65. Using statements and streams — disposal order</summary>

```csharp
using var fs = File.OpenRead("hello.txt");
using var sr = new StreamReader(fs);
Console.WriteLine(await sr.ReadToEndAsync());
```

Answer:
- Properly disposes `StreamReader` then `FileStream` at the end of scope.

Takeaway:
- Use `using` declarations for deterministic disposal of I/O resources.
</details>

<details>
<summary>Q66. Path.Combine vs string concatenation</summary>

```csharp
string folder = "C:\\temp";
string file = "data.txt";
Console.WriteLine(Path.Combine(folder, file));
```

Answer:
- Prints `C:\temp\data.txt`. `Path.Combine` handles separators and edge cases.

Takeaway:
- Use `Path.Combine` for safe, cross-platform path building.
</details>

<details>
<summary>Q67. File.WriteAllLines and ReadAllLines</summary>

```csharp
string path = "lines.txt";
File.WriteAllLines(path, new[] { "a", "b", "c" });
Console.WriteLine(string.Join("-", File.ReadAllLines(path)));
```

Answer:
- Prints `a-b-c`.

Takeaway:
- Use the `AllLines` helpers for small batches of lines.
</details>

<details>
<summary>Q68. Async file read — don’t block</summary>

```csharp
await File.WriteAllTextAsync("data.txt", "content");
string text = await File.ReadAllTextAsync("data.txt");
Console.WriteLine(text);
```

Answer:
- Prints `content`.

Takeaway:
- Prefer async I/O on server apps to avoid blocking threads.
</details>

---

## Simple Unit Testing

<details>
<summary>Q69. Arrange-Act-Assert — basics</summary>

```csharp
// Pseudo xUnit
int Add(int a, int b) => a + b;

[Fact]
public void Add_AddsTwoNumbers()
{
    // Arrange
    var a = 2; var b = 3;

    // Act
    var result = Add(a, b);

    // Assert
    Assert.Equal(5, result);
}
```

Answer:
- Test passes. Clear AAA layout improves readability.

Takeaway:
- Structure tests with Arrange, Act, Assert for clarity and maintainability.
</details>

<details>
<summary>Q70. Parameterized tests reduce duplication</summary>

```csharp
[Theory]
[InlineData(1, 2, 3)]
[InlineData(-1, 1, 0)]
public void Add_Works(int a, int b, int expected)
{
    Assert.Equal(expected, a + b);
}
```

Answer:
- Runs multiple scenarios succinctly.

Takeaway:
- Use data-driven tests to cover more cases with less repetition.
</details>

<details>
<summary>Q71. Testing exceptions — expected throws</summary>

```csharp
void RequireNonNull(string s)
{
    if (s is null) throw new ArgumentNullException(nameof(s));
}
[Fact]
public void RequireNonNull_ThrowsOnNull()
{
    Assert.Throws<ArgumentNullException>(() => RequireNonNull(null!));
}
```

Answer:
- Test passes when exception is thrown.

Takeaway:
- Use `Assert.Throws` (or `Assert.ThrowsAsync`) for exception-based behavior.
</details>

<details>
<summary>Q72. Avoid async void in tests</summary>

```csharp
[Fact]
public async Task TestAsync()
{
    await Task.Delay(10);
    Assert.True(true);
}
```

Answer:
- Correct. Tests should return `Task` for async operations.

Takeaway:
- `async void` cannot be awaited and may cause false positives or lost exceptions.
</details>

<details>
<summary>Q73. Floating-point tolerance in tests</summary>

```csharp
[Fact]
public void Double_Sum_Tolerance()
{
    var sum = 0.1 + 0.2;
    Assert.True(Math.Abs(sum - 0.3) < 1e-12);
}
```

Answer:
- Test passes with a reasonable tolerance.

Takeaway:
- Use tolerances for floating-point comparisons; avoid exact equality.
</details>

---

# Extended Fundamentals — Additional Quick Puzzles

<details>
<summary>Q74. Null-coalescing operator ??</summary>

```csharp
string? s = null;
string r = s ?? "default";
Console.WriteLine(r);
```

Answer:
- Prints `default`.

Takeaway:
- `??` provides fallback when the left side is null.
</details>

<details>
<summary>Q75. Null-conditional operator ?.</summary>

```csharp
string? s = null;
Console.WriteLine(s?.ToUpper() ?? "none");
```

Answer:
- Prints `none`.

Takeaway:
- `?.` short-circuits safely on null; combine with `??` to provide defaults.
</details>

<details>
<summary>Q76. Ternary conditional operator basics</summary>

```csharp
int n = 5;
string r = n % 2 == 0 ? "even" : "odd";
Console.WriteLine(r);
```

Answer:
- Prints `odd`.

Takeaway:
- Use the ternary operator for simple, concise conditional expressions.
</details>

<details>
<summary>Q77. Enum basics and casting</summary>

```csharp
enum Color { Red = 1, Green = 2, Blue = 3 }
Color c = (Color)2;
Console.WriteLine(c);
```

Answer:
- Prints `Green`.

Takeaway:
- Enums map named constants to integral values; casting between underlying integral type and enum is allowed.
</details>

<details>
<summary>Q78. TryParse pattern</summary>

```csharp
if (int.TryParse("123", out int val))
    Console.WriteLine(val);
else
    Console.WriteLine("invalid");
```

Answer:
- Prints `123`.

Takeaway:
- Prefer `TryParse` to avoid exceptions during parsing.
</details>

<details>
<summary>Q79. DateTime formatting basics</summary>

```csharp
var dt = new DateTime(2025, 1, 2, 15, 4, 5);
Console.WriteLine(dt.ToString("yyyy-MM-dd HH:mm:ss"));
```

Answer:
- Prints `2025-01-02 15:04:05`.

Takeaway:
- Use format strings for consistent date/time output; beware of culture defaults when not specifying formats.
</details>

<details>
<summary>Q80. String.Join vs concatenation in loops</summary>

```csharp
var arr = new[] { "a", "b", "c" };
Console.WriteLine(string.Join(",", arr));
```

Answer:
- Prints `a,b,c`.

Takeaway:
- `string.Join` is efficient and concise for joining collections.
</details>

<details>
<summary>Q81. IndexOf returns -1 when not found</summary>

```csharp
string s = "abc";
Console.WriteLine(s.IndexOf("d")); // ?
```

Answer:
- Prints `-1`.

Takeaway:
- Check for `-1` before slicing or indexing from search results.
</details>

<details>
<summary>Q82. Array.Sort in-place</summary>

```csharp
int[] a = {3,1,2};
Array.Sort(a);
Console.WriteLine(string.Join(",", a));
```

Answer:
- Prints `1,2,3`.

Takeaway:
- `Array.Sort` mutates the array; `OrderBy` creates a new sequence.
</details>

<details>
<summary>Q83. Null checks with guard clauses</summary>

```csharp
void Process(string input)
{
    ArgumentNullException.ThrowIfNull(input);
    Console.WriteLine(input.Length);
}
```

Answer:
- Throws when `input` is null; otherwise prints the length.

Takeaway:
- Use guard clauses early to validate inputs and simplify function bodies.
</details>

<details>
<summary>Q84. Using StringComparison with StartsWith</summary>

```csharp
Console.WriteLine("straße".StartsWith("STR", StringComparison.CurrentCultureIgnoreCase));
```

Answer:
- Culture-sensitive; may return True depending on culture rules. For predictable results, use `OrdinalIgnoreCase` when culture-insensitive comparison is desired.

Takeaway:
- Choose `StringComparison` explicitly to avoid ambiguous behavior across cultures.
</details>

<details>
<summary>Q85. Any vs All</summary>

```csharp
var nums = new[] { 2, 4, 6 };
Console.WriteLine(nums.Any(n => n % 2 != 0)); // any odd?
Console.WriteLine(nums.All(n => n % 2 == 0)); // all even?
```

Answer:
- Prints:
  - False
  - True

Takeaway:
- `Any` tests existence; `All` tests universality.
</details>

<details>
<summary>Q86. Select with index overload</summary>

```csharp
var v = new[] { "a", "b" }.Select((s, i) => $"{i}:{s}");
Console.WriteLine(string.Join(",", v));
```

Answer:
- Prints `0:a,1:b`.

Takeaway:
- Use the index-aware overload for position-dependent projections.
</details>

<details>
<summary>Q87. Simple Task.WhenAll</summary>

```csharp
var tasks = new[]
{
    Task.Delay(10),
    Task.Delay(10)
};
await Task.WhenAll(tasks);
Console.WriteLine("All done");
```

Answer:
- Prints `All done` after both complete.

Takeaway:
- `Task.WhenAll` awaits all tasks concurrently and propagates exceptions.
</details>

<details>
<summary>Q88. File.Exists before reading</summary>

```csharp
string path = "maybe.txt";
if (File.Exists(path))
    Console.WriteLine(File.ReadAllText(path));
else
    Console.WriteLine("Missing");
```

Answer:
- Prints file content if present; else prints `Missing`.

Takeaway:
- Check file existence to avoid exceptions; still handle TOCTOU in robust code (the file may be deleted between check and read).
</details>

<details>
<summary>Q89. StreamReader ReadLine loop</summary>

```csharp
using var sr = new StreamReader("lines.txt");
string? line;
while ((line = sr.ReadLine()) is not null)
{
    Console.WriteLine(line);
}
```

Answer:
- Reads and prints lines until EOF.

Takeaway:
- Classic pattern for line-by-line file processing.
</details>

<details>
<summary>Q90. Basic stopwatch timing</summary>

```csharp
var sw = System.Diagnostics.Stopwatch.StartNew();
Thread.Sleep(50);
sw.Stop();
Console.WriteLine(sw.ElapsedMilliseconds >= 50);
```

Answer:
- Prints `True`.

Takeaway:
- Use `Stopwatch` for simple performance measurements; prefer multiple iterations for stability.
</details>

<details>
<summary>Q91. Dictionary iteration order</summary>

```csharp
var d = new Dictionary<int,string> { [2]="b", [1]="a" };
Console.WriteLine(string.Join(",", d.Keys));
```

Answer:
- In .NET Core/.NET 5+ insertion order is preserved for Dictionary iteration as an implementation detail, but do not rely on it for cross-platform/compat unless documented.

Takeaway:
- If you need sorted order, use `SortedDictionary` or sort separately.
</details>

<details>
<summary>Q92. Null forgiving (!) and actual null values</summary>

```csharp
string? s = null;
Console.WriteLine(s!.Length); // ?
```

Answer:
- Compiles but throws `NullReferenceException` at runtime.

Takeaway:
- `!` only suppresses compiler warnings; it doesn’t prevent runtime nulls.
</details>

<details>
<summary>Q93. Try-catch around parsing with default</summary>

```csharp
int value;
try
{
    value = int.Parse("abc");
}
catch
{
    value = 0;
}
Console.WriteLine(value);
```

Answer:
- Prints `0`.

Takeaway:
- Prefer `TryParse` to avoid exceptions; use try-catch for exceptional flows.
</details>

<details>
<summary>Q94. Environment.NewLine vs "\n"</summary>

```csharp
Console.Write("A" + Environment.NewLine + "B");
```

Answer:
- Prints A and B on separate lines, using the platform-specific newline.

Takeaway:
- Use `Environment.NewLine` for platform-correct line breaks in non-interpolated contexts.
</details>

<details>
<summary>Q95. Using directive aliases for clarity</summary>

```csharp
using IO = System.IO;
IO.File.WriteAllText("x.txt", "content");
```

Answer:
- Writes to file; alias improves readability when frequently using a namespace.

Takeaway:
- Namespace aliases can reduce verbosity in heavily-used namespaces.
</details>

<details>
<summary>Q96. Basic switch over enums with default</summary>

```csharp
enum Op { Add, Sub }
int Apply(int a, int b, Op op) => op switch
{
    Op.Add => a + b,
    Op.Sub => a - b,
    _ => throw new NotSupportedException()
};
Console.WriteLine(Apply(3,2,Op.Sub));
```

Answer:
- Prints `1`.

Takeaway:
- Switch expressions over enums are concise; include a default case for future-proofing.
</details>

<details>
<summary>Q97. StringBuilder for repeated concatenation</summary>

```csharp
var sb = new System.Text.StringBuilder();
for (int i = 0; i < 3; i++) sb.Append(i);
Console.WriteLine(sb.ToString());
```

Answer:
- Prints `012`.

Takeaway:
- Prefer `StringBuilder` for many concatenations in loops.
</details>

<details>
<summary>Q98. Basic tuple return and deconstruction</summary>

```csharp
(int sum, int diff) Calc(int a, int b) => (a+b, a-b);
var (s, d) = Calc(5, 3);
Console.WriteLine($"{s},{d}");
```

Answer:
- Prints `8,2`.

Takeaway:
- Tuples are handy for returning multiple values without a class.
</details>

<details>
<summary>Q99. Simple record for immutable data</summary>

```csharp
public record Point(int X, int Y);
var p1 = new Point(1,2);
var p2 = p1 with { X = 3 };
Console.WriteLine($"{p1}, {p2}");
```

Answer:
- Prints `Point { X = 1, Y = 2 }, Point { X = 3, Y = 2 }`.

Takeaway:
- Records provide concise immutable data carriers with value semantics.
</details>

<details>
<summary>Q100. Minimal Program and top-level statements</summary>

```csharp
Console.WriteLine("Hello, world!");
```

Answer:
- In C# 9+, you can write top-level statements without an explicit Program class.

Takeaway:
- Top-level statements simplify small apps and teaching examples.
</details>

---



## Senior and Staff Engineers
### Table of Contents

- Core C# Language Behavior
  - Numeric conversions, overflow, checked/unchecked
  - Default literal, using declarations, static constructors, optional/named args, extension vs instance
  - Equality, interning, reference equality, NaN, value semantics, pattern matching
- Overloading, Overriding, Hiding
- Delegates, Events, Closures
- Generics: Constraints, Variance, Reification
- Nullable Reference Types and Nullability Operators
- Exception Handling, Filters, and Stack Preservation
- Async/Await: Deadlocks, ConfigureAwait, ValueTask, async void, IAsyncEnumerable
- Concurrency & Synchronization Primitives
- LINQ: Deferred Execution, Query Pitfalls, IEnumerable vs IQueryable
- yield return and Iterator Blocks
- Entity Framework Core: Tracking, Lazy Loading, Context Lifetime, Translation
- Unit Testing Pitfalls
- Struct vs Class, Boxing/Unboxing, ref/in/out
- Span/Memory and Performance Types
- Reflection, Expression Trees, Dynamic Invocation
- Compiler/JIT Optimizations
- Interop and unsafe
- Attributes and Metadata
- Design Principles (SOLID)
- Design Patterns (GoF + DDD/CQRS/DI/Repository)

---

## Core C# Language Behavior

<details>
<summary>Q1. checked vs unchecked overflow behavior — what prints?</summary>

```csharp
using System;

class Program
{
    static void Main()
    {
        int a = int.MaxValue;
        int b = a + 1;
        Console.WriteLine(b);

        checked
        {
            try
            {
                int c = int.MaxValue + 1;
                Console.WriteLine(c);
            }
            catch (OverflowException e)
            {
                Console.WriteLine(e.GetType().Name);
            }
        }

        unchecked
        {
            int d = int.MaxValue + 1;
            Console.WriteLine(d);
        }
    }
}
```

Answer:
- The first addition `a + 1` occurs in the default context for your project settings. By default, integer arithmetic is unchecked in release and debug unless the compiler/project enables `checked` for integers. Typically:
  - First line prints `-2147483648` (wrap-around).
- Inside `checked`, `int.MaxValue + 1` throws `OverflowException`, so it prints `OverflowException`.
- Inside `unchecked`, it wraps again and prints `-2147483648`.

Explanation:
- C# integral arithmetic overflows do not throw by default. `checked` enforces overflow checks; `unchecked` disables them.
- The behavior for `const` expressions can be checked at compile time depending on context.

Takeaway:
- For financial/security-sensitive arithmetic, use `checked` (globally or locally) or types like `decimal`. Know your project’s overflow settings.
</details>

<details>
<summary>Q2. Default literal and nullable value types — what’s the type and value?</summary>

```csharp
int x = default;
int? y = default;
var z = default; // What is z?

Console.WriteLine($"{x}, {y}, {z?.GetType()?.Name ?? "null"}");
```

Answer:
- `x = default;` sets `x` to `0`.
- `y = default;` sets `y` to `null` (default of `Nullable<int>` is null).
- `var z = default;` is not allowed because the compiler cannot infer a type from the default literal alone. This line does not compile.

Explanation:
- `default` literal requires a target-typed context. For `var`, there is none.
- You can write `var z = default(object);` which gives `z == null`.

Takeaway:
- `default` literal needs a target type. Use `default(T)` or a cast if type inference fails.
</details>

<details>
<summary>Q3. Using declarations and disposal order — which Dispose runs first?</summary>

```csharp
using System;
class A : IDisposable { public void Dispose() => Console.WriteLine("A.Dispose"); }
class B : IDisposable { public void Dispose() => Console.WriteLine("B.Dispose"); }

class Program
{
    static void Main()
    {
        using var a = new A();
        using var b = new B();
        Console.WriteLine("Body");
    }
}
```

Answer:
- Output:
  - Body
  - B.Dispose
  - A.Dispose

Explanation:
- Using declarations dispose at the end of the scope in reverse declaration order.

Takeaway:
- Using declarations clean up in LIFO order at the end of the scope, similar to nested using statements.
</details>

<details>
<summary>Q4. Static constructor timing — which line executes first?</summary>

```csharp
using System;

class T
{
    static T() { Console.WriteLine("Type Initialized"); }
    public static int Value = 42;
}

class Program
{
    static void Main()
    {
        Console.WriteLine("Before");
        Console.WriteLine(T.Value);
        Console.WriteLine("After");
    }
}
```

Answer:
- Output:
  - Before
  - Type Initialized
  - 42
  - After

Explanation:
- A type initializer runs before any static member is accessed (or an instance is created), once per AppDomain/process.

Takeaway:
- Static constructors run once before first use and are thread-safe. Avoid heavy or blocking work there.
</details>

<details>
<summary>Q5. Optional parameters vs overloads — which method is chosen?</summary>

```csharp
using System;

class C
{
    public void M(int x = 1) => Console.WriteLine($"M(int={x})");
    public void M(object x) => Console.WriteLine("M(object)");
}

class Program
{
    static void Main()
    {
        var c = new C();
        c.M();      // ?
        c.M(null);  // ?
        c.M(5);     // ?
    }
}
```

Answer:
- `c.M();` calls `M(int=1)` because the optional parameter provides a default.
- `c.M(null);` chooses `M(object)` since `null` can’t convert to `int` but is valid for `object`.
- `c.M(5);` prefers `M(int=5)` as the better match over `object`.

Explanation:
- Overload resolution considers exact matches first, then conversions. Optional parameters are compile-time.

Takeaway:
- Be cautious mixing optional parameters and overloads. Ambiguities can be surprising and are compile-time decisions.
</details>

<details>
<summary>Q6. Extension vs instance method — which is called?</summary>

```csharp
using System;

static class Ext
{
    public static void Foo(this string s) => Console.WriteLine("Ext.Foo");
}
class D
{
    public void Foo() => Console.WriteLine("D.Foo");
}

class Program
{
    static void Main()
    {
        var s = "hi";
        // s.Foo(); // which one?
        var d = new D();
        d.Foo();
    }
}
```

Answer:
- The line `s.Foo();` would call `Ext.Foo` because `string` has no instance `Foo`.
- For `d.Foo()`, instance methods always win over extension methods. If there were an extension `Foo(this D)`, `D.Foo` would be chosen.

Explanation:
- Extension methods are only considered when no instance member matches.

Takeaway:
- Instance members take precedence over extensions. Avoid name collisions that change call sites unintentionally when adding extensions.
</details>

<details>
<summary>Q7. Equality vs ReferenceEquals — will this print true?</summary>

```csharp
string a = "abc";
string b = "ab" + "c";
Console.WriteLine(object.ReferenceEquals(a, b));
```

Answer:
- Prints `True`.

Explanation:
- The compiler folds `"ab" + "c"` to `"abc"` at compile time. Both reference the interned literal.

Takeaway:
- String literals are interned; constant-folded results are interned. Runtime concatenation may not be interned unless `String.Intern` is used.
</details>

<details>
<summary>Q8. NaN equality — what prints?</summary>

```csharp
double x = double.NaN;
Console.WriteLine(x == x);
Console.WriteLine(object.Equals(x, x));
Console.WriteLine(double.IsNaN(x));
```

Answer:
- `x == x` prints `False`.
- `object.Equals(x, x)` prints `True`? No — it also prints `True`? Actually: `double.Equals(double.NaN, double.NaN)` returns `True`. But be careful:
  - `object.Equals(double.NaN, double.NaN)` boxes to double and calls `Double.Equals`, which returns `True`.
- `double.IsNaN(x)` prints `True`.

Explanation:
- IEEE-754 equality operator defines NaN != NaN, but `Double.Equals` is special-cased to return true for NaN.

Takeaway:
- Use `double.IsNaN` to check NaN. Equality operators and Equals behave differently here.
</details>

<details>
<summary>Q9. Pattern matching null behavior — what matches?</summary>

```csharp
object o = null;
Console.WriteLine(o is string);              // ?
Console.WriteLine(o is string s);            // ?
Console.WriteLine(o is null);                // ?
Console.WriteLine(o is not null);            // ?
```

Answer:
- `o is string` => `False`.
- `o is string s` => `False` (and no variable assigned).
- `o is null` => `True`.
- `o is not null` => `False`.

Explanation:
- Type patterns do not match null. `null` can be pattern-matched explicitly.

Takeaway:
- Prefer `is null`/`is not null` for clarity in null checks with pattern matching.
</details>

<details>
<summary>Q10. Deconstruction and discards — what’s the result?</summary>

```csharp
(int x, int y) = (1, 2);
var (_, second) = (10, 20);
Console.WriteLine($"{x},{y}; {second}");
```

Answer:
- Prints: `1,2; 20`.

Explanation:
- Discards `_` drop values. Deconstruction by tuple shape.

Takeaway:
- Use discards to denote ignored values, improving readability and avoiding unused variable warnings.
</details>

---

## Overloading, Overriding, Hiding

<details>
<summary>Q11. new vs override — which method is called?</summary>

```csharp
using System;

class Base { public virtual void M() => Console.WriteLine("Base.M"); }
class Derived : Base { public new void M() => Console.WriteLine("Derived.M (new)"); }

class Program
{
    static void Main()
    {
        Base b = new Derived();
        Derived d = new Derived();
        b.M(); // ?
        d.M(); // ?
    }
}
```

Answer:
- `b.M()` calls `Base.M` because `Derived` hides, not overrides.
- `d.M()` calls `Derived.M (new)`.

Explanation:
- `new` hides at compile-time; polymorphism requires `override`.

Takeaway:
- Prefer `override` to extend polymorphic behavior. Use `new` rarely and intentionally; it can be confusing.
</details>

<details>
<summary>Q12. Overload resolution with nullables — which overload wins?</summary>

```csharp
using System;

class C
{
    public void M(int? x) => Console.WriteLine("nullable int");
    public void M(object x) => Console.WriteLine("object");
}

class Program
{
    static void Main()
    {
        C c = new C();
        c.M(null); // ?
    }
}
```

Answer:
- Picks `M(int?)`.

Explanation:
- Overload resolution prefers the more specific type. `null` is convertible to both `object` and `int?`, but `int?` is “better” here.

Takeaway:
- Overloading with nullable and object can be subtle. Avoid ambiguity by clear names or disambiguating APIs.
</details>

<details>
<summary>Q13. Extension method vs instance override — which is chosen?</summary>

```csharp
using System;

class B { public virtual void X() => Console.WriteLine("B.X"); }
class D : B { public override void X() => Console.WriteLine("D.X"); }

static class Ext
{
    public static void X(this B b) => Console.WriteLine("Ext.B.X");
    public static void X(this D d) => Console.WriteLine("Ext.D.X");
}

class Program
{
    static void Main()
    {
        B b = new D();
        b.X();         // ?
        Ext.X((D)b);   // ?
    }
}
```

Answer:
- `b.X()` calls the instance virtual override -> `D.X`.
- `Ext.X((D)b)` resolves extension overloads at compile-time by static type and parameters; here it matches `Ext.D.X` and prints `Ext.D.X`.

Explanation:
- Extension methods are not virtual. Overload resolution occurs at compile-time based on parameter types.

Takeaway:
- Instance virtual methods win on instance calls. Extension methods require explicit static calls to pick overloads across types.
</details>

<details>
<summary>Q14. Operator overload and null — what happens?</summary>

```csharp
using System;

struct S
{
    public int V;
    public static S operator +(S a, S b) => new S { V = a.V + b.V };
}

class Program
{
    static void Main()
    {
        S? a = new S { V = 1 };
        S? b = null;
        var c = a + b; // ?
        Console.WriteLine(c.HasValue ? c.Value.V : -1);
    }
}
```

Answer:
- `a + b` yields `null`. Lifted operator semantics return null if any operand is null (unless both null-handled by custom logic, which doesn’t apply to operator overloading automatically).
- Prints `-1`.

Explanation:
- Nullable lifting of user-defined operators yields nullable result; null presence propagates.

Takeaway:
- Nullable lifting applies to user-defined operators. Know how nulls propagate in operator overloading.
</details>

---

## Delegates, Events, Closures

<details>
<summary>Q15. Closure over loop variable (for) — what prints?</summary>

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        var actions = new List<Action>();
        for (int i = 0; i < 3; i++)
        {
            actions.Add(() => Console.Write(i));
        }
        foreach (var a in actions) a();
    }
}
```

Answer:
- Prints `333` (not `012`).

Explanation:
- The lambda captures the variable `i`, not its value. By the time the actions run, `i == 3`.

Fix:
```csharp
for (int i = 0; i < 3; i++)
{
    int copy = i;
    actions.Add(() => Console.Write(copy));
}
```

Takeaway:
- When capturing loop variables in for-loops, capture a local copy to lock in the value.
</details>

<details>
<summary>Q16. Closure over foreach variable — what prints in modern C#?</summary>

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        var actions = new List<Action>();
        foreach (var s in new[] { "a", "b", "c" })
        {
            actions.Add(() => Console.Write(s));
        }
        foreach (var a in actions) a();
    }
}
```

Answer:
- Prints `abc`.

Explanation:
- Since C# 5, foreach iteration variable is captured per iteration, unlike for-loops. Older C# had a different behavior, but this was fixed.

Takeaway:
- Foreach closures capture per-iteration variable; for-loops still require a local copy for closures.
</details>

<details>
<summary>Q17. Event unsubscription with lambda — does this work?</summary>

```csharp
using System;

class Pub
{
    public event EventHandler? E;
    public void Raise() => E?.Invoke(this, EventArgs.Empty);
}

class Program
{
    static void Main()
    {
        var p = new Pub();
        p.E += (s, e) => Console.WriteLine("Handler");
        p.Raise();
        p.E -= (s, e) => Console.WriteLine("Handler");
        p.Raise();
    }
}
```

Answer:
- Output:
  - Handler
  - Handler

Explanation:
- Unsubscribing with a different lambda instance does nothing. Delegates must match the same instance used in subscription.

Fix:
```csharp
EventHandler h = (s,e) => Console.WriteLine("Handler");
p.E += h;
p.E -= h;
```

Takeaway:
- Always keep a reference to the handler delegate if you need to unsubscribe later.
</details>

<details>
<summary>Q18. Multicast delegate return value — what’s returned?</summary>

```csharp
using System;

class Program
{
    delegate int D();
    static int A() { Console.WriteLine("A"); return 1; }
    static int B() { Console.WriteLine("B"); return 2; }

    static void Main()
    {
        D d = A;
        d += B;
        var r = d(); // ?
        Console.WriteLine($"r={r}");
    }
}
```

Answer:
- Prints:
  - A
  - B
  - r=2

Explanation:
- For multicast delegates, all handlers are invoked; the return value is from the last handler.

Takeaway:
- Don’t rely on return values of multicast delegates. Use event arguments or gather results explicitly.
</details>

---

## Generics: Constraints, Variance, Reification

<details>
<summary>Q19. IList&lt;object&gt; from List&lt;string&gt; — does this compile?</summary>

```csharp
List<string> list = new();
IList<object> olist = list; // ?
```

Answer:
- Does not compile. `IList<T>` is invariant.

Explanation:
- Variance applies to interfaces/delegates with out/in annotations. `IEnumerable<out T>` is covariant; `IList<T>` is not.

Takeaway:
- Use `IEnumerable<object>` or `IReadOnlyList<object>` for covariance where applicable; avoid writing to covariant collections.
</details>

<details>
<summary>Q20. Covariant and contravariant interfaces — which converts?</summary>

```csharp
IEnumerable<string> a = new List<string>();
IEnumerable<object> b = a; // ?
Action<object> x = o => { };
Action<string> y = x; // ?
```

Answer:
- `IEnumerable<string>` to `IEnumerable<object>` is allowed: covariance (`out T`).
- `Action<object>` to `Action<string>` is allowed: contravariance (`in T`), because an `Action<object>` can accept a `string`.

Takeaway:
- Covariance: you can use a more derived type where a base is expected for outputs.
- Contravariance: you can use a less derived type where a more derived is expected for inputs.
</details>

<details>
<summary>Q21. Constraints: struct vs unmanaged vs notnull — what compiles?</summary>

```csharp
// Which constraints allow pointers?
// 1) where T : struct
// 2) where T : unmanaged
// 3) where T : notnull
```

Answer:
- Only `where T : unmanaged` guarantees no references, allowing pointers and `sizeof(T)` in safe contexts with `unsafe` operations.
- `struct` allows managed fields (e.g., `string`), so not safe for pointers.
- `notnull` is a reference-type nullability constraint, not related to pointers.

Takeaway:
- Use `unmanaged` when you need blittable types for interop and low-level operations.
</details>

<details>
<summary>Q22. Reified generics — can you get typeof(T) at runtime?</summary>

```csharp
static Type GetT<T>() => typeof(T);

Console.WriteLine(GetT<int>());
Console.WriteLine(GetT<string>());
```

Answer:
- Yes. .NET generics are reified. Output includes `System.Int32`, `System.String`.

Takeaway:
- Unlike Java type erasure, .NET keeps generic type parameters at runtime. Reflection can inspect T.
</details>

<details>
<summary>Q23. default(T) under constraints — what’s default?</summary>

```csharp
static T Make<T>() => default;
static T MakeValue<T>() where T : struct => default;
static T MakeRef<T>() where T : class => default;

Console.WriteLine(Make<int>());     // ?
Console.WriteLine(MakeValue<int>());// ?
Console.WriteLine(MakeRef<string>() == null); // ?
```

Answer:
- `Make<int>()` => `0`.
- `MakeValue<int>()` => `0`.
- `MakeRef<string>()` => `null` (prints `True`).

Takeaway:
- default(T) is 0-initialized for value types, null for reference types, regardless of constraints; constraints just limit T at compile-time.
</details>

---

## Nullable Reference Types and Nullability Operators

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
- Use `!` sparingly and only when you have logically proven non-null, not to silence warnings prematurely.
</details>

<details>
<summary>Q25. Null-coalescing assignment (??=) — when is it invoked?</summary>

```csharp
Dictionary<string, string>? cache = null;
(cache ??= new()).Add("k", "v"); // ?

Console.WriteLine(cache.Count);
```

Answer:
- If `cache` is null, it’s assigned to `new Dictionary<...>()` and then `.Add` is called. Prints `1`.

Takeaway:
- `??=` is convenient for lazy initialization of references.
</details>

<details>
<summary>Q26. Null-conditional operator chaining — what’s the return?</summary>

```csharp
class A { public B? B { get; set; } }
class B { public string? S { get; set; } }
A? a = null;

Console.WriteLine(a?.B?.S?.ToUpper() ?? "none");
```

Answer:
- Safe navigation returns `null` at first step; `??` coalesces to `"none"`. Prints `none`.

Takeaway:
- Chain `?.` and finish with `??` to provide defaults. Avoids NRE without verbose checks.
</details>

---

## Exception Handling, Filters, Stack Preservation

<details>
<summary>Q27. Rethrow vs throw ex — how does the stack trace differ?</summary>

```csharp
try
{
    Thrower();
}
catch (Exception ex)
{
    // throw ex;
    throw;
}

static void Thrower() => throw new InvalidOperationException("boom");
```

Answer:
- `throw;` preserves the original stack.
- `throw ex;` resets the stack to the throw site, losing original call site information.

Takeaway:
- Always `throw;` when rethrowing inside catch; only use `throw ex;` if intentionally changing the exception context.
</details>

<details>
<summary>Q28. Exception filters — are they executed before catch blocks?</summary>

```csharp
try
{
    throw new Exception("x");
}
catch (Exception e) when (Console.WriteLine("filter"), e.Message == "x")
{
    Console.WriteLine("caught");
}
```

Answer:
- Prints:
  - filter
  - caught

Explanation:
- Filters run before a catch block is entered, and are non-interfering (no state changes to exception). Multiple filters can run on different handlers up the stack.

Takeaway:
- Exception filters are great for conditional handling and logging; they don’t affect stack traces.
</details>

<details>
<summary>Q29. AggregateException from Task.WhenAll — how many errors?</summary>

```csharp
var t = Task.WhenAll(Task.Run(() => throw new Exception("A")),
                     Task.Run(() => throw new Exception("B")));
try
{
    await t;
}
catch (Exception e)
{
    Console.WriteLine(e.GetType().Name);
    foreach (var inner in ((AggregateException)e).InnerExceptions)
        Console.WriteLine(inner.Message);
}
```

Answer:
- Catches `AggregateException` with two InnerExceptions: A and B.

Takeaway:
- `Task.WhenAll` aggregates all failures; be prepared to inspect multiple inner exceptions.
</details>

---

## Async/Await: Deadlocks, ConfigureAwait, ValueTask, async void, IAsyncEnumerable

<details>
<summary>Q30. Deadlock with .Result on a UI/ASP.NET context — what happens?</summary>

```csharp
async Task<int> GetAsync()
{
    await Task.Delay(100); // captures context by default
    return 42;
}

// On a context-bound thread (WinForms/WPF old sync context):
var result = GetAsync().Result; // ?
```

Answer:
- Potential deadlock: the continuation tries to post back to the captured SynchronizationContext, which is blocked waiting for `.Result`.

Fix:
- Use `await` all the way, or `ConfigureAwait(false)` inside library code:
```csharp
await Task.Delay(100).ConfigureAwait(false);
```

Takeaway:
- Never block on async in context-bound threads. Prefer async all the way; library code should use ConfigureAwait(false).
</details>

<details>
<summary>Q31. ConfigureAwait(false) — when and why?</summary>

```csharp
await SomeIOAsync().ConfigureAwait(false);
UpdateUI(); // ?
```

Answer:
- After `ConfigureAwait(false)`, continuation resumes on a thread pool thread, not the original context. Calling `UpdateUI()` (UI-bound) here will throw or be unsafe.

Takeaway:
- Use `ConfigureAwait(false)` in library code that doesn’t need a context. Do not use it where you need to update context-bound components (UI, ASP.NET request context pre-Core).
</details>

<details>
<summary>Q32. async void — where do exceptions go?</summary>

```csharp
async void FireAndForget()
{
    await Task.Delay(10);
    throw new Exception("boom");
}

try
{
    FireAndForget();
}
catch
{
    Console.WriteLine("caught?");
}
```

Answer:
- The catch won’t capture the exception. `async void` exceptions go to the synchronization context and can crash the process.

Takeaway:
- Avoid `async void` except for event handlers. Prefer `async Task` methods so callers can await and handle exceptions.
</details>

<details>
<summary>Q33. ValueTask pitfalls — can you await twice?</summary>

```csharp
async ValueTask<int> V() => 1;
var vt = V();
var a = await vt;
// var b = await vt; // ?
```

Answer:
- You must not await a non-Task-backed ValueTask twice. Doing so is undefined and can throw. If you need multiple awaits, materialize: `await vt.AsTask()`.

Takeaway:
- Use ValueTask for performance-sensitive paths where allocation matters and success is likely synchronous. Otherwise, prefer Task for simplicity.
</details>

<details>
<summary>Q34. Task.Run vs Task.Factory.StartNew — difference in default scheduler?</summary>

```csharp
Task t1 = Task.Run(() => { /* work */ });
Task t2 = Task.Factory.StartNew(() => { /* work */ }); // no TaskScheduler.Default?
```

Answer:
- `Task.Run` always queues to TaskScheduler.Default (thread pool).
- `StartNew` uses current TaskScheduler, which might be a custom scheduler if called inside a task. Also, `StartNew` by default does not use TaskCreationOptions.LongRunning or TaskScheduler.Default; it’s much easier to misuse.

Takeaway:
- Prefer `Task.Run` for simple background work; `StartNew` is advanced and often misused. Use `TaskCreationOptions` explicitly if needed.
</details>

<details>
<summary>Q35. await foreach cancellation — how to pass token?</summary>

```csharp
await foreach (var item in GetStreamAsync())
{
    // ...
}

static async IAsyncEnumerable<int> GetStreamAsync() // token?
{
    yield return 1;
}
```

Answer:
- To pass cancellation:
```csharp
await foreach (var item in GetStreamAsync(ct))
{
}

static async IAsyncEnumerable<int> GetStreamAsync([EnumeratorCancellation] CancellationToken ct)
{
    yield return 1;
}
```

Explanation:
- Use `[EnumeratorCancellation]` on a token parameter so the compiler wires it to the async enumerator’s cancellation mechanism.

Takeaway:
- For IAsyncEnumerable, pass cancellation via a parameter annotated with `[EnumeratorCancellation]` and the caller passes it via `await foreach (..., ct)`.
</details>

---

## Concurrency & Synchronization Primitives

<details>
<summary>Q36. lock sugar and exceptions — can you throw inside lock?</summary>

```csharp
object gate = new();
lock (gate)
{
    Console.WriteLine("Inside");
    throw new Exception("boom");
}
```

Answer:
- Yes. The compiler emits try/finally with `Monitor.Enter/Exit`. The lock is always released even when exceptions occur.

Takeaway:
- `lock` is exception-safe; don’t swallow exceptions to avoid deadlocks — the lock will exit.
</details>

<details>
<summary>Q37. lock(this) — why is it risky?</summary>

```csharp
class C
{
    public void Safe()
    {
        lock (this) { /* ... */ } // ?
    }
}
```

Answer:
- Dangerous: external code can also lock on your publicly accessible instance and cause deadlocks.

Takeaway:
- Lock on a private readonly object dedicated for locking, not `this` or string literals or publicly reachable objects.
</details>

<details>
<summary>Q38. SemaphoreSlim WaitAsync with ConfigureAwait — where’s the deadlock?</summary>

```csharp
var sem = new SemaphoreSlim(1,1);
await sem.WaitAsync().ConfigureAwait(false);
try
{
    // work
}
finally
{
    sem.Release();
}
```

Answer:
- Safe. The pitfall is forgetting to release in exception paths or mixing `Wait()` (sync) and `Release()` across contexts. Use try/finally always. Also, be careful not to await inside a region without handling reentrancy if order matters.

Takeaway:
- Always pair `WaitAsync` with `Release` in a finally. Avoid mixing sync Wait/Result with async.
</details>

<details>
<summary>Q39. ConcurrentDictionary GetOrAdd value factory — called multiple times?</summary>

```csharp
var cd = new ConcurrentDictionary<int, string>();
int calls = 0;
string r = cd.GetOrAdd(1, _ => { calls++; return "x"; });
string r2 = cd.GetOrAdd(1, _ => { calls++; return "y"; });
Console.WriteLine(calls);
```

Answer:
- `calls` can be 1 or 2 depending on race timing, but the stored value will be the first one published for key=1. After the first successful add, later factories may still be invoked but their result discarded.

Takeaway:
- The value factory may be invoked multiple times; minimize side effects inside factory and rely on idempotency.
</details>

<details>
<summary>Q40. Interlocked memory semantics — does Interlocked.Exchange act as a fence?</summary>

```csharp
int x = 0;
Interlocked.Exchange(ref x, 1);
// Are writes before visible after?
```

Answer:
- Interlocked operations are full memory fences; they ensure ordering before/after relative to other threads.

Takeaway:
- Use Interlocked for atomic ops and memory barriers without taking locks; still keep code simple to avoid subtle bugs.
</details>

---

## LINQ: Deferred Execution, IEnumerable vs IQueryable

<details>
<summary>Q41. Deferred execution — when does exception throw?</summary>

```csharp
IEnumerable<int> Seq()
{
    Console.WriteLine("Creating");
    yield return 1;
    throw new Exception("boom");
}

var q = Seq().Select(x => x * 2);
Console.WriteLine("Before foreach");
foreach (var v in q)
{
    Console.WriteLine(v); // where does it throw?
}
```

Answer:
- Output:
  - Creating
  - Before foreach
  - 2
  - Then throws when moving past first item.

Explanation:
- Iterator blocks defer execution until enumeration; exceptions inside the iterator throw during enumeration, not at query creation.

Takeaway:
- LINQ over IEnumerable is deferred. Be prepared for exceptions during enumeration, not definition.
</details>

<details>
<summary>Q42. Multiple enumeration — why is this a problem?</summary>

```csharp
IEnumerable<int> q = Enumerable.Range(1, 3).Select(x => {
    Console.WriteLine($"Map {x}");
    return x * 2;
});

var list = q.ToList();
var sum = q.Sum();
```

Answer:
- Outputs mapping twice; `q` is re-enumerated when calling `.ToList()` and `.Sum()`. Might cause performance or side effects problems.

Takeaway:
- Materialize once (ToList/ToArray) when needed multiple times, or use memoization. Avoid multiple enumeration if the sequence is expensive or stateful.
</details>

<details>
<summary>Q43. IEnumerable vs IQueryable — where does filtering happen?</summary>

```csharp
IQueryable<User> users = db.Users; // EF Core
IEnumerable<User> filtered = users.Where(u => Heavy(u)); // ?
```

Answer:
- Because `Where` here is the IQueryable extension (if `Heavy` is an expression), but since `Heavy(u)` is a normal method that cannot translate to SQL, EF Core (3.0+) will throw at runtime (cannot translate) or may attempt client evaluation in older EF versions (pre-Core 3). If you call `.AsEnumerable()` before Where, filtering happens client-side.

Takeaway:
- Keep expression trees translatable. Use `.AsEnumerable()` to switch to in-memory processing explicitly; otherwise, EF Core must translate or throw.
</details>

<details>
<summary>Q44. OrderBy stability — is LINQ OrderBy stable?</summary>

```csharp
var data = new[] {
    new { K=1, V="a" },
    new { K=1, V="b" },
    new { K=2, V="c" },
};

var ordered = data.OrderBy(x => x.K).ThenBy(x => x.V);
```

Answer:
- C# LINQ OrderBy is stable; items with equal keys preserve relative order before ThenBy. In the snippet, ThenBy changes order for equal keys based on V anyway.

Takeaway:
- OrderBy/ThenBy are stable in LINQ to Objects. Don’t assume stability in databases unless documented by the provider.
</details>

<details>
<summary>Q45. Distinct with custom equality — what’s used?</summary>

```csharp
var s = new[] { "A", "a", "A" };
var d = s.Distinct(StringComparer.OrdinalIgnoreCase);
Console.WriteLine(string.Join(",", d));
```

Answer:
- Prints: `A` (once). Uses provided comparer to identify distinct elements.

Takeaway:
- Distinct uses equality comparer semantics; provide the right comparer to avoid surprises.
</details>

---

## yield return and Iterator Blocks

<details>
<summary>Q46. finally in iterators — when does it run?</summary>

```csharp
IEnumerable<int> Seq()
{
    try
    {
        yield return 1;
        yield return 2;
    }
    finally
    {
        Console.WriteLine("Finally");
    }
}

using var e = Seq().GetEnumerator();
Console.WriteLine(e.MoveNext());
Console.WriteLine(e.Current);
e.Dispose(); // ?
```

Answer:
- Prints:
  - True
  - 1
  - Finally

Explanation:
- Disposing the enumerator triggers the iterator’s finally block, even if enumeration stops early.

Takeaway:
- Iterator `finally` ensures cleanup on disposal. Enumerate with `foreach` or dispose enumerators to run cleanup.
</details>

<details>
<summary>Q47. yield and exception timing — when is exception thrown?</summary>

```csharp
IEnumerable<int> Seq()
{
    yield return 1;
    throw new Exception("boom");
}

foreach (var v in Seq())
{
    Console.WriteLine(v); // when do we see exception?
}
```

Answer:
- Prints `1`, then throws on the next MoveNext after consuming the first item.

Takeaway:
- Exceptions in iterators surface during MoveNext at enumeration time, not at GetEnumerator or first yield necessarily.
</details>

---

## Entity Framework Core

<details>
<summary>Q48. Tracking vs AsNoTracking — which is faster?</summary>

```csharp
var tracked = await db.Users.Where(u => u.Active).ToListAsync();
var untracked = await db.Users.AsNoTracking().Where(u => u.Active).ToListAsync();
```

Answer:
- `AsNoTracking` is typically faster and uses less memory when you don’t plan to modify entities, because change tracker overhead is skipped.

Takeaway:
- Use `AsNoTracking` for read-only queries. For updates, track entities or use `Attach/Update` patterns.
</details>

<details>
<summary>Q49. Lazy loading proxies — why can serialization explode?</summary>

```csharp
var user = await db.Users.FindAsync(id); // lazy-loaded navs
var json = JsonSerializer.Serialize(user); // ?
```

Answer:
- Lazy loading proxies can cause circular reference graphs and unexpected database queries during serialization, leading to performance or stack issues.

Takeaway:
- Avoid lazy loading in serialization boundaries; prefer eager loading (`Include`) or DTOs.
</details>

<details>
<summary>Q50. Context lifetime and thread safety — can DbContext be shared?</summary>

```csharp
// Single DbContext used across threads?
```

Answer:
- DbContext is not thread-safe. Use a short-lived context per unit-of-work/request. Sharing across threads risks race conditions and exceptions.

Takeaway:
- Scope DbContext appropriately (e.g., per request in web apps).
</details>

<details>
<summary>Q51. N+1 problem and AsSplitQuery — when to use?</summary>

```csharp
var q = db.Parents.Include(p => p.Children).Include(p => p.Tags);
// EF Core may produce a single JOIN with cartesian explosion
var list = await q.AsSplitQuery().ToListAsync(); // ?
```

Answer:
- `AsSplitQuery` executes separate queries per include to avoid cartesian explosion, often improving performance at the cost of extra round-trips.

Takeaway:
- Use `AsSplitQuery` for complex graphs to avoid duplicate row blowups; measure the trade-offs.
</details>

<details>
<summary>Q52. Client evaluation removal — why does query throw?</summary>

```csharp
var q = db.Users.Where(u => SomeCSharpOnlyMethod(u.Name)); // ?
```

Answer:
- EF Core 3+ forbids client evaluation in Where and other key clauses; throws a runtime exception (cannot translate). Move logic client-side (AsEnumerable) or translate to EF function.

Takeaway:
- Ensure predicates are translatable or switch to client evaluation explicitly.
</details>

<details>
<summary>Q53. Concurrency tokens — how to handle DbUpdateConcurrencyException?</summary>

```csharp
try
{
    await db.SaveChangesAsync();
}
catch (DbUpdateConcurrencyException ex)
{
    // Resolve
}
```

Answer:
- Concurrency token mismatch throws. You may reload the entity, merge changes, retry, or abort — typical optimistic concurrency resolution.

Takeaway:
- Design for retries and conflict resolution when using concurrency tokens (RowVersion etc.).
</details>

<details>
<summary>Q54. FromSqlInterpolated vs FromSqlRaw — SQL injection risk?</summary>

```csharp
var name = input;
var q = db.Users.FromSqlRaw($"SELECT * FROM Users WHERE Name = '{name}'"); // ?
```

Answer:
- Using string interpolation with FromSqlRaw is unsafe and can inject SQL. Use `FromSqlInterpolated` or parameters.

Takeaway:
- Always parameterize SQL with FromSqlInterpolated or parameter objects.
</details>

---

## Unit Testing Pitfalls

<details>
<summary>Q55. async void test — why is it bad?</summary>

```csharp
[Test]
public async void Test() // ?
{
    await Task.Delay(10);
    Assert.Pass();
}
```

Answer:
- The framework can’t await `async void`; test may finish before completion or miss exceptions.

Fix:
```csharp
[Test]
public async Task Test()
{
    await Task.Delay(10);
    Assert.Pass();
}
```

Takeaway:
- Test methods should return Task for async.
</details>

<details>
<summary>Q56. Mocking EF Core async queries — what’s required?</summary>

```csharp
// IQueryable provider must support async
// Using ToListAsync on an in-memory mock IQueryable without async provider?
```

Answer:
- `ToListAsync` relies on IAsyncQueryProvider. Use EF Core’s InMemory provider or test double that implements async query provider (e.g., tools like MockQueryable).

Takeaway:
- Don’t mock EF Core DbSet with plain lists and expect async to work; use real provider or proper async-capable mocks.
</details>

<details>
<summary>Q57. Floating point assertions — why do tests flake?</summary>

```csharp
Assert.AreEqual(0.3, 0.1 + 0.2); // ?
```

Answer:
- Can fail due to binary floating point rounding. Use tolerances:
```csharp
Assert.That(0.1 + 0.2, Is.EqualTo(0.3).Within(1e-12));
```

Takeaway:
- For double/float, assert with tolerances or use decimal if domain allows exact arithmetic.
</details>

<details>
<summary>Q58. Time-dependent tests — how to avoid DateTime.Now?</summary>

```csharp
var now = DateTime.Now; // test fails on boundary?
```

Answer:
- Inject an IClock or use `TimeProvider` (.NET 8) to control time; freeze or fake in tests.

Takeaway:
- Abstract time for deterministic tests.
</details>

---

## Struct vs Class, Boxing/Unboxing, in/out/ref

<details>
<summary>Q59. Boxing via interface — does this allocate?</summary>

```csharp
struct S { public int X; }
IComparable c = 5; // boxing
```

Answer:
- Yes, boxing occurs when converting value types to reference-typed interfaces or object.

Takeaway:
- Avoid boxing in hot paths; use generics with constraints or specialized overloads to prevent boxing.
</details>

<details>
<summary>Q60. Mutating a struct property — why doesn’t it stick?</summary>

```csharp
struct Point { public int X; }
class Holder { public Point P { get; set; } }

var h = new Holder { P = new Point { X = 1 } };
h.P.X = 2; // ?
Console.WriteLine(h.P.X);
```

Answer:
- For auto-property returns by value, `h.P` returns a copy. `h.P.X = 2` modifies the temporary copy and won’t compile unless C# allows set on return. In general modifying struct properties requires reading, modifying, and assigning back:
```csharp
var p = h.P; p.X = 2; h.P = p;
```

Takeaway:
- Structs copy by value; be careful with mutability through properties. Use `readonly struct` or mutable reference types if needed.
</details>

<details>
<summary>Q61. readonly struct and defensive copies — when do they occur?</summary>

```csharp
readonly struct RS
{
    public int X { get; }
    public int Inc() => X + 1;
}

RS r = new RS();
var v = r.Inc(); // is there a copy?
```

Answer:
- When calling instance methods on a non-readonly variable of a readonly struct via certain contexts (e.g., through an interface or property), the compiler may copy to avoid mutation. Marking members `readonly` and variables `in` reduces copies.

Takeaway:
- Use `readonly` members and careful APIs to avoid hidden copies with readonly structs.
</details>

<details>
<summary>Q62. ref, out, in — what’s allowed?</summary>

```csharp
void M(in int x, ref int y, out int z) { z = x + y; y++; }
int a = 1, b = 2, c;
M(in a, ref b, out c);
Console.WriteLine($"{a},{b},{c}");
```

Answer:
- Prints `1,3,3`.
- `in` is readonly by reference; `ref` allows reading/writing; `out` must be assigned inside.

Takeaway:
- Prefer `in` for large readonly structs to avoid copies; use ref/out only when necessary and clear.
</details>

---

## Span/Memory and Performance Types

<details>
<summary>Q63. Span cannot escape — why won’t this compile?</summary>

```csharp
Span<int> Make() // ?
{
    Span<int> s = stackalloc int[10];
    return s; // error
}
```

Answer:
- `Span<T>` is a ref struct and cannot escape the method stack frame or be boxed/heap-allocated.

Takeaway:
- Ref-like types (`Span<T>`, `ReadOnlySpan<T>`) are stack-only. Return arrays or use Memory<T> for heap-based slices.
</details>

<details>
<summary>Q64. Capturing Span in lambda — allowed?</summary>

```csharp
Span<int> s = stackalloc int[5];
Action a = () => Console.WriteLine(s[0]); // ?
```

Answer:
- Not allowed: ref struct cannot be captured by lambdas, async methods, or iterators.

Takeaway:
- Don’t capture ref structs. If needed, copy data to safe memory before capturing.
</details>

<details>
<summary>Q65. String creation with spans — why is this fast?</summary>

```csharp
string s = string.Create(5, 0, (span, state) => span.Fill('x'));
Console.WriteLine(s);
```

Answer:
- Prints `xxxxx`. `string.Create` allocates once and writes directly into the string’s buffer via a span; reduces intermediate allocations.

Takeaway:
- Use `string.Create` and spans for high-performance string building patterns.
</details>

---

## Reflection, Expression Trees, Dynamic Invocation

<details>
<summary>Q66. DynamicInvoke vs CreateDelegate — performance difference?</summary>

```csharp
var mi = typeof(string).GetMethod(nameof(string.StartsWith), new[] { typeof(string) })!;
var del = (Func<string,string,bool>)mi.CreateDelegate(typeof(Func<string,string,bool>));
bool a = del("abc", "a");

var o = (bool)mi.Invoke("abc", new object[] { "a" }); // slower?
```

Answer:
- `CreateDelegate` with strongly-typed delegate is much faster than reflection Invoke/DynamicInvoke.

Takeaway:
- Cache strongly-typed delegates for repeated invocation; avoid reflection calls in hot paths.
</details>

<details>
<summary>Q67. Expression.Compile caching — why cache?</summary>

```csharp
var p = Expression.Parameter(typeof(int));
var expr = Expression.Lambda<Func<int,int>>(Expression.Add(p, Expression.Constant(1)), p);
var f = expr.Compile(); // expensive?
```

Answer:
- Compilation is relatively expensive; cache compiled delegates when reused.

Takeaway:
- Expression.Compile is not free. Reuse compiled delegates for performance.
</details>

<details>
<summary>Q68. BindingFlags for private members — what’s needed?</summary>

```csharp
var fi = typeof(C).GetField("_x"); // null?
```

Answer:
- Need `BindingFlags.NonPublic | BindingFlags.Instance` (or Static as appropriate).

Takeaway:
- Always specify binding flags when accessing non-public members.
</details>

---

## Compiler/JIT Optimizations

<details>
<summary>Q69. Bounds check elimination — when does it happen?</summary>

```csharp
int Sum(int[] a)
{
    int s = 0;
    for (int i = 0; i < a.Length; i++)
        s += a[i];
    return s;
}
```

Answer:
- JIT can hoist bounds checks and eliminate redundant ones in simple loops like this, improving performance.

Takeaway:
- Write simple loops for the JIT to optimize well. Complex index expressions can inhibit optimizations.
</details>

<details>
<summary>Q70. AggressiveInlining attribute — should you use it?</summary>

```csharp
[MethodImpl(MethodImplOptions.AggressiveInlining)]
static int Add(int x, int y) => x + y;
```

Answer:
- It’s a hint; the JIT may still ignore it. Overuse can hurt performance by code bloat.

Takeaway:
- Trust the JIT; use inlining hints sparingly and measure.
</details>

<details>
<summary>Q71. volatile keyword — what does it guarantee?</summary>

```csharp
volatile int flag;
// Ensures reads/writes are not reordered around volatile operations
```

Answer:
- `volatile` prevents certain reordering and ensures the most up-to-date value is read/written across threads. It does not make compound operations atomic.

Takeaway:
- Use `volatile` for simple flags; use Interlocked or locks for compound operations.
</details>

---

## Interop and unsafe

<details>
<summary>Q72. stackalloc and Span — how to use safely?</summary>

```csharp
Span<byte> buf = stackalloc byte[128];
buf[0] = 42;
// ok within method scope
```

Answer:
- Safe within method; ref struct ensures no escape. Great for small temporary buffers.

Takeaway:
- Use stackalloc for tiny, short-lived buffers; keep sizes modest to avoid stack pressure.
</details>

<details>
<summary>Q73. fixed and string — what does fixed do?</summary>

```csharp
unsafe
{
    string s = "abc";
    fixed (char* p = s)
    {
        // p points to pinned chars; don’t mutate immutable string!
    }
}
```

Answer:
- `fixed` pins memory to prevent GC moves, giving a stable pointer. Do not modify string data; strings are immutable.

Takeaway:
- Pin sparingly to avoid GC fragmentation; use Span and safe APIs where possible.
</details>

<details>
<summary>Q74. delegate* unmanaged function pointers — when to prefer?</summary>

```csharp
unsafe
{
    delegate* unmanaged[Cdecl]<int,int,int> addPtr = &Add;
    int r = addPtr(1,2);
}
```

Answer:
- Function pointers avoid delegate allocations and can be faster for interop/hot paths. Unsafe and low-level; use when needed.

Takeaway:
- Function pointers are powerful but unsafe; measure and encapsulate carefully.
</details>

---

## Attributes and Metadata

<details>
<summary>Q75. Attribute parameters must be constants — what fails?</summary>

```csharp
const string A = "x";
string B = "y";

[Obsolete(A)] // ok
// [Obsolete(B)] // fails
```

Answer:
- Attribute arguments must be compile-time constants, typeof expressions, or arrays of such.

Takeaway:
- Use consts for attribute arguments; non-const fields/props aren’t allowed.
</details>

<details>
<summary>Q76. Conditional attribute — does method get called?</summary>

```csharp
[Conditional("TRACE")]
static void Log(string s) => Console.WriteLine(s);

static void Main()
{
    Log("x");
}
```

Answer:
- If TRACE is not defined at compile time, calls to Log are omitted entirely (not just no-op).

Takeaway:
- Conditional methods are compiled out when symbol is not defined; side effects inside won’t happen.
</details>

---

## Design Principles (SOLID)

<details>
<summary>Q77. Single Responsibility Principle — what’s wrong?</summary>

```csharp
class OrderService
{
    public void PlaceOrder(Order o)
    {
        // validate, save to DB, send email, log, render PDF invoice...
    }
}
```

Question:
- Which responsibilities are conflated and why is this bad?

Answer:
- It mixes validation, persistence, messaging, logging, document generation. Changes in any area ripple through one class, hurting maintainability and testability.

Refactor:
- Split into services: IOrderRepository, IEmailSender, IInvoiceGenerator, IValidator<Order>, etc.

Takeaway:
- Each class should have one reason to change. Decouple concerns; compose via DI.
</details>

<details>
<summary>Q78. Open/Closed Principle — which approach is better?</summary>

```csharp
decimal Discount(Order o)
{
    if (o.Type == "VIP") return o.Total * 0.9m;
    if (o.Type == "Employee") return o.Total * 0.8m;
    // ...
    return o.Total;
}
```

Answer:
- This requires modification for every new type. Better:
```csharp
interface IDiscount { bool Applies(Order o); decimal Apply(Order o); }
class VipDiscount : IDiscount { /*...*/ }
// Compose via a list of IDiscount and apply
```

Takeaway:
- Open for extension, closed for modification. Use polymorphism or rules engines rather than conditionals.
</details>

<details>
<summary>Q79. Liskov Substitution Principle — violation example?</summary>

```csharp
class Rectangle { public virtual int W { get; set; } public virtual int H { get; set; } }
class Square : Rectangle
{
    public override int W { set { base.W = value; base.H = value; } get => base.W; }
    public override int H { set { base.W = value; base.H = value; } get => base.H; }
}
```

Answer:
- Substituting Square for Rectangle breaks expectations: setting W shouldn’t affect H. Violates LSP.

Takeaway:
- Model types correctly; avoid inheritance when invariants differ. Prefer composition.
</details>

<details>
<summary>Q80. Interface Segregation Principle — what's the issue?</summary>

```csharp
interface IPrinter
{
    void Print();
    void Fax();
    void Scan();
}
```

Answer:
- Clients that only need Print are forced to implement Fax/Scan. Split into smaller interfaces: IPrint, IFax, IScan.

Takeaway:
- Prefer small, role-based interfaces.
</details>

<details>
<summary>Q81. Dependency Inversion — show inversion with abstractions</summary>

```csharp
class Service
{
    private readonly Repository _repo = new(); // bad
}
```

Answer:
- Depend on abstraction:
```csharp
interface IRepository { /*...*/ }
class Service { private readonly IRepository _repo; public Service(IRepository repo) => _repo = repo; }
```

Takeaway:
- High-level modules depend on abstractions; inject dependencies for testability and flexibility.
</details>

---

## Design Patterns (GoF + DI/Repository/CQRS)

<details>
<summary>Q82. Singleton — thread-safe and testable?</summary>

```csharp
class Singleton
{
    private static readonly Lazy<Singleton> _i = new(() => new Singleton());
    public static Singleton Instance => _i.Value;
    private Singleton() {}
}
```

Answer:
- Thread-safe lazy singleton. But singletons can be hard to test; prefer DI scope singletons when possible.

Takeaway:
- Use DI container singletons rather than hard-coded singletons for testability.
</details>

<details>
<summary>Q83. Factory Method vs Abstract Factory — when to use?</summary>

```csharp
abstract class Dialog { public void Render() { var btn = CreateButton(); /*...*/ } protected abstract IButton CreateButton(); }
```

Answer:
- Factory Method: let subclasses decide which product to create.
- Abstract Factory: provide families of related objects.

Takeaway:
- Use Factory Method for single product customization; Abstract Factory for families of products.
</details>

<details>
<summary>Q84. Strategy vs State — difference in intent?</summary>

```csharp
// Strategy: interchangeable algorithms
// State: behavior changes with internal state transitions
```

Answer:
- Strategy encapsulates algorithm variants set from outside.
- State encapsulates state-specific behavior and transitions inside the object.

Takeaway:
- Strategy is about selection; State is about transitions over time.
</details>

<details>
<summary>Q85. Decorator vs Proxy — which is which?</summary>

```csharp
// Both wrap another object
```

Answer:
- Decorator adds behavior transparently without changing interface.
- Proxy controls access (lazy init, remote, cache, security).

Takeaway:
- Choose Decorator to add responsibilities; Proxy to control access/indirection.
</details>

<details>
<summary>Q86. Repository pattern — when is it an anti-pattern?</summary>

```csharp
// Over-abstracting EF Core DbContext with thin repositories
```

Answer:
- If your repository just mirrors DbSet and loses EF features (query composition, Include, translation), it can be counterproductive.

Takeaway:
- Consider using DbContext directly with rich queries; use repositories only when adding meaningful abstraction boundaries.
</details>

<details>
<summary>Q87. CQRS — can you mix reads and writes?</summary>

```csharp
// Separate read models and write models
```

Answer:
- Yes, you separate concerns. Reads optimized for queries; writes for consistency. But complexity increases; don’t over-engineer.

Takeaway:
- Use CQRS where read/write requirements differ significantly; otherwise keep it simple.
</details>

<details>
<summary>Q88. Dependency Injection — service lifetimes pitfalls?</summary>

```csharp
// Injecting scoped service into singleton? -> invalid
```

Answer:
- In ASP.NET Core, injecting scoped into singleton causes captured scope bugs. Use factories (IServiceProvider.CreateScope) or rethink lifetimes.

Takeaway:
- Understand Singleton/Scoped/Transient interactions; avoid capturing scoped services inside singletons.
</details>

---
