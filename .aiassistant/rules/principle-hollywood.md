---
apply: by model decision
instructions: Apply Hollywood Principle (Don't call us, we'll call you) to invert control flow. Use for frameworks, event systems, and callback-based architectures.
---

# Hollywood Principle ("Don't Call Us, We'll Call You")

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Low-level components call high-level components directly.
- Tight coupling between framework and application code.
- Difficulty extending or customizing framework behavior.
- Circular dependencies between modules.

### 1.2. Triggers
- Use when designing frameworks or libraries.
- Use when implementing event-driven architectures.
- Use when applying Template Method or Strategy patterns.
- Use when implementing plugin or extension systems.

## 2. Implementation Guidelines

1. **Framework Calls You**: High-level code defines when to call low-level code.
2. **Callbacks and Hooks**: Provide extension points, not direct calls.
3. **Event-Driven**: Use events/listeners instead of direct method calls.
4. **Dependency Injection**: Framework injects dependencies, not the reverse.

## 3. Mandatory Constraints for LLM

1. **Inverted Control**: Application code registers with framework, not vice versa.
2. **Abstract Hooks**: Define abstract methods or interfaces for extension.
3. **Event Emission**: Emit events; let subscribers decide how to handle.
4. **Lifecycle Management**: Framework controls object lifecycle.

## 4. Structural Template (Java)

```java
// Without Hollywood Principle (Bad) - Application calls framework
public class MyApp {
    public void run() {
        var framework = new Framework();
        framework.init();
        framework.loadConfig();
        framework.start();  // Application controls framework
        doMyStuff();
        framework.stop();
    }
}

// With Hollywood Principle (Good) - Framework calls application
public abstract class Application {
    // Template Method - framework controls flow
    public final void run() {
        init();
        configure();    // Hook for subclass
        start();
        onStarted();    // Hook for subclass
        waitForShutdown();
        onShutdown();   // Hook for subclass
    }
    
    protected abstract void configure();
    protected void onStarted() {} // Optional hook
    protected void onShutdown() {} // Optional hook
}

public class MyApp extends Application {
    @Override
    protected void configure() {
        // Framework calls this when it's ready
        registerService(new MyService());
    }
    
    @Override
    protected void onStarted() {
        logger.info("Application started");
    }
}

// Event-based Hollywood Principle
public interface OrderEventListener {
    void onOrderCreated(OrderCreatedEvent event);
}

public class OrderService {
    private final List<OrderEventListener> listeners = new ArrayList<>();
    
    public void addListener(OrderEventListener listener) {
        listeners.add(listener);  // Don't call us...
    }
    
    public void createOrder(Order order) {
        repository.save(order);
        // ...we'll call you
        listeners.forEach(l -> l.onOrderCreated(new OrderCreatedEvent(order)));
    }
}
```
