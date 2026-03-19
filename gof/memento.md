# LLM Agent RuleSet: Memento (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to capture and externalize an object's internal state so that the object can be restored to this state later.
- Violation of encapsulation when trying to save state from outside the object.

### 1.2. Triggers
- Use when a snapshot of an object's state must be saved so that it can be restored to that state later.
- Use when a direct interface to obtaining the state would expose implementation details and break encapsulation.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Originator**: The class whose state needs to be saved. It creates Mementos and uses them to restore its state.
2. **Define Memento**: A value object that stores the state of the Originator. It should be immutable.
3. **Define Caretaker**: Responsible for keeping track of mementos. It never modifies or looks inside a memento.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Originator is responsible for its state; Caretaker is responsible for saving history.
- Information Expert: Only the Originator knows how to save and restore its internal state.

## 4. Mandatory Constraints for LLM

1. **Encapsulation**: Memento should provide a 'wide' interface to the Originator and a 'narrow' interface to the Caretaker.
2. **Memory usage**: Saving many snapshots can consume a lot of memory.

## 5. Structural Template (Java)

```java
public record EditorMemento(String text, int cursorX, int cursorY) {}

public class Editor {
    private String text;
    private int cursorX, cursorY;
    public EditorMemento save() { return new EditorMemento(text, cursorX, cursorY); }
    public void restore(EditorMemento m) {
        this.text = m.text();
        this.cursorX = m.cursorX();
        this.cursorY = m.cursorY();
    }
}
```
