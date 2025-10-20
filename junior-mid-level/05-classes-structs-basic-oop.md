# Classes, Structs, and Basic OOP

[← Back to Main](../README.md) | [Previous: Methods and Parameters ←](04-methods-and-parameters.md) | [Next: Interfaces and Abstract Classes →](06-interfaces-abstract-classes.md)

---

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

[← Back to Main](../README.md) | [Previous: Methods and Parameters ←](04-methods-and-parameters.md) | [Next: Interfaces and Abstract Classes →](06-interfaces-abstract-classes.md)
