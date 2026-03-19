---
apply: by model decision
instructions: Apply when an object's behavior depends on its state and must change at runtime. Use for replacing complex conditional logic based on object state.
---

# State (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- An object's behavior changes based on its internal state.
- Complex conditional logic (if/else, switch) is used to handle state-dependent behavior.
- State transitions need to be managed explicitly and consistently.

### 1.2. Triggers
- Use when an object's behavior depends on its state, and it must change its behavior at runtime depending on that state.
- Use when operations have large, multipart conditional statements that depend on the object's state.

## 2. Implementation Guidelines

1. **Define State Interface**: Declare methods for state-specific behavior.
2. **Implement Concrete States**: Create a class for each state, encapsulating state-specific behavior.
3. **Context Class**: Maintains a reference to the current State object and delegates state-specific work to it.
4. **State Transitions**: Can be managed by the Context or by the State objects themselves.

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Each state class handles behavior for one state.
- Open/Closed Principle (OCP): New states can be added without modifying existing code.
- Information Expert: State objects are experts on their own behavior.

## 4. Mandatory Constraints for LLM

1. **State Immutability**: Prefer stateless State objects when possible.
2. **Transition Logic**: Clearly define where state transitions are managed (Context or State).
3. **Avoid Circular Dependencies**: Be careful with State objects holding references to Context.

## 5. Structural Template (Java)

```java
public interface OrderState {
    void next(Order order);
    void prev(Order order);
    String getStatus();
}

public class DraftState implements OrderState {
    @Override public void next(Order order) { order.setState(new SubmittedState()); }
    @Override public void prev(Order order) { /* no previous state */ }
    @Override public String getStatus() { return "DRAFT"; }
}

public class Order {
    private OrderState state = new DraftState();
    public void setState(OrderState state) { this.state = state; }
    public void nextState() { state.next(this); }
    public String getStatus() { return state.getStatus(); }
}
```
