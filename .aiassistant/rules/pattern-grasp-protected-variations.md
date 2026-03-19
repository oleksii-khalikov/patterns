---
apply: by model decision
instructions: Apply when isolating system elements from variations by creating stable interfaces around unstable points. Use for integrations, external APIs, and variable algorithms.
---

# Protected Variations (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- How to design objects and systems so that variations or instability in these elements do not have an undesirable impact on other elements?

### 1.2. Triggers
- Use when you identify a point of instability or variation in your system.
- Use to protect stable parts of the system from changing parts.

## 2. Implementation Guidelines

1. **Identify Points of Variation**: Find parts of the code that are likely to change.
2. **Wrap in an Interface**: Create a stable interface around the unstable part.
3. **Use Indirection**: Ensure clients use the stable interface.

## 3. SOLID & GRASP Compliance

- Open/Closed Principle (OCP): Protecting variations is the goal of OCP.
- Low Coupling: Isolating changes reduces coupling.
- Dependency Inversion Principle (DIP): Stable interfaces are at the heart of DIP.

## 4. Mandatory Constraints for LLM

1. **Over-engineering**: Don't protect against variations that are unlikely to happen.

## 5. Structural Template (Java)

```java
// Stable interface protects against variations in external systems
public interface ExternalService { void sync(); }

public class LegacyExternalService implements ExternalService { ... }
public class NewExternalService implements ExternalService { ... }
```
