---
apply: by model decision
instructions: Apply OAOO (Once And Only Once) to ensure each behavior is implemented exactly once. Use as the refactoring goal for eliminating all forms of duplication.
---

# OAOO (Once And Only Once)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Same behavior implemented in multiple places.
- Similar code structures repeated throughout the codebase.
- Business rules scattered across multiple classes.
- Copy-paste programming leads to inconsistencies.

### 1.2. Triggers
- Use when refactoring duplicated code.
- Use when extracting common patterns.
- Use when centralizing business rules.
- Use as the goal metric for DRY compliance.

## 2. Implementation Guidelines

1. **Single Implementation**: Each behavior exists in exactly one place.
2. **Extract Mercilessly**: When you see duplication, extract immediately.
3. **Centralize Rules**: Business rules belong in domain objects or dedicated rule classes.
4. **Template Methods**: Use for algorithms with varying steps.

## 3. Mandatory Constraints for LLM

1. **No Copy-Paste**: Never duplicate code; extract and reuse.
2. **Single Source of Truth**: Constants, rules, and configurations in one place.
3. **Refactor Immediately**: Don't leave duplication for later cleanup.
4. **Parameterize Differences**: Use parameters to handle variations.

## 4. Structural Template (Java)

```java
// OAOO Violation (Bad) - Same calculation in multiple places
public class InvoiceService {
    public BigDecimal calculateTotal(Invoice inv) {
        return inv.subtotal().add(inv.subtotal().multiply(TAX_RATE));
    }
}
public class QuoteService {
    public BigDecimal calculateTotal(Quote q) {
        return q.subtotal().add(q.subtotal().multiply(TAX_RATE));
    }
}

// OAOO Compliant (Good) - Single implementation
public final class TaxCalculator {
    private static final BigDecimal TAX_RATE = new BigDecimal("0.20");
    
    public static BigDecimal withTax(BigDecimal subtotal) {
        return subtotal.add(subtotal.multiply(TAX_RATE));
    }
}

// Or using an interface for polymorphism
public interface Taxable {
    BigDecimal subtotal();
    
    default BigDecimal totalWithTax() {
        return TaxCalculator.withTax(subtotal());
    }
}
```
