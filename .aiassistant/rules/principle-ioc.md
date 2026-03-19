---
apply: by model decision
instructions: Apply IoC (Inversion of Control) to decouple components by inverting dependency flow. Use with DI frameworks to let containers manage object creation and wiring.
---

# IoC (Inversion of Control) - Hollywood Principle

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Classes create their own dependencies using `new`.
- High coupling between classes and their concrete dependencies.
- Difficult to test classes in isolation.
- Hard to swap implementations without modifying code.

### 1.2. Triggers
- Use when classes instantiate their dependencies directly.
- Use when you need to swap implementations (e.g., for testing).
- Use when configuration should determine which implementations to use.
- Use when you want to centralize object creation and lifecycle management.

## 2. Implementation Guidelines

1. **Dependency Injection**: Pass dependencies through constructors.
2. **Program to Interfaces**: Depend on abstractions, not concretions.
3. **Container Management**: Let the IoC container wire dependencies.
4. **Configuration Over Code**: Define wiring in configuration, not in business code.

## 3. Mandatory Constraints for LLM

1. **No `new` for Services**: Never instantiate service dependencies directly.
2. **Constructor Injection Only**: No field injection, no setter injection.
3. **Interface Dependencies**: Depend on interfaces, not concrete classes.
4. **Immutable Dependencies**: Mark injected fields as `final`.

## 4. Structural Template (Java)

```java
// Without IoC (Bad) - Direct instantiation
public class OrderService {
    private final OrderRepository repository = new JdbcOrderRepository();
    private final EmailService emailService = new SmtpEmailService();
}

// With IoC (Good) - Constructor Injection
public class OrderService {
    private final OrderRepository repository;
    private final EmailService emailService;
    
    public OrderService(OrderRepository repository, EmailService emailService) {
        this.repository = Objects.requireNonNull(repository);
        this.emailService = Objects.requireNonNull(emailService);
    }
    
    public void createOrder(Order order) {
        repository.save(order);
        emailService.sendConfirmation(order);
    }
}

// Spring Configuration
@Configuration
public class AppConfig {
    @Bean
    public OrderService orderService(OrderRepository repo, EmailService email) {
        return new OrderService(repo, email);
    }
}
```
