# Control Flow (if/else, switch, loops)

[← Back to Main](../README.md) | [Previous: Value vs Reference Types ←](02-value-vs-reference-types.md) | [Next: Methods and Parameters →](04-methods-and-parameters.md)

---

<details>
<summary>Q13. if/else chaining — which branch executes?</summary>

```csharp
int x = 10;
if (x > 10) Console.Write("A");
else if (x == 10) Console.Write("B");
else Console.Write("C");
```

Answer:
- Prints `B`. Conditions are evaluated in order until one matches.

Takeaway:
- Order matters with if/else chains; place more specific conditions first.
</details>

<details>
<summary>Q14. switch expression basics — what prints?</summary>

```csharp
int n = 2;
string result = n switch
{
    1 => "one",
    2 => "two",
    _ => "other"
};
Console.WriteLine(result);
```

Answer:
- Prints `two`.

Takeaway:
- Switch expressions are concise and exhaustive. Use `_` for the default case.
</details>

<details>
<summary>Q15. for loop variable scope — what prints?</summary>

```csharp
for (int i = 0; i < 3; i++) { }
Console.WriteLine(i); // ?
```

Answer:
- Does not compile. `i` is scoped to the for loop.

Takeaway:
- Loop variables are local to the loop construct.
</details>

<details>
<summary>Q16. while vs do-while — how many prints?</summary>

```csharp
int i = 0;
while (i > 0) Console.WriteLine("W");
do { Console.WriteLine("D"); } while (i > 0);
```

Answer:
- Prints `D` once; while prints nothing. do-while executes the body at least once.

Takeaway:
- Use do-while when the body must run at least once before checking the condition.
</details>

<details>
<summary>Q17. break vs continue — what's the output?</summary>

```csharp
for (int i = 0; i < 5; i++)
{
    if (i == 2) continue;
    if (i == 4) break;
    Console.Write(i);
}
```

Answer:
- Prints `013`. `continue` skips the rest of the loop for `i==2`; `break` exits when `i==4`.

Takeaway:
- `continue` skips current iteration; `break` exits the loop entirely.
</details>

<details>
<summary>Q18. Basic pattern matching with type — what prints?</summary>

```csharp
object o = 5;
if (o is int v) Console.WriteLine(v + 1);
```

Answer:
- Prints `6`. The `is` pattern both checks type and introduces a variable.

Takeaway:
- Pattern matching reduces casting boilerplate and improves readability.
</details>

---

[← Back to Main](../README.md) | [Previous: Value vs Reference Types ←](02-value-vs-reference-types.md) | [Next: Methods and Parameters →](04-methods-and-parameters.md)
