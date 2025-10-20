# Extended Fundamentals — Additional Quick Puzzles

[← Back to Main](../README.md) | [Previous: Simple Unit Testing ←](13-simple-unit-testing.md)

---

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
- `!` only suppresses compiler warnings; it doesn't prevent runtime nulls.
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

[← Back to Main](../README.md) | [Previous: Simple Unit Testing ←](13-simple-unit-testing.md)
