---
apply: by file patterns
patterns: *.java
---

# Java Coding Standards

## 1. Java Language Features & Null Safety

- **Strictly avoid `null`**: Use `Optional<T>` or empty collections. Return empty products or throw domain exceptions if creation fails.
- Favor `Stream` API for simple transformations, but use loops for complex logic.
- Prefer simple `if/else` over complex ternary chains.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Switch Expressions** for multi-branch logic.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages or simple product attributes.
- Use `var` for local variables where readability is preserved.
- Use **Sealed Classes** to restrict inheritance hierarchies.

## 2. Naming Conventions

- **Classes/Interfaces**: PascalCase with nouns (`OrderService`, `PaymentGateway`).
- **Methods**: camelCase with verbs (`calculateTotal`, `findById`).
- **Variables**: camelCase with meaningful names (`customerName`, not `cn`).
- **Constants**: UPPER_SNAKE_CASE (`MAX_RETRY_COUNT`, `DEFAULT_TIMEOUT`).
- **Packages**: lowercase, reversed domain (`com.company.project.module`).
- **Booleans**: Use `is`, `has`, `can`, `should` prefixes (`isValid`, `hasAccess`).
- **Collections**: Use plural nouns (`orders`, `items`, `userNames`).

## 3. Exception Handling

- Use **specific exceptions** instead of generic `Exception` or `RuntimeException`.
- Create **domain-specific exceptions** for business errors (`OrderNotFoundException`).
- **Never swallow exceptions** silently; always log or rethrow.
- Use **try-with-resources** for all `AutoCloseable` resources.
- Avoid using exceptions for control flow.
- Include context in exception messages (`"Order not found: " + orderId`).
- Prefer **unchecked exceptions** for programming errors, checked for recoverable conditions.

## 4. Immutability

- Make classes **immutable by default**; use `final` fields.
- Return **defensive copies** of mutable collections: `List.copyOf(items)`.
- Use `Collections.unmodifiableList()` or `List.of()` for immutable collections.
- Avoid setters; prefer builder pattern or constructor initialization.

## 5. Collections Best Practices

- Declare variables using **interface types** (`List`, `Map`, `Set`), not implementations.
- Use `List.of()`, `Set.of()`, `Map.of()` for small immutable collections.
- Prefer `EnumSet` and `EnumMap` for enum-keyed collections.
- Use `computeIfAbsent()` instead of check-then-put for maps.
- Avoid modifying collections while iterating; use `Iterator.remove()` or `removeIf()`.

## 6. Concurrency

- Mark shared mutable state with `volatile` or use `AtomicXxx` classes.
- Prefer `java.util.concurrent` utilities over manual synchronization.
- Use `ConcurrentHashMap` instead of `Collections.synchronizedMap()`.
- Use `ExecutorService` for thread management; never create raw threads.
- Avoid `synchronized` on `this`; use private lock objects.
- Make objects **thread-safe** or **thread-confined**; document the choice.

## 7. Logging

- Use **SLF4J** with parameterized messages: `log.info("Processing order: {}", orderId)`.
- Use appropriate log levels: ERROR (failures), WARN (issues), INFO (milestones), DEBUG (diagnostics).
- **Never log sensitive data** (passwords, tokens, PII).
- Include **correlation IDs** for traceability in distributed systems.
- Avoid string concatenation in log statements; use placeholders.

## 8. Method Design

- Keep methods **short and focused** (ideally under 20 lines).
- Limit parameters to **3-4 maximum**; use parameter objects for more.
- Use **guard clauses** for early returns instead of nested conditionals.
- Return early for error conditions; keep happy path un-indented.
- Avoid **boolean parameters**; use enums or separate methods.

## 9. Class Design

- **One public class per file**; file name must match class name.
- Order members: static fields, instance fields, constructors, methods.
- Prefer **composition over inheritance**.
- Mark classes `final` if not designed for extension.
- Use **enums** instead of integer constants or string literals.

## 10. Resource Management

- **Always use try-with-resources** for streams, connections, readers.
- Close resources in reverse order of acquisition.
- Use `finally` blocks only when try-with-resources is not applicable.
- Pool expensive resources (database connections, thread pools).

## 11. Code Formatting

- **Indentation**: 4 spaces (no tabs).
- **Line length**: Maximum 120 characters.
- **Braces**: Opening brace on the same line as statement.
- **Blank lines**: Separate logical sections; one between methods.
- **Imports**: No wildcard imports; organize and remove unused.

## 12. Object Methods Contract

