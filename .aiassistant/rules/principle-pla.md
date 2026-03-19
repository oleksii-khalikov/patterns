---
apply: by model decision
instructions: Apply PLA (Principle of Least Astonishment) to design APIs that behave as users expect. Use to reduce surprises, confusion, and bugs from unexpected behavior.Apply PLA (Principle of Least Astonishment) to design APIs that behave as users expect. Use to reduce surprises, confusion, and bugs from unexpected behavior.
---

# PLA (Principle of Least Astonishment)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Methods behave unexpectedly based on their names.
- Side effects are hidden or surprising.
- API behavior differs from common conventions.
- Users frequently misuse the API due to confusion.

### 1.2. Triggers
- Use when naming methods, classes, or parameters.
- Use when designing API contracts and return types.
- Use when handling edge cases and errors.
- Use when choosing default behaviors.

## 2. Implementation Guidelines

1. **Descriptive Names**: Names should accurately describe behavior.
2. **Follow Conventions**: Adhere to language and framework conventions.
3. **Predictable Behavior**: Similar methods should behave similarly.
4. **Explicit Over Implicit**: Make behavior obvious, not magical.
5. **Fail Fast**: Throw exceptions early rather than returning invalid data.

## 3. Mandatory Constraints for LLM

1. **No Hidden Side Effects**: Methods named `get*` or `find*` must not modify state.
2. **Standard Return Types**: `findById` returns `Optional`, collections return empty list (not null).
3. **Consistent Naming**: Use Java conventions (camelCase, verb-noun methods).
4. **Document Surprises**: If something is unusual, document it prominently.

## 4. Structural Template (Java)

```java
// Astonishing (Bad)
public class UserService {
    public User getUser(Long id) {
        auditLog.record("User accessed: " + id);  // Surprise side effect!
        return cache.computeIfAbsent(id, this::loadUser);  // Surprise caching!
    }
    
    public List<User> findUsers() {
        return users.isEmpty() ? null : users;  // Surprise null!
    }
}

// Least Astonishment (Good)
public class UserService {
    // Name clearly indicates side effect
    public User getUserAndLogAccess(Long id) {
        auditLog.record("User accessed: " + id);
        return loadUser(id);
    }
    
    // Pure query, no side effects
    public Optional<User> findById(Long id) {
        return Optional.ofNullable(cache.get(id));
    }
    
    // Returns empty list, never null
    public List<User> findAll() {
        return List.copyOf(users);
    }
}
```
