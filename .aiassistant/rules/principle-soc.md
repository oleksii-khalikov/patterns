---
apply: by model decision
instructions: Apply SoC (Separation of Concerns) to divide programs into distinct sections. Use to isolate business logic, data access, and UI into independent modules.
---

# SoC (Separation of Concerns)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Code handles multiple unrelated responsibilities in one place.
- Changes to one aspect (e.g., UI) require changes to unrelated aspects (e.g., business logic).
- Modules are tightly coupled and hard to test in isolation.
- Code reuse is difficult because concerns are intertwined.

### 1.2. Triggers
- Use when a class/method handles both presentation and business logic.
- Use when data access code is mixed with domain logic.
- Use when cross-cutting concerns (logging, security) pollute business code.
- Use when the same code block handles validation, transformation, and persistence.

## 2. Implementation Guidelines

1. **Layered Architecture**: Separate UI, Business Logic, and Data Access layers.
2. **Single Purpose Modules**: Each module addresses one concern.
3. **Clean Interfaces**: Define clear contracts between concerns.
4. **Cross-Cutting via AOP**: Use aspects for logging, security, transactions.

## 3. Mandatory Constraints for LLM

1. **No Mixed Concerns**: Business logic must not contain UI or persistence code.
2. **Layer Independence**: Each layer should be testable in isolation.
3. **DTOs at Boundaries**: Use DTOs to transfer data between layers.
4. **Cohesive Modules**: Group functionally related code together.

## 4. Structural Template (Java)

```java
// Mixed Concerns (Bad)
public class OrderController {
    public void createOrder(HttpRequest req) {
        var json = req.getBody();
        var order = new Order(json.get("product"), json.get("qty"));
        if (order.quantity() > 0) {
            jdbcTemplate.update("INSERT INTO orders...", order);
            sendEmail("Order created");
        }
    }
}

// Separated Concerns (Good)
// Controller - HTTP concern
public class OrderController {
    private final OrderService service;
    public OrderResponse createOrder(OrderRequest request) {
        return service.createOrder(request);
    }
}
// Service - Business logic concern
public class OrderService {
    private final OrderRepository repository;
    public OrderResponse createOrder(OrderRequest request) { ... }
}
// Repository - Data access concern
public class OrderRepository {
    public void save(Order order) { ... }
}
```
