---
apply: by model decision
instructions: Apply when providing sequential access to elements of an aggregate without exposing its internal structure. Use for uniform traversal of collections.
---

# Iterator (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to access elements of an aggregate object sequentially without exposing its underlying representation.
- Need to support multiple traversal algorithms over a collection.
- Need a uniform interface for traversing different aggregate structures.

### 1.2. Triggers
- Use to access an aggregate object's contents without exposing its internal representation.
- Use to support multiple traversals of aggregate objects.
- Use to provide a uniform interface for traversing different aggregate structures.

## 2. Implementation Guidelines

1. **Define Iterator Interface**: Declare methods for traversal (hasNext, next, etc.).
2. **Implement Concrete Iterator**: Keeps track of the current position in the traversal.
3. **Define Aggregate Interface**: Declares a method to create an Iterator.
4. **Implement Concrete Aggregate**: Returns an instance of the proper ConcreteIterator.

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Separates traversal from the aggregate.
- Open/Closed Principle (OCP): New iterators can be added without changing aggregates.
- Information Expert: Iterator is the expert on traversal.

## 4. Mandatory Constraints for LLM

1. **Thread Safety**: Be aware that iterators are generally not thread-safe.
2. **Fail-Fast**: Consider implementing fail-fast behavior for concurrent modifications.

## 5. Structural Template (Java)

```java
public interface Iterator<T> {
    boolean hasNext();
    T next();
}

public interface IterableCollection<T> {
    Iterator<T> createIterator();
}

public class TreeCollection<T> implements IterableCollection<T> {
    private List<T> nodes = new ArrayList<>();

    @Override
    public Iterator<T> createIterator() {
        return new TreeIterator<>(nodes);
    }
}

public class TreeIterator<T> implements Iterator<T> {
    private final List<T> nodes;
    private int position = 0;

    public TreeIterator(List<T> nodes) { this.nodes = nodes; }
    @Override public boolean hasNext() { return position < nodes.size(); }
    @Override public T next() { return nodes.get(position++); }
}
```
