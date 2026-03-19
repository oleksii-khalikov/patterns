# LLM Agent RuleSet: Builder (Creational Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Complex objects requiring step-by-step initialization of many fields.
- Telescopic constructors (monstrous constructors with many parameters).
- Creation logic is mixed with main business logic (SRP violation).
- Client code becomes unreadable due to passing many nulls or default values.

### 1.2. Triggers
- Use to avoid 'telescopic constructors'.
- Use when you want your code to be able to create different representations of some product.
- Use when you need to construct complex objects where some parts are optional.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Builder Interface**: Declare all possible product construction steps.
2. **Implement Concrete Builders**: Implement the steps and provide a way to retrieve the final product.
3. **Define Product**: The complex object being built.
4. **Optional Director**: Create a class that defines the order in which to call construction steps.
5. **Fluent Interface**: Return 'this' from builder methods to allow chaining.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Isolate complex construction logic from the product's business logic.
- High Cohesion: Group related construction steps in the builder.
- Pure Fabrication: The Director and Builder are often pure fabrications to manage complexity.

## 4. Mandatory Constraints for LLM

1. **Immutability**: Consider making the final Product immutable (use Records if possible).
2. **Validation**: Validate the product's state in the build() method.

## 5. Structural Template (Java)

```java
public record Car(String engine, int seats, boolean gps) {}

public interface CarBuilder {
    CarBuilder reset();
    CarBuilder setEngine(String engine);
    CarBuilder setSeats(int seats);
    CarBuilder setGps(boolean gps);
    Car build();
}

public class SportsCarBuilder implements CarBuilder {
    private String engine;
    private int seats;
    private boolean gps;

    @Override public CarBuilder reset() { return this; }
    @Override public CarBuilder setEngine(String engine) { this.engine = engine; return this; }
    @Override public CarBuilder setSeats(int seats) { this.seats = seats; return this; }
    @Override public CarBuilder setGps(boolean gps) { this.gps = gps; return this; }

    @Override
    public Car build() {
        return new Car(engine, seats, gps);
    }
}
```
