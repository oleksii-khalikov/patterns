# LLM Agent RuleSet: Composite (Structural Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Application model can be represented as a tree structure.
- Client code needs to treat individual objects and compositions of objects uniformly.
- Recursive structures (e.g., file systems, organization charts, graphic groups).
- Violation of OCP when adding new element types requires updating client code.

### 1.2. Triggers
- Use when you want to represent part-whole hierarchies of objects.
- Use when you want clients to be able to ignore the difference between compositions of objects and individual objects.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Component Interface**: Declare common operations for both simple and complex elements.
2. **Implement Leaf**: Represents end objects of the composition. A leaf has no children.
3. **Implement Composite**: Stores child components and implements child-related operations. Delegates work to children.
4. **Uniformity**: Ensure both Leaf and Composite implement the same Component interface.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Open/Closed Principle (OCP): New element types can be added without changing client code.
- Polymorphism: Clients use the Component interface to interact with all elements.
- High Cohesion: Logic for processing structures is encapsulated within the components.

## 4. Mandatory Constraints for LLM

1. **Type Safety vs Uniformity**: Decide whether to put child management methods (add/remove) in the Component interface or only in the Composite.
2. **Recursion**: Be careful with deeply nested structures (StackOverflowError).

## 5. Structural Template (Java)

```java
public interface Graphic {
    void draw();
}

public class Dot implements Graphic {
    @Override public void draw() { /* draw dot */ }
}

public class CompoundGraphic implements Graphic {
    private final List<Graphic> children = new ArrayList<>();
    public void add(Graphic g) { children.add(g); }
    @Override
    public void draw() {
        for (Graphic child : children) {
            child.draw();
        }
    }
}
```
