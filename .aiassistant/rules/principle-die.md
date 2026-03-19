---
apply: by model decision
instructions: Apply DIE (Duplication Is Evil) to aggressively eliminate all forms of duplication. Use alongside DRY to enforce single source of truth across code, docs, and config.
---

# DIE (Duplication Is Evil)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Code duplication leads to maintenance nightmares.
- Bug fixes must be applied in multiple places.
- Logical contradictions arise from inconsistent duplicate updates.
- Information or algorithms get lost in the noise of duplication.

### 1.2. Triggers
- **Imposed Duplication**: Developer is forced to duplicate due to knowledge gaps.
- **Inadvertent Duplication**: Developer doesn't realize they're duplicating.
- **Impatient Duplication**: Copy-paste to save time.
- **Inter-developer Duplication**: Multiple developers create similar code independently.

## 2. Implementation Guidelines

1. **Code Duplication**: Same logic in multiple methods/classes.
2. **Data Duplication**: Same data stored in multiple places.
3. **Documentation Duplication**: Comments that repeat what code says.
4. **Configuration Duplication**: Same settings in multiple config files.

## 3. Mandatory Constraints for LLM

1. **Regular Code Reviews**: Identify duplications during review process.
2. **Naming Conventions**: Consistent naming helps identify similar concepts.
3. **Refactor Immediately**: Don't leave duplication "for later."
4. **Extract Utilities**: Common operations belong in utility classes.

## 4. Structural Template (Java)

```java
// Duplication (Bad)
public class UserService {
    public boolean isValidEmail(String email) {
        return email != null && email.contains("@") && email.contains(".");
    }
}
public class ContactService {
    public boolean checkEmail(String email) {
        return email != null && email.contains("@") && email.contains(".");
    }
}

// Single Source (Good)
public final class EmailValidator {
    private static final Pattern EMAIL_PATTERN = Pattern.compile("^[\\w.-]+@[\\w.-]+\\.[a-zA-Z]{2,}$");
    
    public static boolean isValid(String email) {
        return email != null && EMAIL_PATTERN.matcher(email).matches();
    }
}
// Usage: EmailValidator.isValid(email);
```
