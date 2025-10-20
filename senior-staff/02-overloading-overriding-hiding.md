# Overloading, Overriding, Hiding

[← Back to Main](../README.md) | [Previous: Core C# Language Behavior ←](01-core-csharp-language-behavior.md) | [Next: Delegates, Events, Closures →](03-delegates-events-closures.md)

---

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
- Overload resolution prefers the more specific type. `null` is convertible to both `object` and `int?`, but `int?` is "better" here.

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
- `a + b` yields `null`. Lifted operator semantics return null if any operand is null.
- Prints `-1`.

Explanation:
- Nullable lifting of user-defined operators yields nullable result; null presence propagates.

Takeaway:
- Nullable lifting applies to user-defined operators. Know how nulls propagate in operator overloading.
</details>

---

[← Back to Main](../README.md) | [Previous: Core C# Language Behavior ←](01-core-csharp-language-behavior.md) | [Next: Delegates, Events, Closures →](03-delegates-events-closures.md)
