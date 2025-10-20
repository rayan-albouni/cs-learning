# Collections and Arrays

[← Back to Main](../README.md) | [Previous: Interfaces and Abstract Classes ←](06-interfaces-abstract-classes.md) | [Next: Exceptions and Basic Error Handling →](08-exceptions-error-handling.md)

---

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
<summary>Q38. Range slicing — what's the result?</summary>

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
- Don't structurally modify a collection while enumerating it. Collect changes and apply later.
</details>

---

[← Back to Main](../README.md) | [Previous: Interfaces and Abstract Classes ←](06-interfaces-abstract-classes.md) | [Next: Exceptions and Basic Error Handling →](08-exceptions-error-handling.md)
