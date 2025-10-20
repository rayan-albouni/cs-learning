# Exceptions and Basic Error Handling

[← Back to Main](../README.md) | [Previous: Collections and Arrays ←](07-collections-arrays.md) | [Next: Strings and Basic Formatting →](09-strings-formatting.md)

---

<details>
<summary>Q43. Catching specific exception types — which catch runs?</summary>

```csharp
try
{
    int x = int.Parse("not-an-int");
}
catch (FormatException)
{
    Console.WriteLine("Format");
}
catch (Exception)
{
    Console.WriteLine("General");
}
```

Answer:
- Prints `Format`. More specific catch should appear before general catch.

Takeaway:
- Order catch blocks from most specific to most general.
</details>

<details>
<summary>Q44. finally block behavior — does it always run?</summary>

```csharp
try
{
    Console.WriteLine("Try");
}
finally
{
    Console.WriteLine("Finally");
}
```

Answer:
- Prints:
  - Try
  - Finally

Takeaway:
- `finally` runs whether or not an exception is thrown, including most early returns; avoid `Environment.FailFast` or process termination if cleanup is required.
</details>

<details>
<summary>Q45. Throwing your own exception — best practice?</summary>

```csharp
if (filePath is null)
    throw new ArgumentNullException(nameof(filePath));
```

Question:
- Why use `nameof` and specific exception types?

Answer:
- `nameof` reduces magic strings; specific exception types communicate intent and enable targeted handling.

Takeaway:
- Throw the most specific applicable exception and use `nameof` for parameter names.
</details>

<details>
<summary>Q46. Try-catch around minimal code</summary>

```csharp
try
{
    // many lines of code
    var content = File.ReadAllText("missing.txt");
    // many more lines
}
catch (IOException ex)
{
    Console.WriteLine(ex.Message);
}
```

Question:
- What's a better structure?

Answer:
- Wrap only the risky call in try-catch or separate it into a method to avoid catching unrelated errors.

Takeaway:
- Keep try blocks small and focused on the operations that can throw the expected exceptions.
</details>

<details>
<summary>Q47. Re-throwing with `throw;` preserves stack</summary>

```csharp
try
{
    MightThrow();
}
catch (Exception)
{
    // log
    throw;
}
```

Answer:
- `throw;` rethrows with original stack trace intact.

Takeaway:
- Prefer `throw;` to preserve stack trace; avoid `throw ex;` unless you intentionally reset context.
</details>

---

[← Back to Main](../README.md) | [Previous: Collections and Arrays ←](07-collections-arrays.md) | [Next: Strings and Basic Formatting →](09-strings-formatting.md)
