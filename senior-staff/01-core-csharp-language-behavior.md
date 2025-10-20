# Core C# Language Behavior

[← Back to Main](../README.md) | [Next: Overloading, Overriding, Hiding →](02-overloading-overriding-hiding.md)

---

<details>
<summary>Q1. Numeric overflow, checked/unchecked — what prints?</summary>

```csharp
using System;

class Program
{
    static void Main()
    {
        int a = int.MaxValue;
        Console.WriteLine(a + 1); // ?
        
        try
        {
            checked { Console.WriteLine(int.MaxValue + 1); }
        }
        catch (OverflowException)
        {
            Console.WriteLine("OverflowException");
        }
        
        unchecked
        {
            Console.WriteLine(int.MaxValue + 1);
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
- For financial/security-sensitive arithmetic, use `checked` (globally or locally) or types like `decimal`. Know your project's overflow settings.
</details>

<details>
<summary>Q2. Default literal and nullable value types — what's the type and value?</summary>

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
- `c.M(null);` chooses `M(object)` since `null` can't convert to `int` but is valid for `object`.
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
- `object.Equals(x, x)` prints `True` (because `Double.Equals` is special-cased to return true for NaN).
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
<summary>Q10. Deconstruction and discards — what's the result?</summary>

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

[← Back to Main](../README.md) | [Next: Overloading, Overriding, Hiding →](02-overloading-overriding-hiding.md)
