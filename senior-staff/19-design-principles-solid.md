# Design Principles (SOLID)

[← Back to Main](../README.md) | [Previous: Attributes and Metadata ←](18-attributes-metadata.md) | [Next: Design Patterns →](20-design-patterns.md)

---

<details>
<summary>Q77. Single Responsibility Principle — what's wrong?</summary>

Answer:
- A class mixing validation, persistence, messaging, logging, document generation has too many responsibilities.

Takeaway:
- Each class should have one reason to change. Decouple concerns; compose via DI.
</details>

<details>
<summary>Q78. Open/Closed Principle — which approach is better?</summary>

Answer:
- Use polymorphism or rules engines rather than conditionals.

Takeaway:
- Open for extension, closed for modification.
</details>

<details>
<summary>Q79. Liskov Substitution Principle — violation example?</summary>

Answer:
- Substituting Square for Rectangle breaks expectations.

Takeaway:
- Model types correctly; avoid inheritance when invariants differ.
</details>

<details>
<summary>Q80. Interface Segregation Principle — what's the issue?</summary>

Answer:
- Clients forced to implement methods they don't need.

Takeaway:
- Prefer small, role-based interfaces.
</details>

<details>
<summary>Q81. Dependency Inversion — show inversion with abstractions</summary>

Answer:
- Depend on abstraction; inject dependencies.

Takeaway:
- High-level modules depend on abstractions.
</details>

---

[← Back to Main](../README.md) | [Previous: Attributes and Metadata ←](18-attributes-metadata.md) | [Next: Design Patterns →](20-design-patterns.md)
