# LLM Agent RuleSet: Decorator (Structural Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to add responsibilities to objects dynamically without affecting other objects.
- Inheritance leads to a combinatorial explosion of subclasses.
- Behavior needs to be combined in various ways at runtime.

### 1.2. Triggers
- Use to add responsibilities to individual objects dynamically and transparently.
- Use for responsibilities that can be withdrawn.
- Use when extension by subclassing is impractical.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Component Interface**: Common interface for both objects and decorators.
2. **Implement Concrete Component**: The basic object to which additional responsibilities can be added.
3. **Define Base Decorator**: Implements the Component interface and wraps a Component object.
4. **Implement Concrete Decorators**: Add specific responsibilities by overriding Component methods and calling the wrapped object.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Each decorator adds a single responsibility.
- Open/Closed Principle (OCP): Classes are open for extension (via decorators) but closed for modification.
- Indirection (GRASP): Decorators provide a layer of indirection.

## 4. Mandatory Constraints for LLM

1. **Interface Identity**: A decorator must implement the same interface as the wrapped object.
2. **Lightweight**: Decorators should be thin and only add the necessary behavior.

## 5. Structural Template (Java)

```java
public interface Notifier {
    void send(String message);
}

public class EmailNotifier implements Notifier {
    @Override public void send(String message) { /* send email */ }
}

public abstract class NotifierDecorator implements Notifier {
    protected final Notifier wrappee;
    protected NotifierDecorator(Notifier wrappee) { this.wrappee = wrappee; }
    @Override public void send(String message) { wrappee.send(message); }
}

public class SmsDecorator extends NotifierDecorator {
    public SmsDecorator(Notifier wrappee) { super(wrappee); }
    @Override
    public void send(String message) {
        super.send(message);
        sendSms(message);
    }
    private void sendSms(String m) { /* ... */ }
}
```
