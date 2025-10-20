# Exception Handling, Filters, Stack Preservation

[← Back to Main](../README.md) | [Previous: Nullable Reference Types ←](05-nullable-reference-types.md) | [Next: Async/Await Advanced →](07-async-await-advanced.md)

---

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
```

Answer:
- `throw;` preserves the original stack.
- `throw ex;` resets the stack to the throw site.

Takeaway:
- Always `throw;` when rethrowing inside catch.
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
- Prints: filter, caught

Explanation:
- Filters run before a catch block is entered.

Takeaway:
- Exception filters are great for conditional handling and logging.
</details>

<details>
<summary>Q29. AggregateException from Task.WhenAll — how many errors?</summary>

```csharp
var t = Task.WhenAll(Task.Run(() => throw new Exception("A")),
                     Task.Run(() => throw new Exception("B")));
try { await t; }
catch (Exception e) { /* handle */ }
```

Answer:
- Catches `AggregateException` with two InnerExceptions: A and B.

Takeaway:
- `Task.WhenAll` aggregates all failures.
</details>

---

[← Back to Main](../README.md) | [Previous: Nullable Reference Types ←](05-nullable-reference-types.md) | [Next: Async/Await Advanced →](07-async-await-advanced.md)
