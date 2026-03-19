---
apply: by model decision
instructions: Apply Boy-Scout Rule to leave code cleaner than you found it. Use to prevent technical debt accumulation through continuous small improvements.
---

# Boy-Scout Rule

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Technical debt accumulates over time.
- Code quality degrades with each modification.
- Small issues are ignored because "it's not my code."
- Refactoring is postponed indefinitely.

### 1.2. Triggers
- Use when making any change to existing code.
- Use when you notice a small improvement opportunity.
- Use when code is slightly messy but "works."
- Use to resist the temptation to leave things as-is.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Small Improvements**: Make at least one small improvement with each commit.
2. **Opportunistic Refactoring**: Clean up code you're already touching.
3. **No Broken Windows**: Don't leave obvious issues for others to fix.
4. **Incremental Progress**: Big cleanups happen through many small changes.

### 2.2. Examples of Small Improvements
- Rename a poorly named variable or method.
- Extract a method from duplicated code.
- Add a missing `final` keyword.
- Remove unused imports or dead code.
- Improve a confusing comment or remove an obsolete one.
- Replace a magic number with a named constant.

## 3. Mandatory Constraints for LLM

1. **Always Improve**: Never leave code worse than you found it.
2. **Scope Appropriately**: Keep improvements related to the current task.
3. **Test Coverage**: Ensure improvements don't break existing tests.
4. **Commit Separately**: Consider separating refactoring from feature commits.

## 4. Structural Template (Java)

```java
// Before (found this while adding a feature)
public void process(String s) {
    if (s != null) {
        if (s.length() > 0) {
            // process
            System.out.println(s.toUpperCase());
        }
    }
}

// After (left it cleaner)
public void process(String input) {
    if (input == null || input.isEmpty()) {
        return;
    }
    var upperCased = input.toUpperCase();
    logger.info("Processed: {}", upperCased);
}

// Improvements made:
// 1. Renamed 's' to 'input'
// 2. Used guard clause instead of nested ifs
// 3. Replaced System.out with proper logging
// 4. Used isEmpty() instead of length() > 0
// 5. Added var for local variable
```
