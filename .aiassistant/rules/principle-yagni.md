---
apply: by model decision
instructions: Apply YAGNI (You Aren't Gonna Need It) to avoid implementing features until necessary. Use to prevent code bloat and speculative generality.
---

# YAGNI (You Aren't Gonna Need It)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Implementing features "just in case" they might be needed later.
- Adding configuration options or parameters that aren't currently required.
- Building abstractions for hypothetical future use cases.
- Code bloat from unused functionality.

### 1.2. Triggers
- Use when tempted to add functionality for anticipated future requirements.
- Use when creating abstractions without concrete current use cases.
- Use when adding parameters or options "for flexibility."
- Use when building frameworks instead of solving the immediate problem.

## 2. Implementation Guidelines

1. **Implement Only What's Needed**: Build for today's requirements, not tomorrow's guesses.
2. **Defer Decisions**: Delay adding complexity until you have concrete requirements.
3. **Delete Speculative Code**: Remove code that was added "just in case."
4. **Refactor When Needed**: Trust that you can refactor when new requirements emerge.

## 3. Mandatory Constraints for LLM

1. **No Speculative Features**: Only implement what is explicitly required.
2. **No Premature Abstraction**: Create interfaces/abstractions only when there are 2+ implementations.
3. **No Unused Parameters**: Don't add method parameters for potential future use.
4. **No Dead Code**: Remove any code that isn't actively used.

## 4. Structural Template (Java)

```java
// YAGNI Violation (Bad) - Speculative abstraction
public interface DataProcessor<T, R, E extends Exception> {
    R process(T input) throws E;
    default R processWithRetry(T input, int retries) { ... } // Not needed yet
    default CompletableFuture<R> processAsync(T input) { ... } // Not needed yet
}

// YAGNI Compliant (Good) - Only what's needed
public class OrderProcessor {
    public OrderResult process(Order order) {
        // Implementation for current requirements only
        return new OrderResult(order.id(), Status.PROCESSED);
    }
}
```
