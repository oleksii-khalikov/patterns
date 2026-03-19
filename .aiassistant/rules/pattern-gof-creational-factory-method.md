---
apply: by model decision
instructions: Apply when a class cannot anticipate the type of objects it must create. Use for delegating instantiation to subclasses while maintaining interface consistency.
---

# Factory Method (Creational Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- A class cannot anticipate the class of objects it must create.
- A class wants its subclasses to specify the objects it creates.
- Logic for creating objects is mixed with business logic (SRP violation).
- Violation of OCP: Adding new product types requires changing existing code.

### 1.2. Triggers
- Use when a class can't anticipate the class of objects it must create.
- Use when a class wants its subclasses to specify the objects it creates.
- Use when you want to provide users of your library or framework with a way to extend its internal components.

## 2. Implementation Guidelines

1. **Define Product Interface**: Common interface for all objects the factory creates.
2. **Implement Concrete Products**: Specific implementations of the Product interface.
3. **Define Creator Class**: Declares the factory method. It can also provide a default implementation.
4. **Implement Concrete Creators**: Override the factory method to return a specific Concrete Product.

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Move product creation code into one place.
- Open/Closed Principle (OCP): Introduce new product types without breaking existing client code.
- Dependency Inversion Principle (DIP): Client depends on Product interface.

## 4. Mandatory Constraints for LLM

1. **Naming**: Use descriptive names for factory methods (e.g., createTransport, createButton).
2. **Parameterization**: Factory methods can be parameterized to create multiple types of products.

## 5. Structural Template (Java)

```java
public interface Transport { void deliver(); }
public class Truck implements Transport { @Override public void deliver() { /* road */ } }
public class Ship implements Transport { @Override public void deliver() { /* sea */ } }

public abstract class Logistics {
    public void planDelivery() {
        Transport t = createTransport();
        t.deliver();
    }
    public abstract Transport createTransport();
}

public class RoadLogistics extends Logistics {
    @Override public Transport createTransport() { return new Truck(); }
}
```
