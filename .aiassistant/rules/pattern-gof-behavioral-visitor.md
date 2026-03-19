---
apply: by model decision
instructions: Apply when adding new operations to object structures without modifying elements. Use for operations across heterogeneous class hierarchies via double dispatch.
---

# Visitor (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to define new operations on elements of an object structure without changing the classes of the elements.
- Many distinct and unrelated operations need to be performed on objects in an object structure.
- The classes defining the object structure rarely change, but you often want to define new operations over the structure.

### 1.2. Triggers
- Use when an object structure contains many classes with differing interfaces, and you want to perform operations that depend on their concrete classes.
- Use when you need to perform operations across a heterogeneous class hierarchy (e.g., Document with JSON, XML, YAML elements).
- Use when algorithms need to be kept separate from the objects on which they operate.

## 2. Implementation Guidelines

1. **Define Visitor Interface**: Declare a `visit` method for each type of concrete element in the object structure.
2. **Define Element Interface**: Declare an `accept(Visitor)` method.
3. **Implement Concrete Elements**: Each element implements `accept()` by calling `visitor.visit(this)` (double dispatch).
4. **Implement Concrete Visitors**: Each visitor implements all `visit` methods with specific operations for each element type.
5. **Object Structure**: A collection or composite that can enumerate its elements and allow a visitor to visit them.

## 3. SOLID & GRASP Compliance

- Open/Closed Principle (OCP): New operations (visitors) can be added without modifying existing element classes.
- Single Responsibility Principle (SRP): Each visitor encapsulates a single operation; elements focus on their data.
- High Cohesion: Algorithm logic is centralized in the visitor rather than scattered across element classes.
- Double Dispatch: Achieves runtime polymorphism based on both visitor and element types.

## 4. Mandatory Constraints for LLM

1. **Element Stability**: Visitor works best when the element hierarchy is stable. Adding new element types requires updating all visitors.
2. **Encapsulation Trade-off**: Visitors may need access to internal state of elements, potentially breaking encapsulation.
3. **Accept Method**: Each concrete element must implement `accept()` with `visitor.visit(this)` to enable double dispatch.

## 5. Structural Template (Java)

```java
// 1. Visitor Interface
public interface DocumentVisitor {
    void visit(JsonElement element);
    void visit(XmlElement element);
    void visit(YamlElement element);
}

// 2. Element Interface
public interface Element {
    void accept(DocumentVisitor visitor);
}

// 3. Concrete Elements
public class JsonElement implements Element {
    private final String content;
    
    public JsonElement(String content) { this.content = content; }
    public String getContent() { return content; }
    
    @Override
    public void accept(DocumentVisitor visitor) {
        visitor.visit(this); // Double dispatch
    }
}

public class XmlElement implements Element {
    private final String content;
    
    public XmlElement(String content) { this.content = content; }
    public String getContent() { return content; }
    
    @Override
    public void accept(DocumentVisitor visitor) {
        visitor.visit(this);
    }
}

// 4. Concrete Visitor
public class ExportVisitor implements DocumentVisitor {
    @Override
    public void visit(JsonElement element) {
        System.out.println("Exporting JSON: " + element.getContent());
    }
    
    @Override
    public void visit(XmlElement element) {
        System.out.println("Exporting XML: " + element.getContent());
    }
    
    @Override
    public void visit(YamlElement element) {
        System.out.println("Exporting YAML: " + element.getContent());
    }
}

// 5. Object Structure (Composite)
public class Document {
    private final List<Element> elements = new ArrayList<>();
    
    public void addElement(Element element) { elements.add(element); }
    
    public void accept(DocumentVisitor visitor) {
        for (Element element : elements) {
            element.accept(visitor);
        }
    }
}
```
