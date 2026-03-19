---
apply: by model decision
instructions: Apply when supporting large numbers of fine-grained objects efficiently by sharing common state. Use to reduce memory consumption in object-heavy applications.
---

# Flyweight (Structural Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Application needs to create a huge number of objects that consume too much RAM.
- Objects contain a lot of duplicated (intrinsic) data.
- Memory exhaustion on low-resource systems.

### 1.2. Triggers
- Use when an application uses a large number of objects.
- Use when storage costs are high because of the quantity of objects.
- Use when most object state can be made extrinsic.

## 2. Implementation Guidelines

1. **Separate State**: Identify Intrinsic (shared, constant) and Extrinsic (unique, variable) state.
2. **Define Flyweight**: A class that stores the intrinsic state. It must be immutable.
3. **Create Flyweight Factory**: Manages a pool of Flyweight objects and ensures they are shared.
4. **Define Context**: Stores the extrinsic state and a reference to a Flyweight object.

## 3. SOLID & GRASP Compliance

- Pure Fabrication: Flyweight Factory is a pure fabrication for resource management.
- Immutability: Ensuring intrinsic state cannot be changed is critical for safe sharing.
- High Cohesion: Group related intrinsic state in the flyweight.

## 4. Mandatory Constraints for LLM

1. **Extrinsic State Management**: The client or a context object must provide extrinsic state to the flyweight.
2. **Thread Safety**: The flyweight factory and the flyweights themselves must be thread-safe if shared.

## 5. Structural Template (Java)

```java
public record TreeType(String name, String color, String texture) {}

public class TreeFlyweight {
    private final TreeType type;
    public TreeFlyweight(TreeType type) { this.type = type; }
    public void draw(Canvas canvas, int x, int y) { /* draw using type at x,y */ }
}

public class TreeFactory {
    private static final Map<TreeType, TreeFlyweight> treeTypes = new HashMap<>();
    public static TreeFlyweight getTreeType(String name, String color, String texture) {
        var type = new TreeType(name, color, texture);
        return treeTypes.computeIfAbsent(type, k -> new TreeFlyweight(k));
    }
}
```
