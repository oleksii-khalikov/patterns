---
apply: by model decision
instructions: Apply when assigning responsibilities to artificial classes that don't represent domain concepts. Use for services, repositories, and utilities that support low coupling.
---

# Pure Fabrication (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- What object should have a responsibility, when you do not want to violate High Cohesion and Low Coupling but the solution offered by Information Expert is not appropriate?
- Need to create utility or service classes that don't correspond to domain concepts.

### 1.2. Triggers
- Use when assigning a responsibility to an Information Expert would violate cohesion or coupling.
- Use when you need to create a helper class that doesn't represent a real-world concept.

## 2. Implementation Guidelines

1. **Identify Misfit Responsibilities**: Find responsibilities that don't fit well in existing domain classes.
2. **Create Artificial Class**: Create a new class whose sole purpose is to fulfill these responsibilities.
3. **Focus on Service**: Pure fabrications are often services, factories, or repositories.

## 3. SOLID & GRASP Compliance

- High Cohesion: The fabricated class has a focused set of related responsibilities.
- Low Coupling: Domain classes remain decoupled from infrastructure concerns.
- Single Responsibility Principle: Each fabrication focuses on one concern.

## 4. Mandatory Constraints for LLM

1. **Don't Overuse**: Pure fabrications should support, not replace, a rich domain model.
2. **Naming**: Name fabrications by their responsibility (e.g., `OrderRepository`, `EmailService`).

## 5. Structural Template (Java)

```java
// Pure Fabrication: Repository (doesn't represent a domain concept)
public interface OrderRepository {
    Optional<Order> findById(UUID id);
    void save(Order order);
}

// Pure Fabrication: Service (coordinates work)
public class NotificationService {
    private final EmailSender emailSender;
    
    public NotificationService(EmailSender emailSender) {
        this.emailSender = emailSender;
    }
    
    public void notifyOrderShipped(Order order) {
        var message = createShippingMessage(order);
        emailSender.send(message);
    }
}
```
