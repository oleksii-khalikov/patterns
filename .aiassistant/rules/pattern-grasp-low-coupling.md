---
apply: by model decision
instructions: Apply when reducing dependencies between components to minimize change impact. Use to increase reusability and make components more independent.
---

# Low Coupling (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- How to reduce the impact of changes?
- How to increase reusability?
- High dependency between classes makes the system fragile and hard to test.

### 1.2. Triggers
- Use to reduce dependencies between components.
- Use when you want to make components more independent and reusable.

## 2. Implementation Guidelines

1. **Prefer Interfaces**: Depend on abstractions rather than concrete classes.
2. **Use Dependency Injection**: Don't create dependencies inside a class; pass them in.
3. **Limit Visibility**: Keep as many classes and methods as possible private or package-private.
4. **Law of Demeter**: An object should only talk to its immediate neighbors.

## 3. SOLID & GRASP Compliance

- Maintainability: Low coupling makes the system easier to change.
- Testability: Decoupled classes are easier to unit test.
- Reusability: Classes with few dependencies are easier to reuse in other contexts.

## 4. Mandatory Constraints for LLM

1. **Don't take it to extremes**: Some coupling is necessary for objects to work together.

## 5. Structural Template (Java)

```java
// High Coupling (Bad)
public class Service {
    public void run() {
        new ConcreteWorker().doWork();
    }
}

// Low Coupling (Good)
public class Service {
    private final Worker worker;
    public Service(Worker worker) { this.worker = worker; }
    public void run() { worker.doWork(); }
}
```
