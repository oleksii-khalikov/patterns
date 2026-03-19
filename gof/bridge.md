# LLM Agent RuleSet: Bridge (Structural Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Exponential growth of class hierarchy due to extension in several orthogonal dimensions.
- Violation of SRP: Each class takes responsibility for multiple independent axes of change.
- Violation of OCP: Adding a new variant in one dimension requires changes across the entire hierarchy.

### 1.2. Triggers
- Use when you want to divide and organize a monolithic class that has several variants of some functionality.
- Use when you need to extend a class in several orthogonal (independent) dimensions.
- Use when you need to be able to switch implementations at runtime.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Identify Orthogonal Dimensions**: Separate high-level control (Abstraction) from low-level execution (Implementation).
2. **Define Implementation Interface**: Create a common interface for all concrete implementations.
3. **Create Refined Abstractions**: Extend the base abstraction for specific high-level variants.
4. **Implement Concrete Implementations**: Create classes that implement the Implementation interface.
5. **Bridge them**: The Abstraction must contain a reference to the Implementation, injected via constructor.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Each hierarchy focuses on only one dimension of change.
- Open/Closed Principle (OCP): New abstractions and implementations can be added independently.
- Dependency Inversion Principle (DIP): Abstraction depends on Implementation interface, not concrete classes.
- Indirection (GRASP): The bridge reference provides indirection between abstraction and implementation.

## 4. Mandatory Constraints for LLM

1. **No Tight Coupling**: Abstraction must not know concrete implementation classes.
2. **Constructor Injection**: The implementation must be injected into the abstraction.

## 5. Structural Template (Java)

```java
// 1. Implementation Interface
public interface DrawingApi {
    void drawCircle(double x, double y, double radius);
}

// 2. Abstraction
public abstract class Shape {
    protected final DrawingApi drawingApi;

    protected Shape(DrawingApi drawingApi) {
        this.drawingApi = Objects.requireNonNull(drawingApi);
    }

    public abstract void draw();
}

// 3. Refined Abstraction
public class Circle extends Shape {
    private final double x, y, radius;

    public Circle(double x, double y, double radius, DrawingApi drawingApi) {
        super(drawingApi);
        this.x = x; this.y = y; this.radius = radius;
    }

    @Override
    public void draw() {
        drawingApi.drawCircle(x, y, radius);
    }
}
```
