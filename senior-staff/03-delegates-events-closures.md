# Delegates, Events, Closures

[← Back to Main](../README.md) | [Previous: Overloading, Overriding, Hiding ←](02-overloading-overriding-hiding.md) | [Next: Generics →](04-generics.md)

---

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
- Since C# 5, foreach iteration variable is captured per iteration, unlike for-loops.

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
<summary>Q18. Multicast delegate return value — what's returned?</summary>

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
- Don't rely on return values of multicast delegates. Use event arguments or gather results explicitly.
</details>

---

[← Back to Main](../README.md) | [Previous: Overloading, Overriding, Hiding ←](02-overloading-overriding-hiding.md) | [Next: Generics →](04-generics.md)
