# LLM Agent RuleSet: Observer (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- An abstraction has two aspects, one dependent on the other.
- A change to one object requires changing others, and you don't know how many objects need to be changed.
- An object should be able to notify other objects without making assumptions about who these objects are.

### 1.2. Triggers
- Use when a change to one object requires changing others, and you don't know how many objects need to be changed.
- Use when an object should be able to notify other objects without making assumptions about who these objects are.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Subject**: Maintains a list of observers and provides methods to attach/detach them.
2. **Define Observer Interface**: Declares an update method to be called by the subject.
3. **Implement Concrete Observers**: Perform actions in response to notifications from the subject.
4. **Notification Mechanism**: The subject calls update() on all its observers when its state changes.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Open/Closed Principle (OCP): New observers can be added without modifying the subject.
- Low Coupling: Subject and observers are loosely coupled.
- High Cohesion: Observers encapsulate specific reaction logic.

## 4. Mandatory Constraints for LLM

1. **Memory leaks**: Be sure to detach observers when they are no longer needed (or use weak references).
2. **Unexpected updates**: Observers don't know about each other, and can cause a cascade of updates.

## 5. Structural Template (Java)

```java
public interface EventListener {
    void update(String eventType, File file);
}

public class EventManager {
    private final Map<String, List<EventListener>> listeners = new HashMap<>();
    public void subscribe(String eventType, EventListener listener) { /* ... */ }
    public void notify(String eventType, File file) {
        for (var listener : listeners.get(eventType)) {
            listener.update(eventType, file);
        }
    }
}
```
