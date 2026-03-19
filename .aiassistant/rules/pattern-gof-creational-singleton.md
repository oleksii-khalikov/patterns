---
apply: by model decision
instructions: Apply when exactly one instance of a class is needed with a global access point. Use sparingly as it can introduce hidden dependencies and testing issues.
---

# Singleton (Creational Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to ensure a class has only one instance.
- Need to provide a global point of access to that instance.
- Coordinating actions across a system where only one instance should exist.

### 1.2. Triggers
- Use when there must be exactly one instance of a class, and it must be accessible from a well-known access point.
- Use when the sole instance should be extensible by subclassing, and clients should be able to use an extended instance without modifying their code.

## 2. Implementation Guidelines

1. **Private Constructor**: Prevent direct instantiation.
2. **Static Instance**: Hold the single instance in a static field.
3. **Public Accessor**: Provide a static method that returns the instance.
4. **Thread Safety**: Ensure thread-safe initialization (e.g., static holder idiom, enum).

## 3. SOLID & GRASP Compliance

- Pure Fabrication: Often used for services that don't represent domain concepts.
- SRP: Single responsibility for instance management.

## 4. Mandatory Constraints for LLM

1. **Avoid Overuse**: Singletons can introduce hidden dependencies and make testing difficult.
2. **Thread Safety**: Ensure the implementation is thread-safe.
3. **Prefer DI**: In modern applications, prefer dependency injection over Singleton.

## 5. Structural Template (Java)

```java
// Preferred: Enum Singleton (thread-safe, serialization-safe)
public enum ConfigurationManager {
    INSTANCE;
    
    private final Map<String, String> properties = new HashMap<>();
    
    public String getProperty(String key) { return properties.get(key); }
    public void setProperty(String key, String value) { properties.put(key, value); }
}

// Alternative: Static Holder Idiom (lazy, thread-safe)
public class Logger {
    private Logger() {}
    
    private static class Holder {
        private static final Logger INSTANCE = new Logger();
    }
    
    public static Logger getInstance() { return Holder.INSTANCE; }
}
```
