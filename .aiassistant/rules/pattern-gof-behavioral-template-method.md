---
apply: by model decision
instructions: Apply when defining an algorithm's skeleton in a base class, letting subclasses override specific steps. Use to avoid code duplication in similar
---

# Template Method (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to implement the invariant parts of an algorithm once and leave it up to subclasses to implement the parts that can vary.
- Code duplication across subclasses that implement similar algorithms.

### 1.2. Triggers
- Use to implement the invariant parts of an algorithm once and leave it up to subclasses to implement the behavior that can vary.
- Use when common behavior among subclasses should be factored and localized in a common class to avoid code duplication.

## 2. Implementation Guidelines

1. **Define Abstract Class**: Implements the template method defining the algorithm's skeleton.
2. **Template Method**: Calls primitive operations (abstract or default). It should usually be final.
3. **Implement Concrete Classes**: Override primitive operations to provide specific behavior.

## 3. SOLID & GRASP Compliance

- Hollywood Principle: 'Don't call us, we'll call you.' Base class calls operations of subclasses.
- OCP: Subclasses can extend algorithm steps without changing its structure.

## 4. Mandatory Constraints for LLM

1. **LSP Compliance**: Ensure subclasses don't break the invariants of the algorithm.
2. **Number of steps**: Too many abstract steps can make implementation difficult for subclasses.

## 5. Structural Template (Java)

```java
public abstract class DataMiner {
    public final void mine(String path) {
        openFile(path);
        extractData();
        parseData();
        closeFile();
    }
    protected abstract void extractData();
    protected abstract void parseData();
    protected void openFile(String path) { /* default */ }
    protected void closeFile() { /* default */ }
}
```
