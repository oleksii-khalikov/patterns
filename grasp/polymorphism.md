# LLM Agent RuleSet: Polymorphism (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- How to handle related but varying behaviors based on type?
- Massive switch or if/else blocks based on object type.

### 1.2. Triggers
- Use when behavior varies by type.
- Use to replace conditional logic with polymorphic method calls.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Common Interface**: Create an interface for the varying behavior.
2. **Implement Concrete Variants**: Create a class for each behavior variant.
3. **Invoke Polymorphically**: Call the method on the interface, allowing the runtime to select the correct implementation.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Open/Closed Principle (OCP): New variants can be added without modifying existing code.
- Indirection: Polymorphism via interfaces provides indirection.

## 4. Mandatory Constraints for LLM

1. **Inheritance depth**: Prefer shallow hierarchies.

## 5. Structural Template (Java)

```java
public interface TaxCalculator { BigDecimal calculate(Order o); }
public class VATCalculator implements TaxCalculator { ... }
public class USATaxCalculator implements TaxCalculator { ... }

public class Order {
    public void applyTax(TaxCalculator calculator) {
        this.tax = calculator.calculate(this);
    }
}
```
