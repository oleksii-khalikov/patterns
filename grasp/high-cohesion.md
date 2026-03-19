# LLM Agent RuleSet: High Cohesion (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- How to keep objects focused, understandable, and manageable?
- Classes with too many unrelated responsibilities (God Objects) are hard to maintain and reuse.

### 1.2. Triggers
- Use when a class is doing too many different things.
- Use when a class is too large and difficult to understand.
- Use when changes to one part of a class frequently affect unrelated parts.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Group Related Responsibilities**: Ensure that all methods and data in a class are closely related to its primary purpose.
2. **Extract Unrelated Logic**: Move responsibilities that don't fit into new or existing classes.
3. **Minimize Dependencies**: A highly cohesive class should depend on fewer other classes.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): High Cohesion is the GRASP equivalent of SRP.
- Maintainability: Cohesive classes are easier to test, understand, and modify.

## 4. Mandatory Constraints for LLM

1. **Avoid 'Fragmentation'**: Don't split classes so much that the system becomes hard to follow due to too many small classes.

## 5. Structural Template (Java)

```java
// Low Cohesion (Bad)
public class UserHandler {
    public void saveUser(User u) { /* db logic */ }
    public void sendEmail(User u) { /* email logic */ }
    public void validateUser(User u) { /* validation */ }
}

// High Cohesion (Good)
public class UserRepository { public void save(User u) { ... } }
public class EmailService { public void send(User u) { ... } }
public class UserValidator { public void validate(User u) { ... } }
```
