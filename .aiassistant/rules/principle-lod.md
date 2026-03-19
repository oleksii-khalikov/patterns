---
apply: by model decision
instructions: Apply LoD (Law of Demeter) to reduce coupling by limiting object interactions. Use when objects access deeply nested properties or "talk to strangers."
---

# LoD (Law of Demeter) - "Don't Talk to Strangers"

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Method chains like `a.getB().getC().doSomething()` (train wrecks).
- Objects know too much about the internal structure of other objects.
- Changes to internal structure ripple through the codebase.
- High coupling between unrelated classes.

### 1.2. Triggers
- Use when you see method chains longer than one dot.
- Use when a method accesses objects through other objects' getters.
- Use when tests require complex mock setups due to deep dependencies.
- Use when changes to one class break many unrelated classes.

## 2. Implementation Guidelines

A method M of object O may only call methods of:
1. **O itself** - the object's own methods.
2. **M's parameters** - objects passed as arguments.
3. **Objects created within M** - locally instantiated objects.
4. **O's direct components** - fields of the object.

## 3. Mandatory Constraints for LLM

1. **No Train Wrecks**: Avoid `a.b().c().d()` chains.
2. **Tell, Don't Ask**: Command objects to do work rather than querying their state.
3. **Encapsulate Traversal**: If navigation is needed, encapsulate it in a method.
4. **Facade for Complex Structures**: Provide simplified interfaces to complex object graphs.

## 4. Structural Template (Java)

```java
// LoD Violation (Bad) - Train wreck
public class OrderProcessor {
    public void process(Order order) {
        var discount = order.getCustomer().getAccount().getDiscountRate();
        var address = order.getCustomer().getAddress().getStreet();
    }
}

// LoD Compliant (Good) - Tell, don't ask
public class Order {
    private final Customer customer;
    
    public BigDecimal getApplicableDiscount() {
        return customer.getDiscountRate(); // Customer handles the details
    }
    
    public String getDeliveryStreet() {
        return customer.getDeliveryStreet();
    }
}

public class OrderProcessor {
    public void process(Order order) {
        var discount = order.getApplicableDiscount();
        var address = order.getDeliveryStreet();
    }
}
```
