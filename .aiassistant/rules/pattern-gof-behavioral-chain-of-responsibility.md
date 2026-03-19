---
apply: by model decision
instructions: Apply when multiple handlers can process a request and the handler isn't known beforehand. Use for decoupling request senders from receivers.
---

# Chain of Responsibility (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Multiple objects may handle a request, and the handler isn't known a priori.
- A request should be handled by one of several objects without specifying the receiver explicitly.
- The set of objects that can handle a request should be specified dynamically.

### 1.2. Triggers
- Use when more than one object may handle a request, and the handler should be ascertained automatically.
- Use when you want to issue a request to one of several objects without specifying the receiver explicitly.
- Use when the set of objects that can handle a request should be specified dynamically.

## 2. Implementation Guidelines

1. **Define Handler Interface**: Declare a method for handling requests and (optionally) setting the next handler.
2. **Implement Concrete Handlers**: Each handler decides whether to process the request or pass it along.
3. **Build the Chain**: Link handlers together, typically at configuration time.
4. **Client**: Sends the request to the first handler in the chain.

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Each handler focuses on one type of request.
- Open/Closed Principle (OCP): New handlers can be added without modifying existing ones.
- Low Coupling: Senders are decoupled from receivers.

## 4. Mandatory Constraints for LLM

1. **Guarantee Handling**: Ensure a request doesn't fall through the chain unhandled (consider a default handler).
2. **Chain Order**: Be mindful that the order of handlers in the chain matters.

## 5. Structural Template (Java)

```java
public interface Handler {
    void setNext(Handler next);
    void handle(Request request);
}

public abstract class BaseHandler implements Handler {
    private Handler next;
    @Override public void setNext(Handler next) { this.next = next; }
    protected void passToNext(Request request) {
        if (next != null) next.handle(request);
    }
}

public class AuthenticationHandler extends BaseHandler {
    @Override
    public void handle(Request request) {
        if (canHandle(request)) { /* process */ }
        else { passToNext(request); }
    }
}
```
