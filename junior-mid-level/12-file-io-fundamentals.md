# File I/O Fundamentals

[← Back to Main](../README.md) | [Previous: Simple async/await and Task Basics ←](11-async-await-basics.md) | [Next: Simple Unit Testing →](13-simple-unit-testing.md)

---

<details>
<summary>Q64. Reading and writing text files — basics</summary>

```csharp
string path = "hello.txt";
File.WriteAllText(path, "Hi");
string content = File.ReadAllText(path);
Console.WriteLine(content);
```

Answer:
- Prints `Hi`.

Takeaway:
- `File.WriteAllText` and `ReadAllText` are convenient for small text files.
</details>

<details>
<summary>Q65. Using statements and streams — disposal order</summary>

```csharp
using var fs = File.OpenRead("hello.txt");
using var sr = new StreamReader(fs);
Console.WriteLine(await sr.ReadToEndAsync());
```

Answer:
- Properly disposes `StreamReader` then `FileStream` at the end of scope.

Takeaway:
- Use `using` declarations for deterministic disposal of I/O resources.
</details>

<details>
<summary>Q66. Path.Combine vs string concatenation</summary>

```csharp
string folder = "C:\\temp";
string file = "data.txt";
Console.WriteLine(Path.Combine(folder, file));
```

Answer:
- Prints `C:\temp\data.txt`. `Path.Combine` handles separators and edge cases.

Takeaway:
- Use `Path.Combine` for safe, cross-platform path building.
</details>

<details>
<summary>Q67. File.WriteAllLines and ReadAllLines</summary>

```csharp
string path = "lines.txt";
File.WriteAllLines(path, new[] { "a", "b", "c" });
Console.WriteLine(string.Join("-", File.ReadAllLines(path)));
```

Answer:
- Prints `a-b-c`.

Takeaway:
- Use the `AllLines` helpers for small batches of lines.
</details>

<details>
<summary>Q68. Async file read — don't block</summary>

```csharp
await File.WriteAllTextAsync("data.txt", "content");
string text = await File.ReadAllTextAsync("data.txt");
Console.WriteLine(text);
```

Answer:
- Prints `content`.

Takeaway:
- Prefer async I/O on server apps to avoid blocking threads.
</details>

---

[← Back to Main](../README.md) | [Previous: Simple async/await and Task Basics ←](11-async-await-basics.md) | [Next: Simple Unit Testing →](13-simple-unit-testing.md)
