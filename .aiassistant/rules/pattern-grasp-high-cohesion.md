---
apply: by model decision
instructions: Apply when keeping classes focused and manageable with closely related responsibilities. Use to avoid God Objects and ensure single-purpose classes.
---

# High Cohesion (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- How to keep objects focused, understandable, and manageable?
- Classes with too many unrelated responsibilities (God Objects) are hard to maintain and reuse.

### 1.2. Triggers
- Use when a class is doing too many different things.
- Use when a class is too large and difficult to understand.
- Use when changes to one part of a class frequently affect unrelated parts.

## 2. Implementation Guidelines

1. **Group Related Responsibilities**: Ensure that all methods and data in a class are closely related to its primary purpose.
2. **Extract Unrelated Logic**: Move responsibilities that don't fit into new or existing classes.
3. **Minimize Dependencies**: A highly cohesive class should depend on fewer other classes.

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
