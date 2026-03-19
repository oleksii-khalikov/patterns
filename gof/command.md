# LLM Agent RuleSet: Command (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to separate the object that initiates an operation from the object that performs it.
- Need to support undo/redo operations.
- Need to queue operations or execute them at different times.
- High coupling between UI and business logic.

### 1.2. Triggers
- Use when you want to parameterize objects by an action to perform.
- Use when you need to specify, queue, and execute requests at different times.
- Use when you need to support undo.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Command Interface**: Declare an execute() method.
2. **Implement Concrete Commands**: Store parameters and a reference to the Receiver. Delegate work to the Receiver in execute().
3. **Define Invoker**: The class that triggers the command.
4. **Define Receiver**: The class that contains the actual business logic.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Command class encapsulates a single request.
- Open/Closed Principle (OCP): New commands can be added without changing existing code.
- Indirection (GRASP): Command object provides indirection between invoker and receiver.

## 4. Mandatory Constraints for LLM

1. **Encapsulation**: Concrete commands should contain all information needed for execution.
2. **Undo State**: If supporting undo, commands must store enough state to revert the operation.

## 5. Structural Template (Java)

```java
public interface Command {
    void execute();
    void undo();
}

public class SaveCommand implements Command {
    private final Editor editor;
    private String backup;

    public SaveCommand(Editor editor) { this.editor = editor; }

    @Override
    public void execute() {
        backup = editor.getText();
        editor.save();
    }

    @Override
    public void undo() {
        editor.setText(backup);
    }
}
```
