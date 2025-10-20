# Simple Unit Testing

[← Back to Main](../README.md) | [Previous: File I/O Fundamentals ←](12-file-io-fundamentals.md) | [Next: Extended Fundamentals →](14-extended-fundamentals.md)

---

<details>
<summary>Q69. Arrange-Act-Assert — basics</summary>

```csharp
// Pseudo xUnit
int Add(int a, int b) => a + b;

[Fact]
public void Add_AddsTwoNumbers()
{
    // Arrange
    var a = 2; var b = 3;

    // Act
    var result = Add(a, b);

    // Assert
    Assert.Equal(5, result);
}
```

Answer:
- Test passes. Clear AAA layout improves readability.

Takeaway:
- Structure tests with Arrange, Act, Assert for clarity and maintainability.
</details>

<details>
<summary>Q70. Parameterized tests reduce duplication</summary>

```csharp
[Theory]
[InlineData(1, 2, 3)]
[InlineData(-1, 1, 0)]
public void Add_Works(int a, int b, int expected)
{
    Assert.Equal(expected, a + b);
}
```

Answer:
- Runs multiple scenarios succinctly.

Takeaway:
- Use data-driven tests to cover more cases with less repetition.
</details>

<details>
<summary>Q71. Testing exceptions — expected throws</summary>

```csharp
void RequireNonNull(string s)
{
    if (s is null) throw new ArgumentNullException(nameof(s));
}
[Fact]
public void RequireNonNull_ThrowsOnNull()
{
    Assert.Throws<ArgumentNullException>(() => RequireNonNull(null!));
}
```

Answer:
- Test passes when exception is thrown.

Takeaway:
- Use `Assert.Throws` (or `Assert.ThrowsAsync`) for exception-based behavior.
</details>

<details>
<summary>Q72. Avoid async void in tests</summary>

```csharp
[Fact]
public async Task TestAsync()
{
    await Task.Delay(10);
    Assert.True(true);
}
```

Answer:
- Correct. Tests should return `Task` for async operations.

Takeaway:
- `async void` cannot be awaited and may cause false positives or lost exceptions.
</details>

<details>
<summary>Q73. Floating-point tolerance in tests</summary>

```csharp
[Fact]
public void Double_Sum_Tolerance()
{
    var sum = 0.1 + 0.2;
    Assert.True(Math.Abs(sum - 0.3) < 1e-12);
}
```

Answer:
- Test passes with a reasonable tolerance.

Takeaway:
- Use tolerances for floating-point comparisons; avoid exact equality.
</details>

---

[← Back to Main](../README.md) | [Previous: File I/O Fundamentals ←](12-file-io-fundamentals.md) | [Next: Extended Fundamentals →](14-extended-fundamentals.md)
