---
apply: by model decision
instructions: Apply when encapsulating requests as objects for parameterization, queuing, logging, or undo operations. Use for decoupling invokers from executors.
---

# Command (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to parameterize objects with operations.
- Need to queue, log, or support undoable operations.
- Need to decouple the object that invokes the operation from the object that performs it.

### 1.2. Triggers
- Use to parameterize objects by an action to perform.
- Use to specify, queue, and execute requests at different times.
- Use to support undo/redo operations.
- Use to structure a system around high-level operations built on primitive operations (transactions).

## 2. Implementation Guidelines

1. **Define Command Interface**: Declare an execute() method.
2. **Implement Concrete Commands**: Encapsulate a receiver and an action to invoke on the receiver.
3. **Invoker**: Asks the command to carry out the request.
4. **Receiver**: Knows how to perform the operations associated with carrying out a request.

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Each command encapsulates one action.
- Open/Closed Principle (OCP): New commands can be added without changing existing code.
- Indirection (GRASP): Command provides indirection between invoker and receiver.

## 4. Mandatory Constraints for LLM

1. **Undo Support**: If undo is needed, store enough state in the command to reverse the action.
2. **Macro Commands**: Consider composite commands for complex operations.

## 5. Structural Template (Java)

```java
public interface Command {
    void execute();
    void undo(); // Optional
}

public class CreateOrderCommand implements Command {
    private final OrderService receiver;
    private final OrderRequest request;
    private Order createdOrder;

    public CreateOrderCommand(OrderService receiver, OrderRequest request) {
        this.receiver = receiver;
        this.request = request;
    }

    @Override public void execute() { createdOrder = receiver.create(request); }
    @Override public void undo() { receiver.delete(createdOrder.id()); }
}
```
