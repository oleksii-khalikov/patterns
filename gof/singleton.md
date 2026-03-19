# LLM Agent RuleSet: Singleton (Creational Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- A class must have exactly one instance, and it must be accessible to clients from a well-known access point.
- Need to control access to a shared resource (DB connection, config).

### 1.2. Triggers
- Use when there must be exactly one instance of a class, and it must be accessible to clients from a well-known access point.
- Use when the sole instance should be extensible by subclassing.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Private Constructor**: Prevent direct instantiation from outside.
2. **Static Instance Field**: Store the single instance.
3. **Public Static Getter**: Provide a global access point and implement lazy initialization.
4. **Thread Safety**: Use Double-Checked Locking or other mechanisms in multithreaded environments.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- High Cohesion: Singleton manages its own lifecycle.
- Pure Fabrication: Singleton is often used as a pure fabrication to manage global state.

## 4. Mandatory Constraints for LLM

1. **Testing Difficulty**: Singletons can make unit testing hard. Consider using Dependency Injection instead of direct global access.
2. **Violation of SRP**: Singleton often takes on two responsibilities (uniqueness and business logic).

## 5. Structural Template (Java)

```java
public final class Database {
    private static volatile Database instance;
    private Database() {}
    public static Database getInstance() {
        if (instance == null) {
            synchronized (Database.class) {
                if (instance == null) instance = new Database();
            }
        }
        return instance;
    }
}
```
