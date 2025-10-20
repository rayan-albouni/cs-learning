# Simple async/await and Task Basics

[← Back to Main](../README.md) | [Previous: Basic LINQ ←](10-basic-linq.md) | [Next: File I/O Fundamentals →](12-file-io-fundamentals.md)

---

<details>
<summary>Q59. Basic await — what prints order?</summary>

```csharp
async Task Demo()
{
    Console.WriteLine("Start");
    await Task.Delay(50);
    Console.WriteLine("End");
}
await Demo();
```

Answer:
- Prints:
  - Start
  - End

Takeaway:
- `await` asynchronously yields control; execution resumes when the awaited task completes.
</details>

<details>
<summary>Q60. Task.Run to offload work</summary>

```csharp
int Compute() { Thread.Sleep(50); return 42; }
int result = await Task.Run(Compute);
Console.WriteLine(result);
```

Answer:
- Prints `42`. `Task.Run` executes CPU-bound work on a thread pool thread.

Takeaway:
- Use `Task.Run` to offload CPU-bound work; don't wrap inherently asynchronous I/O with Task.Run unnecessarily.
</details>

<details>
<summary>Q61. Async method returning Task — exceptions</summary>

```csharp
async Task<int> F()
{
    await Task.Delay(10);
    throw new InvalidOperationException("boom");
}
try
{
    var x = await F();
}
catch (InvalidOperationException)
{
    Console.WriteLine("Caught");
}
```

Answer:
- Prints `Caught`. Exceptions in async methods are captured into the Task and rethrown on await.

Takeaway:
- Await to observe exceptions; don't ignore tasks or use async void (except event handlers).
</details>

<details>
<summary>Q62. Multiple awaits in sequence — order?</summary>

```csharp
await Task.Delay(10);
await Task.Delay(10);
Console.WriteLine("Done");
```

Answer:
- Executes sequentially; total delay ~20ms plus overhead.

Takeaway:
- For independent operations, consider starting tasks concurrently and awaiting with `await Task.WhenAll(...)`.
</details>

<details>
<summary>Q63. Returning completed tasks — fast path</summary>

```csharp
Task<int> Return42() => Task.FromResult(42);
Console.WriteLine(await Return42());
```

Answer:
- Prints `42`. `Task.FromResult` returns an already-completed Task without allocation of a state machine.

Takeaway:
- Use `Task.FromResult` for synchronous, already-known results.
</details>

---

[← Back to Main](../README.md) | [Previous: Basic LINQ ←](10-basic-linq.md) | [Next: File I/O Fundamentals →](12-file-io-fundamentals.md)
