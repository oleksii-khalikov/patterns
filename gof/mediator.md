# LLM Agent RuleSet: Mediator (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- A set of objects communicate in well-defined but complex ways.
- Dependencies between objects are unstructured and difficult to understand (spaghetti code).
- Reuse of an object is difficult because it refers to and communicates with many other objects.

### 1.2. Triggers
- Use when a set of objects communicate in well-defined but complex ways.
- Use when reusing an object is difficult because it refers to and communicates with many other objects.
- Use when a behavior that's distributed between several classes should be customizable without a lot of subclassing.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Mediator Interface**: Declare methods for communication between components.
2. **Implement Concrete Mediator**: Coordinates communication between various components. It often stores references to them.
3. **Define Component Classes**: Components don't know about each other; they only know about the mediator.
4. **Implement Communication**: Components notify the mediator about events; the mediator decides who should respond.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Centralize communication logic in one place.
- Open/Closed Principle (OCP): New mediators can be introduced without changing components.
- Low Coupling: Components are decoupled from each other.

## 4. Mandatory Constraints for LLM

1. **Avoid God Object**: The Mediator can easily become too complex. If it does, split it.
2. **Performance**: Centralized communication can sometimes be a bottleneck.

## 5. Structural Template (Java)

```java
public interface Mediator {
    void notify(Component sender, String event);
}

public class AuthenticationDialog implements Mediator {
    private Button loginButton;
    private TextBox username;
    @Override
    public void notify(Component sender, String event) {
        if (sender == loginButton && event.equals("click")) {
            // validate username and login
        }
    }
}
```
