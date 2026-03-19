# LLM Agent RuleSet: Indirection (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- How to avoid direct coupling between two or more objects?
- How to decouple objects so that they can be easily replaced or modified?

### 1.2. Triggers
- Use when you want to avoid a direct dependency between components.
- Use when a component needs to be replaceable (e.g., using an interface).

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Introduce Intermediary**: Create a class or interface that acts as a buffer between the two components.
2. **Depend on Abstraction**: Ensure components communicate through the intermediary rather than directly.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Low Coupling: Indirection is a primary tool for achieving low coupling.
- Protected Variations: The intermediary protects one component from changes in another.
- Dependency Inversion Principle (DIP): Indirection via interfaces supports DIP.

## 4. Mandatory Constraints for LLM

1. **Performance**: Every layer of indirection adds a small amount of overhead.
2. **Complexity**: Too much indirection can make the code harder to follow.

## 5. Structural Template (Java)

```java
// Direct coupling (Bad)
public class OrderService {
    private final MySQLRepository repo = new MySQLRepository();
}

// Indirection (Good)
public class OrderService {
    private final Repository repo;
    public OrderService(Repository repo) { this.repo = repo; }
}
```
