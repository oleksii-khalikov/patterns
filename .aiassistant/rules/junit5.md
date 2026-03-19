---
apply: by file patterns
patterns: src/test/java/**/*.java
---

## Scope and Tooling
- **JUnit version**: Use JUnit 5 (Jupiter).
- **Assertions**: Prefer AssertJ for fluent, descriptive assertions; JUnit assertions are acceptable.
- **Mocking**: Use Mockito to isolate dependencies. Avoid mocking value objects.
- **Location**: Tests live under `src/test/java`, mirroring the package structure of `src/main/java`.
- **Project rule**: Exclude the `examples/` folder from any testing rules and checks.

## Naming and Structure
- **Class names**: `ClassNameTest` or `ClassNameShould`.
- **Method names**: Use `should...` / `shouldNot...` or `given_when_then` style.
- **Arrange-Act-Assert (AAA)**: Separate blocks with blank lines and/or `// Arrange`, `// Act`, `// Assert` comments.
- **One behavior per test**: Keep tests focused. Prefer one assertion per test; if multiple assertions are required, ensure they validate a single behavior (use AssertJ soft assertions if needed).

## Core Principles
- **Isolation**: Tests must be independent, with no side-effects across tests or modules. Do not rely on execution order.
- **Determinism**: No randomness, time-of-day, thread scheduling, or environment dependence. Inject a `Clock` or provide deterministic inputs.
- **No conditional logic or loops** in test methods. If needed, use parameterized tests.
- **No external I/O**: Do not touch network, filesystem, DB, message brokers. Mock boundary interfaces.
- **Public behavior only**: Test via public contracts. Test private logic indirectly via public methods.
- **No magic numbers**: Use named constants/variables for clarity.
- **Meaningful failures**: Use assertion methods that yield clear messages; add a custom failure message when helpful.
- **Prefer state-based testing**. Use interaction verification only where behavior truly depends on collaborator calls.
- **No printing/logging** from tests. Use assertions instead.

## Assertions
- **Expected vs actual clarity**: Prefer variable names like `expectedX` and `actualX`.
- **Specific assertion methods**: Avoid generic `assertTrue/false` when a more specific assertion exists (e.g., `assertThat(result).contains(...)`).
- **Numeric comparisons**: Avoid unnecessary precision. Specify tolerances only when relevant.
- **Exceptions**: Use `assertThrows` (or AssertJ `assertThatThrownBy`) to verify exception type and message.

## Mocking and Test Doubles
- **Mockito setup**: Use `@ExtendWith(MockitoExtension.class)` and constructor injection for the SUT.
- **Stubbing**: Prefer `when(x.method()).thenReturn(...)`. Avoid spying unless necessary; do not stub the class under test.
- **Verification**: Verify only meaningful interactions with precise `times(1)` etc. Avoid over-specifying collaborator calls.
- **Boundaries only**: Mock external systems, not simple data/value objects.

## Spring Testing Guidance
- **Unit tests do not start Spring**: Avoid `@SpringBootTest` for unit tests; wire SUT and mocks manually.
- **Use slices for integration/component tests** (outside pure unit scope): `@WebMvcTest` for MVC, `@DataJpaTest` for JPA, etc., when verifying framework integration.

## Parameterized and Boundary Testing
- **Parameterized tests**: Use JUnit 5 parameterized tests to cover multiple inputs without loops/conditionals in test bodies.
- **Edge cases**: Include positive, negative, boundary, and error scenarios.

## Test Data and Builders
- **Builders/factories**: Use test data builders for complex objects. Keep data minimal and explicit in the test.
- **No production logic reuse**: Do not call production code to build test inputs. Keep helpers simple and transparent.

## Time and Concurrency
- **Time**: Inject `Clock` or pass timestamps as parameters; avoid `Instant.now()`/`new Date()` inside SUT.
- **Async**: Prefer separating logic from scheduling. If asserting async behavior, use tools like Awaitility with sensible timeouts and deterministic triggers.

## Coverage and CI
- **Coverage target**: Aim for at least 80% line/branch coverage. Do not pursue coverage at the expense of meaningful tests.
- **Automation**: Ensure tests run in CI and fail the build on any test failure.
- **Flakiness**: No `Thread.sleep`-based timing. Eliminate nondeterminism.

## Prohibited in Unit Tests
- **External effects**: Network calls, database access, file I/O, environment-dependent paths or values.
- **Hidden state**: Static/global mutable state; shared singletons across tests.
- **Order dependence**: Tests must pass individually and together in any order.

## Minimal Template (JUnit 5 + Mockito + AssertJ)
 ```java
 @ExtendWith(MockitoExtension.class)
 class PriceServiceTest {
   @Mock private ExchangeRateClient exchangeRateClient;

   private PriceService priceService;

   @BeforeEach
   void setUp() {
     priceService = new PriceService(exchangeRateClient);
   }

   @Test
   void shouldConvertAmountUsingLatestRate() {
     // Arrange
     BigDecimal amount = new BigDecimal("100.00");
     when(exchangeRateClient.latest("EUR", "USD")).thenReturn(new BigDecimal("1.10"));

     // Act
     BigDecimal actualTotal = priceService.convert(amount, "EUR", "USD");

     // Assert
     assertThat(actualTotal).isEqualByComparingTo("110.00");
   }

   @Test
   void shouldThrowWhenRateUnavailable() {
     // Arrange
     when(exchangeRateClient.latest("EUR", "USD")).thenThrow(new RateUnavailableException("EUR->USD"));

     // Act + Assert
     assertThatThrownBy(() -> priceService.convert(new BigDecimal("1.00"), "EUR", "USD"))
       .isInstanceOf(RateUnavailableException.class)
       .hasMessageContaining("EUR->USD");
   }
 }
 ```

## Parameterized Example
 ```java
 @ParameterizedTest
 @CsvSource({
   "0, 0",
   "1, 1",
   "2, 4",
   "-3, 9"
 })
 void shouldSquareInputs(int input, int expected) {
   // Act
   int actual = MathUtils.square(input);

   // Assert
   assertThat(actual).isEqualTo(expected);
 }
 ```

## Quick Lintable Checks:
- **AAA present**: The test shows clear Arrange, Act, Assert blocks.
- **Single behavior**: The test name and assertions align to one expected behavior.
- **Deterministic**: No time, randomness, sleeps, or external systems.
- **No conditionals/loops**: Use parameterized tests instead.
- **Clear assertions**: Specific assertion methods, meaningful messages.
- **Isolation**: No shared/static mutable state; no reliance on order.
- **Public contract**: Test interacts with public API, not internals.
