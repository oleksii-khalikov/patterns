---
apply: by model decision
instructions: Apply Curly's Law (Do One Thing) to ensure each code unit has a single purpose. Use for methods, classes, and modules to maximize focus and clarity.
---

# Curly's Law (Do One Thing)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Methods that do multiple unrelated things.
- Classes with too many responsibilities.
- Variables that change meaning during their lifetime.
- Functions that are hard to name because they do too much.

### 1.2. Triggers
- Use when a method name requires "and" to describe it.
- Use when a variable is reused for different purposes.
- Use when you can't easily write a single test for a method.
- Use when a class has multiple reasons to change.

## 2. Implementation Guidelines

1. **Single Purpose Methods**: Each method does exactly one thing.
2. **Single Purpose Variables**: Each variable represents one concept.
3. **Single Level of Abstraction**: Don't mix high-level and low-level code.
4. **Cohesive Classes**: All methods relate to a single responsibility.

## 3. Mandatory Constraints for LLM

1. **One Thing Per Method**: If a method does two things, split it.
2. **Descriptive Names**: If you can't name it simply, it does too much.
3. **No Variable Reuse**: Don't repurpose variables for different meanings.
4. **Short Methods**: Methods should typically be 5-15 lines.

## 4. Structural Template (Java)

```java
// Violates Curly's Law (Bad) - Does multiple things
public void processOrder(Order order) {
    // Validates
    if (order.items().isEmpty()) throw new ValidationException("No items");
    // Calculates
    var total = order.items().stream().mapToDouble(Item::price).sum();
    // Persists
    repository.save(order);
    // Notifies
    emailService.send(order.customerEmail(), "Order confirmed");
}

// Follows Curly's Law (Good) - Each method does one thing
public void processOrder(Order order) {
    validate(order);
    var total = calculateTotal(order);
    save(order);
    notifyCustomer(order);
}

private void validate(Order order) {
    if (order.items().isEmpty()) throw new ValidationException("No items");
}

private BigDecimal calculateTotal(Order order) {
    return order.items().stream()
        .map(Item::price)
        .reduce(BigDecimal.ZERO, BigDecimal::add);
}

private void save(Order order) {
    repository.save(order);
}

private void notifyCustomer(Order order) {
    emailService.send(order.customerEmail(), "Order confirmed");
}
```
