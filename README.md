# The Ultimate .NET and C# Interview Questions

**Audience:** Junior, Mid-level, Senior, and Staff .NET engineers

**Version assumptions:** C# 12, .NET 8+

---

## 📚 Navigation

This repository is organized as an interactive learning platform for C# and .NET interview preparation. Click on any topic below to dive into detailed questions and answers.

---

## 👨‍💻 Junior and Mid-level Engineers

### Core Fundamentals
1. **[Variables, Data Types, and Type Inference](junior-mid-level/01-variables-data-types-type-inference.md)**
   - var type inference, default literals, numeric types, constants, casting vs Convert

2. **[Value vs Reference Types](junior-mid-level/02-value-vs-reference-types.md)**
   - Value/reference semantics, boxing, nullable value types, equality comparisons

3. **[Control Flow](junior-mid-level/03-control-flow.md)**
   - if/else, switch expressions, loops, break/continue, pattern matching

4. **[Methods and Parameters](junior-mid-level/04-methods-and-parameters.md)**
   - Overloading, ref/out/in parameters, return by ref, optional/named parameters

5. **[Classes, Structs, and Basic OOP](junior-mid-level/05-classes-structs-basic-oop.md)**
   - Constructors, encapsulation, inheritance, virtual/override, method hiding, init setters

6. **[Interfaces and Abstract Classes](junior-mid-level/06-interfaces-abstract-classes.md)**
   - Interface implementation, explicit interface implementation, default interface methods, sealed overrides

7. **[Collections and Arrays](junior-mid-level/07-collections-arrays.md)**
   - Array indexing, ranges, List, Dictionary, Queue, Stack, foreach vs for

8. **[Exceptions and Basic Error Handling](junior-mid-level/08-exceptions-error-handling.md)**
   - Catching exceptions, finally blocks, throwing exceptions, rethrowing

9. **[Strings and Basic Formatting](junior-mid-level/09-strings-formatting.md)**
   - String interpolation, verbatim strings, formatting, immutability, comparisons

10. **[Basic LINQ](junior-mid-level/10-basic-linq.md)**
    - Select, Where, First/FirstOrDefault, Count/Any, OrderBy/ThenBy, SelectMany

11. **[Simple async/await and Task Basics](junior-mid-level/11-async-await-basics.md)**
    - Basic await, Task.Run, exceptions, sequential awaits, Task.FromResult

12. **[File I/O Fundamentals](junior-mid-level/12-file-io-fundamentals.md)**
    - Reading/writing files, using statements, Path.Combine, async file operations

13. **[Simple Unit Testing](junior-mid-level/13-simple-unit-testing.md)**
    - Arrange-Act-Assert, parameterized tests, testing exceptions, async tests, floating-point tolerances

14. **[Extended Fundamentals](junior-mid-level/14-extended-fundamentals.md)** (Q74-Q100)
    - Null operators, enums, TryParse, DateTime, IndexOf, guards, tuples, records, top-level statements

---

## 🎯 Senior and Staff Engineers

