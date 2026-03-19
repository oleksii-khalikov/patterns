---
apply: by model decision
instructions: Apply KISS (Keep It Simple, Stupid) when code becomes overly complex. Use to favor straightforward solutions over clever or over-engineered approaches.
---

# KISS (Keep It Simple, Stupid)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Code is difficult to understand or explain to others.
- Simple tasks require complex implementations.
- Over-engineering with unnecessary abstractions or patterns.
- "Clever" code that sacrifices readability for brevity.

### 1.2. Triggers
- Use when a simpler solution exists but was overlooked.
- Use when code requires extensive comments to explain what it does.
- Use when debugging takes longer than expected due to complexity.
- Use when new team members struggle to understand the codebase.

## 2. Implementation Guidelines

1. **Prefer Clarity**: Write code that is easy to read and understand.
2. **Avoid Premature Abstraction**: Don't create abstractions until they're needed.
3. **Use Standard Solutions**: Prefer well-known patterns over custom inventions.
4. **Minimize Moving Parts**: Reduce the number of components, layers, and indirections.
5. **Delete Dead Code**: Remove unused code, commented-out blocks, and obsolete features.

## 3. Mandatory Constraints for LLM

1. **No Over-Engineering**: Don't add patterns or abstractions "just in case."
2. **Readable Names**: Use clear, descriptive names for variables, methods, and classes.
3. **Short Methods**: Keep methods focused and under 20-30 lines when possible.
4. **Flat Hierarchies**: Avoid deep inheritance; prefer composition.

## 4. Structural Template (Java)

```java
// Complex (Bad)
public String processData(String input) {
    return Optional.ofNullable(input)
        .map(String::trim)
        .filter(s -> !s.isEmpty())
        .map(s -> s.substring(0, 1).toUpperCase() + s.substring(1))
        .orElseGet(() -> "default");
}

// Simple (Good)
public String processData(String input) {
    if (input == null || input.isBlank()) {
        return "default";
    }
    var trimmed = input.trim();
    return Character.toUpperCase(trimmed.charAt(0)) + trimmed.substring(1);
}
```
