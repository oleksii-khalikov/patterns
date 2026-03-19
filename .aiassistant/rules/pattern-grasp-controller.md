---
apply: by model decision
instructions: Apply when assigning responsibility for handling system events. Use for coordinating use cases without polluting UI or domain objects with orchestration logic.
---

# Controller (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Who should be assigned the responsibility for handling an incoming system event?
- UI objects are becoming tightly coupled to business logic.
- Domain entities are becoming polluted with coordination logic.

### 1.2. Triggers
- Use when you need an object to receive and coordinate a system operation.
- Use to maintain low coupling between the UI and the Domain layer.

## 2. Implementation Guidelines

1. **Identify System Events**: Determine which events need to be handled.
2. **Create Controller Class**: Create a class (e.g., Service or Facade) to receive these events.
3. **Delegate Work**: The controller should not perform business logic itself; it should delegate to domain objects.
4. **Coordinate Responses**: The controller manages transaction boundaries and coordinates the flow of the operation.

## 3. SOLID & GRASP Compliance

- Low Coupling: UI depends only on the Controller.
- High Cohesion: Controller focuses on coordination, while entities focus on business rules.
- Pure Fabrication: Controller is an artificial class created for architectural health.

## 4. Mandatory Constraints for LLM

1. **Avoid Bloated Controller**: Do not put business logic in the controller. Keep it thin.
2. **Transaction Management**: Controllers are ideal places for managing transactions.

## 5. Structural Template (Java)

```java
public class OrderService {
    private final OrderRepository repository;

    public OrderService(OrderRepository repository) { this.repository = repository; }

    public void processOrder(OrderRequest request) {
        // 1. Retrieval
        var order = repository.findById(request.orderId());
        // 2. Delegation (Information Expert)
        order.applyDiscount(request.discount());
        // 3. Persistence
        repository.save(order);
    }
}
```