### Advanced Language Features
1. **[Core C# Language Behavior](senior-staff/01-core-csharp-language-behavior.md)**
   - Numeric overflow, default literal, using declarations, static constructors, optional parameters, extensions, equality, NaN, pattern matching

2. **[Overloading, Overriding, Hiding](senior-staff/02-overloading-overriding-hiding.md)**
   - new vs override, overload resolution, extension methods, operator overloading with nullable

3. **[Delegates, Events, Closures](senior-staff/03-delegates-events-closures.md)**
   - Closure over loop variables, foreach closures, event unsubscription, multicast delegates

4. **[Generics: Constraints, Variance, Reification](senior-staff/04-generics.md)**
   - Invariance, covariance/contravariance, struct/unmanaged/notnull constraints, reified generics, default(T)

5. **[Nullable Reference Types and Nullability Operators](senior-staff/05-nullable-reference-types.md)**
   - Null-forgiving operator (!), null-coalescing assignment (??=), null-conditional chaining (?.)

### Async & Concurrency
6. **[Exception Handling, Filters, Stack Preservation](senior-staff/06-exception-handling.md)**
   - Rethrow vs throw ex, exception filters, AggregateException

7. **[Async/Await: Deadlocks, ConfigureAwait, ValueTask](senior-staff/07-async-await-advanced.md)**
   - Deadlocks with .Result, ConfigureAwait(false), async void, ValueTask, IAsyncEnumerable

8. **[Concurrency & Synchronization Primitives](senior-staff/08-concurrency-synchronization.md)**
   - lock behavior, lock(this) risks, SemaphoreSlim, ConcurrentDictionary, Interlocked

### Data Access & Testing
9. **[LINQ: Deferred Execution, IEnumerable vs IQueryable](senior-staff/09-linq-advanced.md)**
   - Deferred execution, multiple enumeration, IQueryable translation, OrderBy stability, Distinct

10. **[yield return and Iterator Blocks](senior-staff/10-yield-iterator-blocks.md)**
    - finally in iterators, exception timing

11. **[Entity Framework Core](senior-staff/11-entity-framework-core.md)**
    - Tracking vs AsNoTracking, lazy loading, context lifetime, N+1 problem, AsSplitQuery, client evaluation, concurrency tokens, SQL injection

12. **[Unit Testing Pitfalls](senior-staff/12-unit-testing-pitfalls.md)**
    - async void tests, mocking EF Core, floating-point assertions, time-dependent tests

### Performance & Low-Level
13. **[Struct vs Class, Boxing/Unboxing, in/out/ref](senior-staff/13-struct-class-boxing.md)**
    - Boxing via interface, mutating struct properties, readonly struct, defensive copies, ref/out/in parameters

14. **[Span/Memory and Performance Types](senior-staff/14-span-memory-performance.md)**
    - Span escape rules, capturing Span in lambdas, string.Create with spans

15. **[Reflection, Expression Trees, Dynamic Invocation](senior-staff/15-reflection-expression-trees.md)**
    - DynamicInvoke vs CreateDelegate, Expression.Compile caching, BindingFlags

16. **[Compiler/JIT Optimizations](senior-staff/16-compiler-jit-optimizations.md)**
    - Bounds check elimination, AggressiveInlining, volatile keyword

17. **[Interop and unsafe](senior-staff/17-interop-unsafe.md)**
    - stackalloc and Span, fixed keyword, delegate* unmanaged function pointers

18. **[Attributes and Metadata](senior-staff/18-attributes-metadata.md)**
    - Attribute parameters as constants, Conditional attribute

### Design & Architecture
19. **[Design Principles (SOLID)](senior-staff/19-design-principles-solid.md)**
    - Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion

20. **[Design Patterns (GoF + DI/Repository/CQRS)](senior-staff/20-design-patterns.md)**
    - Singleton, Factory Method vs Abstract Factory, Strategy vs State, Decorator vs Proxy, Repository pattern, CQRS, DI service lifetimes

---

## 🚀 How to Use This Repository

This repository is designed as an **interactive learning app** for cracking C# and .NET technical interviews:

1. **Navigate by skill level:** Start with Junior/Mid-level if you're building fundamentals, or jump to Senior/Staff for advanced topics
2. **Follow the learning path:** Each file includes navigation links to move to the previous, next, or back to the main page
3. **Test your knowledge:** Questions are presented with expandable details - try answering before expanding
4. **Learn from explanations:** Each question includes the answer, explanation, and key takeaway

### Navigation Structure
- **[← Back to Main]** - Returns to this main page
- **[Previous: Topic ←]** - Goes to the previous topic in the sequence
- **[Next: Topic →]** - Proceeds to the next topic

---

## 📝 License

This project is licensed under the terms specified in the [LICENSE](LICENSE) file.

---

## 🤝 Contributing

Contributions are welcome! If you have additional questions, corrections, or improvements, feel free to open an issue or submit a pull request.

---

**Happy Learning! 🎓**
