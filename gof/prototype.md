# LLM Agent RuleSet: Prototype (Creational Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to create a copy of an object without exposing its internal structure.
- Creation of objects via 'new' is expensive or complex.
- Need to avoid a hierarchy of factories that parallels the hierarchy of products.

### 1.2. Triggers
- Use when the classes to instantiate are specified at run-time.
- Use to avoid building a class hierarchy of factories that parallels the class hierarchy of products.
- Use when instances of a class can have one of only a few different combinations of state.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Prototype Interface**: Declare a clone() method.
2. **Implement Concrete Prototypes**: Implement clone() by creating a new object and copying fields.
3. **Prototype Constructor**: A constructor that takes an instance of the same class to copy its state.
4. **Client**: Creates a new object by asking a prototype to clone itself.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Information Expert: The object itself is the expert on how to clone its state.
- Low Coupling: Client doesn't need to know the concrete class of the object it clones.

## 4. Mandatory Constraints for LLM

1. **Deep vs Shallow Copy**: Be careful when cloning objects that contain references to other objects.
2. **Circular References**: Handling circular references during cloning can be tricky.

## 5. Structural Template (Java)

```java
public abstract class Shape {
    public int x, y;
    public String color;
    public Shape() {}
    public Shape(Shape source) {
        this.x = source.x; this.y = source.y; this.color = source.color;
    }
    public abstract Shape clone();
}

public class Circle extends Shape {
    public int radius;
    public Circle() {}
    public Circle(Circle source) { super(source); this.radius = source.radius; }
    @Override public Shape clone() { return new Circle(this); }
}
```
