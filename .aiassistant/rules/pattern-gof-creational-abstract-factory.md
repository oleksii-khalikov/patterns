---
apply: by model decision
instructions: Apply when creating families of related objects that must be used together. Use to ensure product compatibility and isolate client from concrete classes.
---

# Abstract Factory (Creational Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- The system needs to create families of **related or interdependent products**.
- You must ensure that products within one family are always compatible (e.g., all "Modern" style furniture).
- Client code is tightly coupled to concrete classes via the `new` operator.
- Adding new product variants requires significant changes to existing code (OCP violation).

### 1.2. Triggers
- Use when there are multiple families of products.
- Use when the system should be independent of how its products are created, composed, and represented.
- Use when you want to enforce a constraint that products from the same family must be used together.

## 2. Implementation Guidelines

1.  **Identify Families and Products**: Define the product types (e.g., `Chair`, `Sofa`) and their variants (e.g., `Modern`, `Victorian`).
2.  **Define Abstract Products**: Create interfaces for each product type. Use **Records** for simple data-carrying products.
3.  **Define Abstract Factory**: Create an interface with methods for creating each abstract product (e.g., `createChair()`, `createSofa()`).
4.  **Implement Concrete Factories**: Create a factory class for each variant. Each factory must return compatible concrete products.
5.  **Refactor Client Code**: Inject the Abstract Factory interface into the client via **Constructor Injection**. The client must only interact with abstract products.

## 3. SOLID & GRASP Compliance

- **Dependency Inversion Principle (DIP)**: Client depends on `FurnitureFactory` interface, not `ModernFurnitureFactory`.
- **Open/Closed Principle (OCP)**: New variants (e.g., `CyberpunkFactory`) can be added without modifying the `FurnitureOrderService`.
- **Single Responsibility Principle (SRP)**: Factory classes encapsulate creation logic, while services focus on business logic.
- **High Cohesion**: Creation logic is centralized in factories.

## 4. Mandatory Constraints for LLM

1.  **No Field Injection**: Use exclusively Constructor Injection for the factory.
2.  **Compatibility Guarantee**: Ensure that a concrete factory *only* creates products from its family.
3.  **Runtime Configuration**: The concrete factory should be selected at the application entry point (Composition Root) based on configuration.

## 5. Structural Template (Java)

```java
// 1. Abstract Products
public interface Chair { String sitOn(); }
public interface Sofa { String lieOn(); }

// 2. Abstract Factory
public interface FurnitureFactory {
    Chair createChair();
    Sofa createSofa();
}

// 3. Concrete Factory
public class ModernFurnitureFactory implements FurnitureFactory {
    @Override public Chair createChair() { return new ModernChair(); }
    @Override public Sofa createSofa() { return new ModernSofa(); }
}

// 4. Client (DIP)
public class FurnitureOrderService {
    private final FurnitureFactory factory;

    public FurnitureOrderService(FurnitureFactory factory) {
        this.factory = Objects.requireNonNull(factory);
    }

    public void processOrder() {
        var chair = factory.createChair();
        var sofa = factory.createSofa();
        // ... business logic
    }
}
```
