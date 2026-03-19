---
apply: by model decision
instructions: Apply SLAP (Single Level of Abstraction Principle) to keep methods at one abstraction level. Use to improve readability by not mixing high-level and low-level code.
---

# SLAP (Single Level of Abstraction Principle)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Methods mix high-level operations with low-level details.
- Code is hard to read because abstraction levels jump around.
- Business logic is intertwined with implementation details.
- Methods are long and hard to understand at a glance.

### 1.2. Triggers
- Use when a method contains both business concepts and technical details.
- Use when reading a method requires mental context-switching.
- Use when some lines describe "what" while others describe "how."
- Use when extracting methods would improve readability.

## 2. Implementation Guidelines

1. **Consistent Abstraction**: All statements in a method should be at the same level.
2. **Extract Lower Levels**: Move lower-level details into separate methods.
3. **Stepdown Rule**: Code should read like a top-down narrative.
4. **Name by Intent**: Method names should describe what, not how.

## 3. Mandatory Constraints for LLM

1. **No Mixed Levels**: High-level steps shouldn't include low-level details.
2. **Readable Flow**: Method should read like a summary of steps.
3. **Extract Aggressively**: Create helper methods for lower-level operations.
4. **Descriptive Names**: Helper method names explain their purpose.

## 4. Structural Template (Java)

```java
// Mixed Abstraction Levels (Bad)
public void processOrder(Order order) {
    // High level
    if (!order.isValid()) throw new ValidationException("Invalid order");
    
    // Low level details mixed in
    var conn = dataSource.getConnection();
    var stmt = conn.prepareStatement("INSERT INTO orders...");
    stmt.setString(1, order.id());
    stmt.executeUpdate();
    
    // High level again
    notifyCustomer(order);
    
    // Low level again
    var props = new Properties();
    props.put("mail.smtp.host", smtpHost);
    var session = Session.getInstance(props);
    // ...
}

// Single Level of Abstraction (Good)
public void processOrder(Order order) {
    validateOrder(order);
    saveOrder(order);
    notifyCustomer(order);
    updateInventory(order);
}

private void validateOrder(Order order) {
    if (!order.isValid()) {
        throw new ValidationException("Invalid order");
    }
}

private void saveOrder(Order order) {
    orderRepository.save(order);
}

private void notifyCustomer(Order order) {
    emailService.sendOrderConfirmation(order);
}

private void updateInventory(Order order) {
    inventoryService.reserve(order.items());
}
```
