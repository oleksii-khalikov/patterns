# LLM Agent RuleSet: State (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- An object's behavior depends on its state, and it must change its behavior at run-time depending on that state.
- Operations have large, multipart conditional statements that depend on the object's state.

### 1.2. Triggers
- Use when an object's behavior depends on its state, and it must change its behavior at run-time depending on that state.
- Use when operations have large, multipart conditional statements that depend on the object's state.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define State Interface**: Declares methods for state-specific behaviors.
2. **Implement Concrete States**: Each class implements behavior associated with a specific state of the Context.
3. **Define Context Class**: Maintains an instance of a Concrete State subclass that defines the current state.
4. **State Transition**: Transitions can be handled either in the Context or in the Concrete State classes.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Organize logic related to particular states into separate classes.
- Open/Closed Principle (OCP): Introduce new states without changing existing state classes or the context.
- Polymorphism: Behavior changes based on the concrete state object.

## 4. Mandatory Constraints for LLM

1. **State sharing**: If states have no instance variables, they can be shared (Flyweight).
2. **Complex transitions**: Managing many transitions between states can become complicated.

## 5. Structural Template (Java)

```java
public abstract class State {
    protected AudioPlayer player;
    public State(AudioPlayer player) { this.player = player; }
    public abstract void clickLock();
    public abstract void clickPlay();
}

public class LockedState extends State {
    public LockedState(AudioPlayer player) { super(player); }
    @Override public void clickLock() { player.changeState(new ReadyState(player)); }
    @Override public void clickPlay() { /* do nothing */ }
}
```
