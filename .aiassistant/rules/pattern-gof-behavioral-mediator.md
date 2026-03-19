---
apply: by model decision
instructions: Apply when reducing chaotic dependencies between objects by centralizing communication. Use for decoupling many-to-many relationships between components.
---

# Mediator (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- A set of objects communicate in well-defined but complex ways.
- Reusing an object is difficult because it refers to and communicates with many other objects.
- Customizing behavior distributed among several classes should not require subclassing.

### 1.2. Triggers
- Use when a set of objects communicate in well-defined but complex ways, resulting in interdependencies that are unstructured and difficult to understand.
- Use when reusing an object is difficult because it refers to and communicates with many other objects.

## 2. Implementation Guidelines

1. **Define Mediator Interface**: Declares methods for communication between colleagues.
2. **Implement Concrete Mediator**: Coordinates communication between colleague objects.
3. **Colleague Classes**: Each colleague communicates with its mediator rather than with other colleagues.

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Centralizes complex communication logic.
- Low Coupling: Objects don't need to know about each other.
- Indirection (GRASP): Mediator provides indirection between colleagues.

## 4. Mandatory Constraints for LLM

1. **Avoid God Object**: The mediator can become overly complex. Consider splitting if it grows too large.
2. **One-Way Communication**: Colleagues know the mediator, but the mediator should use abstractions for colleagues if possible.

## 5. Structural Template (Java)

```java
public interface DialogMediator {
    void notify(Component sender, String event);
}

public class AuthenticationDialog implements DialogMediator {
    private Button loginButton;
    private TextField usernameField;

    @Override
    public void notify(Component sender, String event) {
        if (sender == usernameField && event.equals("textChanged")) {
            loginButton.setEnabled(!usernameField.getText().isEmpty());
        }
    }
}

public abstract class Component {
    protected DialogMediator mediator;
    public void setMediator(DialogMediator mediator) { this.mediator = mediator; }
}
```
