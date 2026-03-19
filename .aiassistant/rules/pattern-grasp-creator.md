---
apply: by model decision
instructions: Apply when determining which class should create instances of another class. Use to assign creation responsibility based on existing relationships and data ownership.
---

# Creator (GRASP Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Who should be responsible for creating a new instance of some class?
- Inappropriate assignment of creation responsibility leads to high coupling.

### 1.2. Triggers
- Assign class B the responsibility to create an instance of class A if one of the following is true:
- - B contains or aggregately contains A.
- - B records A.
- - B closely uses A.
- - B has the initializing data for A.

## 2. Implementation Guidelines

1. **Identify Container/Recorder**: Find classes that already have a close relationship with the object to be created.
2. **Add Create Method**: Implement a method in the Creator that instantiates and returns (or stores) the object.
3. **Pass Initializing Data**: The Creator should have or receive the data needed to initialize the new object.

## 3. SOLID & GRASP Compliance

- Low Coupling: By using existing relationships for creation, you avoid introducing new dependencies.
- High Cohesion: Creation responsibility is placed in a logically related class.

## 4. Mandatory Constraints for LLM

1. **Factory Pattern**: If the creation logic is very complex, consider using a Factory instead of the Creator pattern.

## 5. Structural Template (Java)

```java
public class Order {
    private final List<LineItem> items = new ArrayList<>();

    public void addItem(Product product, int quantity) {
        // Order is the Creator for LineItem
        var item = new LineItem(product, quantity);
        items.add(item);
    }
}
```
