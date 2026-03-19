# LLM Agent RuleSet: Pure Fabrication (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Where to place a responsibility when none of the domain classes are suitable experts?
- Placing logic in domain classes violates high cohesion or SRP.

### 1.2. Triggers
- Use when you need a place for logic that doesn't naturally belong to any domain object (e.g., persistence, logging, security).
- Use to maintain high cohesion in domain classes.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Identify 'Artificial' Class**: Create a class that doesn't represent a domain concept.
2. **Assign Responsibility**: Give it a clearly defined, highly cohesive set of responsibilities.
3. **Implement as Service or Utility**: Often implemented as a Service, Repository, or Helper.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Move infrastructure or cross-cutting concerns out of domain objects.
- High Cohesion: Fabrication classes are highly focused on a specific technical task.
- Low Coupling: Provides a way to decouple domain objects from technical details.

## 4. Mandatory Constraints for LLM

1. **Avoid Bloated Fabrications**: Don't use them as a dumping ground for unrelated logic.

## 5. Structural Template (Java)

```java
// Repository is a Pure Fabrication
public interface OrderRepository {
    void save(Order order);
    Order findById(UUID id);
}
```
