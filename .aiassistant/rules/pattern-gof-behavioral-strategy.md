---
apply: by model decision
instructions: Apply when implementing variable algorithms or behaviors that need to be interchangeable at runtime. Use for eliminating conditional logic based on type.
---

# Strategy (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Multiple algorithms or behaviors exist that differ only in implementation details.
- Conditional statements (if/else, switch) are used to select an algorithm at runtime.
- The client code is tightly coupled to specific algorithm implementations.

### 1.2. Triggers
- Use when many related classes differ only in their behavior.
- Use when you need different variants of an algorithm.
- Use when a class defines many behaviors that appear as multiple conditional statements.

## 2. Implementation Guidelines

1. **Define Strategy Interface**: Declare a common interface for all supported algorithms.
2. **Implement Concrete Strategies**: Create a class for each algorithm variant.
3. **Context Class**: Maintains a reference to a Strategy object and delegates work to it.
4. **Constructor Injection**: Pass the strategy to the context via the constructor.

## 3. SOLID & GRASP Compliance

- Open/Closed Principle (OCP): New strategies can be added without modifying existing code.
- Single Responsibility Principle (SRP): Each strategy encapsulates one algorithm.
- Dependency Inversion Principle (DIP): Context depends on the Strategy abstraction.

## 4. Mandatory Constraints for LLM

1. **No Field Injection**: Use exclusively Constructor Injection for the strategy.
2. **Immutable Strategies**: Prefer stateless strategies when possible.
3. **Naming**: Use descriptive names that reflect the algorithm (e.g., `QuickSortStrategy`, `MergeSortStrategy`).

## 5. Structural Template (Java)

```java
public interface PaymentStrategy {
    PaymentResult pay(BigDecimal amount);
}

public class CreditCardStrategy implements PaymentStrategy {
    @Override
    public PaymentResult pay(BigDecimal amount) {
        // Credit card payment logic
        return new PaymentResult("CC", amount);
    }
}

public class PaymentService {
    private final PaymentStrategy strategy;

    public PaymentService(PaymentStrategy strategy) {
        this.strategy = Objects.requireNonNull(strategy);
    }

    public PaymentResult processPayment(BigDecimal amount) {
        return strategy.pay(amount);
    }
}
```
