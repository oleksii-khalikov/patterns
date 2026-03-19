---
apply: by model decision
instructions: Apply DRY (Don't Repeat Yourself) to eliminate code duplication. Use to ensure every piece of knowledge has a single, authoritative representation.
---

# DRY (Don't Repeat Yourself)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Same logic appears in multiple places in the codebase.
- Bug fixes need to be applied in multiple locations.
- Changes to business rules require updates across many files.
- Copy-paste programming is prevalent.

### 1.2. Triggers
- Use when you see the same code block repeated 2+ times.
- Use when the same business rule is implemented in multiple places.
- Use when constants or magic values are duplicated.
- Use when similar data structures exist without a common abstraction.

## 2. Implementation Guidelines

1. **Extract Common Logic**: Move repeated code into shared methods or classes.
2. **Use Constants**: Define magic numbers and strings as named constants.
3. **Create Abstractions**: Build common base classes or interfaces for shared behavior.
4. **Centralize Configuration**: Keep configuration in one place (properties, constants class).
5. **Apply Rule of Three**: Consider extracting after seeing duplication 3 times.

## 3. Mandatory Constraints for LLM

1. **Single Source of Truth**: Every piece of knowledge must exist in exactly one place.
2. **Extract Helper Methods**: Common operations should be in utility or helper methods.
3. **Centralize Constants**: No magic numbers or strings scattered in code.
4. **Don't Over-DRY**: Don't force unification of code that only looks similar but has different reasons to change.

## 4. Structural Template (Java)

```java
// DRY Violation (Bad)
public class OrderService {
    public void validateOrder(Order o) {
        if (o.total().compareTo(BigDecimal.ZERO) <= 0) throw new ValidationException("Invalid total");
    }
}
public class InvoiceService {
    public void validateInvoice(Invoice i) {
        if (i.amount().compareTo(BigDecimal.ZERO) <= 0) throw new ValidationException("Invalid amount");
    }
}

// DRY Compliant (Good)
public class MoneyValidator {
    public static void requirePositive(BigDecimal amount, String fieldName) {
        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new ValidationException("Invalid " + fieldName);
        }
    }
}
// Usage: MoneyValidator.requirePositive(order.total(), "total");
```
