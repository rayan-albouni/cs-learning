# Interfaces and Abstract Classes

[← Back to Main](../README.md) | [Previous: Classes, Structs, and Basic OOP ←](05-classes-structs-basic-oop.md) | [Next: Collections and Arrays →](07-collections-arrays.md)

---

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

[← Back to Main](../README.md) | [Previous: Classes, Structs, and Basic OOP ←](05-classes-structs-basic-oop.md) | [Next: Collections and Arrays →](07-collections-arrays.md)