- **Always override `equals()` and `hashCode()` together**; if objects are equal, hash codes must be equal.
- Implement `toString()` for meaningful debugging output; include key identifying fields.
- Use `Objects.equals()` and `Objects.hash()` for null-safe implementations.
- For `Comparable`: ensure consistency with `equals()`; `compareTo() == 0` should imply `equals() == true`.
- Use `record` when possible—it generates `equals()`, `hashCode()`, and `toString()` automatically.

## 13. Annotations

- **Always use `@Override`** when overriding methods or implementing interface methods.
- Use `@Deprecated(since="version", forRemoval=true)` with Javadoc explaining alternatives.
- Use `@SuppressWarnings` narrowly with a comment justifying suppression.
- Prefer `@Nullable`/`@NonNull` to document null contracts.
- Never use `@SuppressWarnings("all")`; be specific about what you're suppressing.

## 14. Javadoc & Documentation

- **Document all public APIs**: classes, interfaces, public methods, and constructors.
- Start Javadoc with a summary fragment (noun/verb phrase, not "This method...").
- Use `@param`, `@return`, `@throws` in that order; never leave them empty.
- Document thread-safety guarantees and side effects.
- Self-explanatory getters/setters may omit Javadoc; complex logic requires it.

## 15. String Handling

- Use `StringBuilder` for concatenation in loops; avoid `+` operator repeatedly.
- Prefer `String.format()` or `MessageFormat` for complex string building.
- Use `String.isBlank()` over `trim().isEmpty()` for whitespace checks.
- Compare strings with `equals()`, never `==`.
- Use `equalsIgnoreCase()` for case-insensitive comparison.

## 16. Numeric Types & Precision

- **Use `BigDecimal` for monetary/financial calculations**; never `float`/`double`.
- Create `BigDecimal` from `String`, not `double`: `new BigDecimal("0.1")`.
- Use `compareTo()` for `BigDecimal` comparison, not `equals()` (scale matters).
- Prefer `int`/`long` primitives over wrappers unless nullability is required.
- Be explicit about integer overflow; use `Math.addExact()` for checked arithmetic.

## 17. Static Members

- **Access static members via class name**, not instance: `ClassName.staticMethod()`.
- Avoid mutable static fields; prefer constants or dependency injection.
- Use static factory methods (`of()`, `from()`, `valueOf()`) over constructors for clarity.
- Static imports: use sparingly; acceptable for test assertions and common utilities.

## 18. API Design

- Return **empty collections** instead of `null`; use `Collections.emptyList()` or `List.of()`.
- Prefer **method overloading** over optional parameters with `null` defaults.
- Use **fluent APIs** (return `this`) for builder-style configuration.
- Make defensive copies of mutable parameters in constructors.
- Fail fast: validate parameters at method entry with clear error messages.

## 19. Date & Time (java.time)

- **Use `java.time` API** (`LocalDate`, `LocalDateTime`, `Instant`, `ZonedDateTime`); never legacy `Date`/`Calendar`.
- Use `Instant` for timestamps and machine time; `LocalDateTime` for human-readable without timezone.
- Use `ZonedDateTime` when timezone matters; store as UTC, convert for display.
- **Inject `Clock`** for testability instead of calling `Instant.now()` directly.
- Use `Duration` for time-based amounts; `Period` for date-based amounts.
- Parse/format with `DateTimeFormatter`; prefer ISO formats for APIs.

## 20. Generics

- **Prefer generic types** over raw types; never use raw `List`, always `List<String>`.
- Use **bounded wildcards**: `<? extends T>` for producers, `<? super T>` for consumers (PECS).
- Avoid generic arrays; use `List<T>` instead of `T[]`.
- Use `@SafeVarargs` on final/static methods with generic varargs.
- Prefer `List<?>` over `List<Object>` for unknown element types.

## 21. Lambdas & Functional Interfaces

- Keep lambdas **short** (1-3 lines); extract to method reference or named method if longer.
- Prefer **method references** when clearer: `list.forEach(System.out::println)`.
- Use standard functional interfaces (`Function`, `Predicate`, `Consumer`, `Supplier`) over custom ones.
- Avoid side effects in lambdas used with streams; keep them pure.
- Use `Optional.map()`/`flatMap()` instead of `if (optional.isPresent())` checks.

## 22. Security

- **Never log sensitive data**: passwords, tokens, credit cards, PII.
- **Validate all external input**: sanitize user input, API parameters, file paths.
- Use **parameterized queries** for SQL; never concatenate user input into queries.
- Store passwords with **bcrypt/scrypt/argon2**; never plain text or simple hashing.
- Use `SecureRandom` for cryptographic randomness, not `Random`.
- Avoid deserializing untrusted data; use allowlists if unavoidable.