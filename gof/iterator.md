# LLM Agent RuleSet: Iterator (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to access elements of an aggregate object sequentially without exposing its underlying representation.
- Need to support multiple traversals of aggregate objects.
- Need to provide a uniform interface for traversing different aggregate structures.

### 1.2. Triggers
- Use to access an aggregate object's contents without exposing its internal representation.
- Use to support multiple traversals of aggregate objects.
- Use to provide a uniform interface for traversing different aggregate structures.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Iterator Interface**: Declare methods for traversing (next, hasNext).
2. **Implement Concrete Iterator**: Stores current position and traversal logic for a specific aggregate.
3. **Define Aggregate Interface**: Declares a method for creating an iterator.
4. **Implement Concrete Aggregate**: Returns an instance of the corresponding Concrete Iterator.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Move traversal logic out of the aggregate class.
- Open/Closed Principle (OCP): New types of collections and iterators can be added without breaking existing code.
- Polymorphism: Client uses the Iterator interface regardless of the collection type.

## 4. Mandatory Constraints for LLM

1. **Fail-fast vs Fail-safe**: Decide how to handle concurrent modifications to the collection during iteration.
2. **State management**: The iterator must maintain its own state (e.g., current index).

## 5. Structural Template (Java)

```java
public interface Iterator<T> {
    T next();
    boolean hasNext();
}

public class ListIterator<T> implements Iterator<T> {
    private final List<T> list;
    private int index = 0;
    public ListIterator(List<T> list) { this.list = list; }
    @Override public boolean hasNext() { return index < list.size(); }
    @Override public T next() { return list.get(index++); }
}
```
