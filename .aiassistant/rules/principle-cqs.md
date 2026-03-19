---
apply: by model decision
instructions: Apply CQS (Command Query Separation) to separate methods that change state from those that return data. Use to improve predictability and reduce side effects.
---

# CQS (Command Query Separation)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Methods both modify state and return values, causing confusion.
- Side effects are hidden in getter-like methods.
- Difficult to reason about code behavior.
- Testing is complicated by methods that do too much.

### 1.2. Triggers
- Use when methods mix state changes with data retrieval.
- Use when calling a "getter" unexpectedly modifies state.
- Use when method names don't clearly indicate their effects.
- Use when debugging is difficult due to hidden side effects.

## 2. Implementation Guidelines

1. **Commands**: Methods that change state should return `void`.
2. **Queries**: Methods that return data should not change state.
3. **Clear Naming**: Commands use verbs (create, update, delete); queries use nouns or getters.
4. **Idempotent Queries**: Calling a query multiple times should yield the same result.

## 3. Mandatory Constraints for LLM

1. **Pure Queries**: Getters and find methods must not modify state.
2. **Void Commands**: State-changing methods should return void (or return `this` for fluent APIs).
3. **No Surprises**: Method behavior must match its name and signature.
4. **Exception**: Factory methods may create and return objects.

## 4. Structural Template (Java)

```java
// CQS Violation (Bad) - Mixed command and query
public class Stack<T> {
    public T pop() {  // Changes state AND returns value
        var item = items.removeLast();
        return item;
    }
}

// CQS Compliant (Good) - Separated
public class Stack<T> {
    // Query - returns data, no state change
    public T peek() {
        return items.getLast();
    }
    
    // Command - changes state, returns void
    public void pop() {
        items.removeLast();
    }
    
    // Usage: var item = stack.peek(); stack.pop();
}

// CQS in Repository
public interface OrderRepository {
    // Queries
    Optional<Order> findById(UUID id);
    List<Order> findByCustomer(UUID customerId);
    
    // Commands
    void save(Order order);
    void delete(UUID id);
}
```
