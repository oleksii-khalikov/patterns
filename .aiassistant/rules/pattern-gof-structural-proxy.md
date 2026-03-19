---
apply: by model decision
instructions: Apply when controlling access to an object for lazy initialization, access control, logging, or caching. Use as a placeholder for expensive or remote resources.
---

# Proxy (Structural Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- Need to control access to an object (lazy init, security, logging, caching).
- The real object is heavyweight or remote.
- Violation of SRP if access control is built into the main object.

### 1.2. Triggers
- Use whenever there is a need for a more versatile or sophisticated reference to an object than a simple pointer.
- Use for Virtual Proxy (lazy init), Remote Proxy, Protection Proxy, or Caching Proxy.

## 2. Implementation Guidelines

1. **Define Service Interface**: Common interface for both the real object and the proxy.
2. **Implement Real Service**: The actual object that performs the work.
3. **Implement Proxy**: Implements the same interface, maintains a reference to the real service, and controls access to it.

## 3. SOLID & GRASP Compliance

- Single Responsibility Principle (SRP): Proxy handles auxiliary tasks; real service handles core logic.
- Open/Closed Principle (OCP): New proxies can be added without changing existing code.
- Indirection (GRASP): Proxy provides a layer of indirection.

## 4. Mandatory Constraints for LLM

1. **Interface Identity**: Proxy must implement the same interface as the real service.
2. **Delegation**: Proxy should delegate the actual work to the real service after performing its own logic.

## 5. Structural Template (Java)

```java
public interface ThirdPartyYouTubeLib {
    Video getVideoInfo(String id);
}

public class ThirdPartyYouTubeClass implements ThirdPartyYouTubeLib {
    @Override public Video getVideoInfo(String id) { /* expensive call */ return new Video(); }
}

public class CachedYouTubeClass implements ThirdPartyYouTubeLib {
    private final ThirdPartyYouTubeLib service;
    private Video cache;

    public CachedYouTubeClass(ThirdPartyYouTubeLib service) { this.service = service; }

    @Override
    public Video getVideoInfo(String id) {
        if (cache == null) cache = service.getVideoInfo(id);
        return cache;
    }
}
```
