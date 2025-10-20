# Basic LINQ (Select, Where, First, Count, etc.)

[← Back to Main](../README.md) | [Previous: Strings and Basic Formatting ←](09-strings-formatting.md) | [Next: Simple async/await and Task Basics →](11-async-await-basics.md)

---

<details>
<summary>Q53. Select and Where — what's the output?</summary>

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

[← Back to Main](../README.md) | [Previous: Strings and Basic Formatting ←](09-strings-formatting.md) | [Next: Simple async/await and Task Basics →](11-async-await-basics.md)
