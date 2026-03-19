# LLM Agent RuleSet: Chain of Responsibility (Behavioral Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- A request needs to pass through a series of sequential checks or processings.
- The exact sequence or specific handlers are unknown in advance or change dynamically.
- Massive if/else or switch blocks for handling different request types.
- Tight coupling between sender and all potential receivers.

### 1.2. Triggers
- Use when more than one object may handle a request, and the handler isn't known a priori.
- Use when you want to issue a request to one of several objects without specifying the receiver explicitly.
- Use when the set of objects that can handle a request should be specified dynamically.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Handler Interface**: Declare a method for handling requests and optionally for setting the next handler.
2. **Create Base Handler**: Implement the boilerplate logic for linking and passing requests.
3. **Implement Concrete Handlers**: Implement specific logic. Decide whether to handle the request or pass it on.
4. **Compose the Chain**: Link handler objects together in a sequence.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Each handler focuses on a single task.
- Open/Closed Principle (OCP): New handlers can be added without modifying existing code.
- Low Coupling: Sender doesn't know who will eventually handle the request.

## 4. Mandatory Constraints for LLM

1. **Termination**: Ensure the chain either handles the request or has a clear 'end of chain' behavior.
2. **Order Dependency**: Be aware that the order of handlers in the chain can be significant.

## 5. Structural Template (Java)

```java
public interface Handler {
    void setNext(Handler next);
    void handle(Request request);
}

public abstract class BaseHandler implements Handler {
    private Handler next;
    @Override public void setNext(Handler next) { this.next = next; }
    protected void handleNext(Request request) {
        if (next != null) next.handle(request);
    }
}

public class AuthHandler extends BaseHandler {
    @Override
    public void handle(Request request) {
        if (request.isAuthenticated()) {
            handleNext(request);
        } else {
            throw new UnauthorizedException();
        }
    }
}
```
