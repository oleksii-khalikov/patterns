# LLM Agent RuleSet: Visitor (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- An object structure contains many classes of objects with differing interfaces, and you want to perform operations on these objects that depend on their concrete classes.
- Need to perform many unrelated operations on objects in an object structure.
- Adding new operations requires modifying existing classes (OCP violation).

### 1.2. Triggers
- Use when an object structure contains many classes of objects with differing interfaces, and you want to perform operations on these objects that depend on their concrete classes.
- Use when many distinct and unrelated operations need to be performed on objects in an object structure, and you want to avoid 'polluting' their classes with these operations.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Visitor Interface**: Declares a visit() method for each class of Concrete Element in the object structure.
2. **Implement Concrete Visitors**: Implement each operation declared by the Visitor interface.
3. **Define Element Interface**: Declares an accept() method that takes a visitor as an argument.
4. **Implement Concrete Elements**: Implement accept() by calling the corresponding visit() method on the visitor (Double Dispatch).

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Move unrelated operations into visitor classes.
- Open/Closed Principle (OCP): Add new operations by adding new visitor classes without changing elements.
- Pure Fabrication: Visitors are often pure fabrications.

## 4. Mandatory Constraints for LLM

1. **Stable Element Hierarchy**: Visitor is difficult to use if the element hierarchy changes frequently.
2. **Broken Encapsulation**: Visitors often require elements to provide public access to their internal state.

## 5. Structural Template (Java)

```java
public interface Visitor {
    void visitDot(Dot dot);
    void visitCircle(Circle circle);
}

public interface Shape {
    void accept(Visitor v);
}

public class Dot implements Shape {
    @Override public void accept(Visitor v) { v.visitDot(this); }
}

public class XMLExportVisitor implements Visitor {
    @Override public void visitDot(Dot dot) { /* export dot to XML */ }
    @Override public void visitCircle(Circle c) { /* export circle to XML */ }
}
```
