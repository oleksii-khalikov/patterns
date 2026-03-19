---
apply: by model decision
instructions: Apply Robustness Principle (Postel's Law) to be conservative in output and liberal in input. Use for APIs and integrations to maximize interoperability.
---

# Robustness Principle (Postel's Law)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- APIs that break when receiving slightly malformed input.
- Strict validation that rejects valid data due to minor formatting issues.
- Integration failures due to whitespace or case sensitivity.
- Systems that can't evolve because clients depend on exact output format.

### 1.2. Triggers
- Use when designing APIs that will be consumed by external systems.
- Use when integrating with third-party services.
- Use when building systems that need to be forward-compatible.
- Use when handling user input or external data.

## 2. Implementation Guidelines

1. **Conservative Output**: Produce strict, well-formed, documented output.
2. **Liberal Input**: Accept variations in input format when meaning is clear.
3. **Tolerant Reader**: Ignore unknown fields in received data.
4. **Graceful Degradation**: Handle edge cases without crashing.

## 3. Mandatory Constraints for LLM

1. **Trim Inputs**: Remove leading/trailing whitespace from strings.
2. **Normalize Data**: Convert to canonical form (e.g., lowercase emails).
3. **Ignore Unknown**: Don't fail on unexpected fields in JSON/XML.
4. **Strict Output**: Always produce valid, consistent output format.

## 4. Structural Template (Java)

```java
// Brittle (Bad) - Strict input validation
public User parseUser(Map<String, String> data) {
    if (!data.containsKey("email")) throw new ValidationException("Missing email");
    return new User(data.get("email"));  // Fails on "EMAIL" or " email"
}

// Robust (Good) - Liberal input, conservative output
public User parseUser(Map<String, String> data) {
    var email = findValue(data, "email")
        .map(String::trim)
        .map(String::toLowerCase)
        .filter(this::isValidEmail)
        .orElseThrow(() -> new ValidationException("Valid email required"));
    
    return new User(email);  // Always lowercase, trimmed
}

private Optional<String> findValue(Map<String, String> data, String key) {
    return data.entrySet().stream()
        .filter(e -> e.getKey().equalsIgnoreCase(key))
        .map(Map.Entry::getValue)
        .filter(v -> v != null && !v.isBlank())
        .findFirst();
}

// Jackson configuration for tolerant reading
@JsonIgnoreProperties(ignoreUnknown = true)
public record ApiResponse(String status, String message) {}
```
