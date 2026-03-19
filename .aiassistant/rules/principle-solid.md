---
apply: by model decision
instructions: Apply SOLID principles for maintainable OOP design. Use to ensure classes are focused, extensible, substitutable, interface-segregated, and depend on abstractions.
---

# SOLID Principles

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Classes are becoming too large with multiple responsibilities.
- Modifications to one part of the system require changes in unrelated parts.
- Subclasses cannot be substituted for their base classes without breaking behavior.
- Clients are forced to depend on interfaces they don't use.
- High-level modules depend on low-level implementation details.

### 1.2. Triggers
- **S (SRP)**: A class has more than one reason to change.
- **O (OCP)**: Adding new functionality requires modifying existing code.
- **L (LSP)**: Subtypes cannot be used interchangeably with their base types.
- **I (ISP)**: Clients implement interface methods they don't need.
- **D (DIP)**: High-level modules instantiate low-level modules directly.

## 2. Implementation Guidelines

### 2.1. The Five Principles
1. **Single Responsibility Principle (SRP)**: A class should have only one reason to change.
2. **Open/Closed Principle (OCP)**: Software entities should be open for extension, closed for modification.
3. **Liskov Substitution Principle (LSP)**: Subtypes must be substitutable for their base types.
4. **Interface Segregation Principle (ISP)**: Clients should not be forced to depend on interfaces they don't use.
5. **Dependency Inversion Principle (DIP)**: Depend on abstractions, not concretions.

## 3. Mandatory Constraints for LLM

1. **SRP**: Each class must have a single, well-defined responsibility.
2. **OCP**: Use polymorphism (Strategy, Template Method) instead of conditionals.
3. **LSP**: Never strengthen preconditions or weaken postconditions in subclasses.
4. **ISP**: Prefer many small interfaces over one large interface.
5. **DIP**: Always inject dependencies via constructor; never use `new` for services.

## 4. Structural Template (Java)

```java
// SRP: One responsibility per class
public class OrderValidator { boolean validate(Order o) { ... } }
public class OrderRepository { void save(Order o) { ... } }

// OCP: Open for extension via interface
public interface DiscountStrategy { BigDecimal apply(Order o); }

// LSP: Subtypes are substitutable
public class PremiumCustomer extends Customer { /* honors base contract */ }

// ISP: Small, focused interfaces
public interface Readable { String read(); }
public interface Writable { void write(String data); }

// DIP: Depend on abstractions
public class OrderService {
    private final OrderRepository repository; // interface, not impl
    public OrderService(OrderRepository repository) { this.repository = repository; }
}
```
