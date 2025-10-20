# Strings and Basic Formatting/Interpolation

[← Back to Main](../README.md) | [Previous: Exceptions and Basic Error Handling ←](08-exceptions-error-handling.md) | [Next: Basic LINQ →](10-basic-linq.md)

---

<details>
<summary>Q48. String interpolation — what prints?</summary>

```csharp
int x = 5;
Console.WriteLine($"x = {x}, x+1 = {x + 1}");
```

Answer:
- Prints `x = 5, x+1 = 6`.

Takeaway:
- Interpolation embeds expressions inside strings; prefer for clarity.
</details>

<details>
<summary>Q49. Verbatim strings and escaping</summary>

```csharp
var path1 = "C:\\temp\\file.txt";
var path2 = @"C:\temp\file.txt";
Console.WriteLine(path1 == path2);
```

Answer:
- Prints `True`. Verbatim strings `@""` avoid needing to escape backslashes.

Takeaway:
- Use verbatim strings for paths and multiline text; double quotes are escaped as `""`.
</details>

<details>
<summary>Q50. String.Format and standard numeric formats</summary>

```csharp
double n = 1234.567;
Console.WriteLine(string.Format("{0:F2}", n));
Console.WriteLine($"{n:N1}");
```

Answer:
- Prints:
  - 1234.57
  - 1,234.6 (culture-dependent thousands separator)

Takeaway:
- Use standard numeric format strings; remember they are culture-sensitive by default.
</details>

<details>
<summary>Q51. String immutability — what prints?</summary>

```csharp
string s = "a";
s += "b";
Console.WriteLine(s);
```

Answer:
- Prints `ab`. Each concatenation creates a new string.

Takeaway:
- Strings are immutable. For repeated concatenations, use `StringBuilder` or `string.Create` when performance matters.
</details>

<details>
<summary>Q52. Case-insensitive comparisons</summary>

```csharp
string a = "Hello";
string b = "hello";
Console.WriteLine(a.Equals(b, StringComparison.OrdinalIgnoreCase));
```

Answer:
- Prints `True`.

Takeaway:
- Always specify `StringComparison` to avoid culture-related surprises.
</details>

---

[← Back to Main](../README.md) | [Previous: Exceptions and Basic Error Handling ←](08-exceptions-error-handling.md) | [Next: Basic LINQ →](10-basic-linq.md)
