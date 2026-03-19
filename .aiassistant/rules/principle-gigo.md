---
apply: by model decision
instructions: Apply GIGO (Garbage In, Garbage Out) to validate all inputs early. Use to prevent invalid data from propagating through the system causing hard-to-debug issues.
---

# GIGO (Garbage In, Garbage Out)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Invalid data causes failures deep in the system.
- Errors are hard to trace back to their source.
- Null values propagate and cause NullPointerExceptions far from origin.
- Bad data corrupts database or downstream systems.

### 1.2. Triggers
- Use at system boundaries (API endpoints, file readers, message consumers).
- Use when accepting user input.
- Use when integrating with external systems.
- Use when data enters the domain layer.

## 2. Implementation Guidelines

1. **Validate Early**: Check inputs at the system boundary.
2. **Fail Fast**: Reject invalid data immediately with clear error messages.
3. **Sanitize Input**: Clean and normalize data before processing.
4. **Domain Validation**: Enforce business rules in domain objects.

## 3. Mandatory Constraints for LLM

1. **Boundary Validation**: All external input must be validated.
2. **Null Checks**: Use `Objects.requireNonNull()` or `Optional`.
3. **Clear Errors**: Validation errors must identify the problem clearly.
4. **Valid by Construction**: Domain objects should be valid upon creation.

## 4. Structural Template (Java)

```java
// No Validation (Bad) - Garbage propagates
public class OrderService {
    public void createOrder(OrderRequest request) {
        var order = new Order(request.customerId(), request.items());
        repository.save(order);  // Might save invalid data
    }
}

// Early Validation (Good) - Fail fast at boundary
public class OrderController {
    public OrderResponse createOrder(@Valid OrderRequest request) {
        return orderService.createOrder(request);
    }
}

public record OrderRequest(
    @NotNull @NotBlank String customerId,
    @NotEmpty List<@Valid ItemRequest> items
) {
    public OrderRequest {
        Objects.requireNonNull(customerId, "customerId is required");
        Objects.requireNonNull(items, "items is required");
        if (items.isEmpty()) {
            throw new IllegalArgumentException("Order must have at least one item");
        }
    }
}

// Domain object - valid by construction
public record Order(CustomerId customerId, List<LineItem> items) {
    public Order {
        Objects.requireNonNull(customerId);
        items = List.copyOf(Objects.requireNonNull(items));
        if (items.isEmpty()) {
            throw new DomainException("Order must contain items");
        }
    }
}
```
