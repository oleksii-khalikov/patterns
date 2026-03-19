# LLM Agent RuleSet: Strategy (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Many related classes differ only in their behavior.
- Need to use different versions of an algorithm.
- An algorithm uses data that clients shouldn't know about.
- A class defines many behaviors, which appear as multiple conditional statements in its operations.

### 1.2. Triggers
- Use when many related classes differ only in their behavior.
- Use when you need different variants of an algorithm.
- Use to avoid exposing complex, algorithm-specific data structures.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Strategy Interface**: Common for all supported algorithms.
2. **Implement Concrete Strategies**: Implement the algorithm using the Strategy interface.
3. **Define Context Class**: Configured with a Concrete Strategy object and maintains a reference to a Strategy object.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Open/Closed Principle (OCP): Introduce new strategies without changing the context.
- Dependency Inversion Principle (DIP): Context depends on Strategy abstraction.
- High Cohesion: Each strategy encapsulates a single algorithm.

## 4. Mandatory Constraints for LLM

1. **Client must know about strategies**: The client usually chooses which strategy to use.
2. **Overhead**: Communication between Context and Strategy can sometimes add overhead.

## 5. Structural Template (Java)

```java
public interface Strategy {
    int execute(int a, int b);
}

public class ConcreteStrategyAdd implements Strategy {
    @Override public int execute(int a, int b) { return a + b; }
}

public class Context {
    private Strategy strategy;
    public void setStrategy(Strategy strategy) { this.strategy = strategy; }
    public int executeStrategy(int a, int b) { return strategy.execute(a, b); }
}
```
