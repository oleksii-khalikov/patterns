---
apply: by model decision
instructions: Apply when assigning responsibilities to the class that has the information needed to fulfill them. Use to create Rich Domain Models with encapsulated behavior.
---

# Information Expert (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- What is the most basic principle by which responsibilities are assigned to objects?
- Anemic domain models where logic is separated from data.
- Violation of encapsulation (TDA).

### 1.2. Triggers
- Assign a responsibility to the information expert — the class that has the information necessary to fulfill the responsibility.

## 2. Implementation Guidelines

1. **Identify Data Owners**: Find which class has the fields needed for a particular calculation or action.
2. **Place Logic in Data Owner**: Implement the behavior directly in that class.
3. **Tell, Don't Ask**: Instead of asking for data, tell the expert to perform the task.

## 3. SOLID & GRASP Compliance

- High Cohesion: Logic is kept with the data it uses.
- Encapsulation: Objects manage their own state.
- Rich Domain Model: Entities have both state and behavior.

## 4. Mandatory Constraints for LLM

1. **Avoid Bloated Entities**: If an operation requires information from many different experts, consider a Service or a specialized object.

## 5. Structural Template (Java)

```java
public class LineItem {
    private Product product;
    private int quantity;
    public BigDecimal getSubtotal() {
        return product.price().multiply(BigDecimal.valueOf(quantity));
    }
}

public class Order {
    private List<LineItem> items;
    public BigDecimal calculateTotal() {
        return items.stream()
            .map(LineItem::getSubtotal)
            .reduce(BigDecimal.ZERO, BigDecimal::add);
    }
}
```
