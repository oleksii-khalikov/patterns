---
apply: by model decision
instructions: Apply CoI (Composition Over Inheritance) to favor object composition over class inheritance. Use to achieve flexibility and avoid fragile base class problems.
---

# CoI (Composition Over Inheritance)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Deep inheritance hierarchies that are hard to understand.
- Base class changes break subclasses (fragile base class problem).
- Subclasses inherit unnecessary behavior.
- Need to share behavior across unrelated classes.

### 1.2. Triggers
- Use when inheritance is used only for code reuse (not true "is-a").
- Use when subclasses override most parent methods.
- Use when you need behavior from multiple sources (Java has no multiple inheritance).
- Use when the relationship is "has-a" or "uses-a" rather than "is-a."

## 2. Implementation Guidelines

1. **Favor "has-a"**: Compose objects that provide needed behavior.
2. **Delegate**: Forward calls to composed objects.
3. **Interface Inheritance**: Inherit interfaces (contracts), not implementations.
4. **Shallow Hierarchies**: Keep inheritance depth to 1-2 levels maximum.

## 3. Mandatory Constraints for LLM

1. **Test for LSP**: Only use inheritance when subtypes are truly substitutable.
2. **Prefer Interfaces**: Define contracts with interfaces, implement with composition.
3. **Inject Behavior**: Use constructor injection for composed behaviors.
4. **No Deep Hierarchies**: Avoid inheritance chains deeper than 2 levels.

## 4. Structural Template (Java)

```java
// Inheritance (Problematic)
public class TextBox extends Rectangle {
    // Inherits draw(), resize(), etc. - but is TextBox really a Rectangle?
}

// Composition (Better)
public class TextBox {
    private final Rectangle bounds;  // Has-a relationship
    private final TextRenderer renderer;
    
    public TextBox(Rectangle bounds, TextRenderer renderer) {
        this.bounds = bounds;
        this.renderer = renderer;
    }
    
    public void draw(Canvas canvas) {
        bounds.draw(canvas);
        renderer.render(canvas, bounds);
    }
}

// Strategy via Composition
public class PaymentProcessor {
    private final PaymentStrategy strategy;  // Composed behavior
    
    public PaymentProcessor(PaymentStrategy strategy) {
        this.strategy = strategy;
    }
    
    public Receipt process(Payment payment) {
        return strategy.execute(payment);
    }
}
```
