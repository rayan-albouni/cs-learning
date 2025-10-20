# yield return and Iterator Blocks

[← Back to Main](../README.md) | [Previous: LINQ Advanced ←](09-linq-advanced.md) | [Next: Entity Framework Core →](11-entity-framework-core.md)

---

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
```

Answer:
- Disposing the enumerator triggers the iterator's finally block.

Takeaway:
- Iterator `finally` ensures cleanup on disposal.
</details>

<details>
<summary>Q47. yield and exception timing — when is exception thrown?</summary>

Answer:
- Prints `1`, then throws on the next MoveNext after consuming the first item.

Takeaway:
- Exceptions in iterators surface during MoveNext at enumeration time.
</details>

---

[← Back to Main](../README.md) | [Previous: LINQ Advanced ←](09-linq-advanced.md) | [Next: Entity Framework Core →](11-entity-framework-core.md)
