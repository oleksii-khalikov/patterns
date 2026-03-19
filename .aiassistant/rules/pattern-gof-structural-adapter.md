---
apply: by model decision
instructions: Apply when integrating incompatible interfaces, especially with legacy code or third-party libraries. Use to make existing classes work with others without modifying source.
---

# Adapter (Structural Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- You need to use an existing class (Service), but its interface is incompatible with your client code.
- The Service is a 3rd-party library, legacy code, or otherwise unchangeable.
- Client code is becoming polluted with data conversion logic (e.g., XML to DTO), violating SRP.
- High coupling between client and a specific, incompatible service implementation.

### 1.2. Triggers
- Use when you want to use an existing class but its interface does not match the one you need.
- Use when you want to create a reusable class that cooperates with unrelated or unforeseen classes—that is, classes that don't necessarily have compatible interfaces.
- Use when you need to use several existing subclasses, but it's impractical to adapt their interface by subclassing every one.

## 2. Implementation Guidelines

1. **Identify Incompatible Components**: Identify the Service (incompatible) and the Client (needs functionality).
2. **Declare Client Interface (Target)**: Define the interface that your main business logic expects.
3. **Create Adapter Class**: Implement the Target interface in a new Adapter class.
4. **Compose Service**: Inject the Service into the Adapter via Constructor Injection.
5. **Implement Delegation**: In the Adapter's methods, convert data, call the Service, and convert the results back for the Client.
6. **Refactor Client**: Ensure the Client only interacts with the Target interface.

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Adapter isolates conversion logic from business logic.
- Open/Closed Principle (OCP): New adapters can be added without changing client code.
- Dependency Inversion Principle (DIP): Client depends on the Target abstraction, not the concrete Legacy Service.
- Indirection (GRASP): The Adapter provides a point of indirection between the client and the service.
- Pure Fabrication (GRASP): The Adapter is a class that does not represent a domain concept but is created to achieve low coupling.

## 4. Mandatory Constraints for LLM

1. **Composition over Inheritance**: Use the Object Adapter pattern (composition) rather than the Class Adapter pattern (inheritance).
2. **No Field Injection**: Use exclusively Constructor Injection for the Service being adapted.
3. **Encapsulation**: Keep conversion methods private within the Adapter.

## 5. Structural Template (Java)

```java
// 1. Target Interface
public interface PaymentGateway {
    PaymentResponse process(PaymentRequest request);
}

// 2. Service to be Adapted (Incompatible)
public class LegacyPaymentService {
    public String executeTransaction(String xml) { return "<res>OK</res>"; }
}

// 3. Adapter (Object Adapter)
public class PaymentAdapter implements PaymentGateway {
    private final LegacyPaymentService service;

    public PaymentAdapter(LegacyPaymentService service) {
        this.service = Objects.requireNonNull(service);
    }

    @Override
    public PaymentResponse process(PaymentRequest request) {
        var xml = convertToXml(request);
        var responseXml = service.executeTransaction(xml);
        return parseResponse(responseXml);
    }

    private String convertToXml(PaymentRequest request) { return ""; }
    private PaymentResponse parseResponse(String xml) { return new PaymentResponse(); }
}
```
