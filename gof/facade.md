# LLM Agent RuleSet: Facade (Structural Pattern)

## 1. Recognition & Application Triggers

### 1.1. Problem Context
- A system is very complex or depends on many libraries/classes.
- Client code is tightly coupled to the internal structure of a subsystem.
- Need to provide a simple entry point to a complex subsystem.

### 1.2. Triggers
- Use when you want to provide a simple interface to a complex subsystem.
- Use when there are many dependencies between clients and the implementation classes of an abstraction.
- Use when you want to layer your subsystems.

## 2. Implementation Guidelines

### 2.1. Structural Rules
1. **Define Facade Interface/Class**: Create a class that provides a simplified API.
2. **Identify Subsystem Classes**: The complex classes that perform the actual work.
3. **Implement Delegation**: The Facade delegates client requests to the appropriate subsystem objects.
4. **Encapsulation**: Hide the complexity of the subsystem from the client.

### 2.2. Java 21 & Coding Standards
- Use `interface` for abstractions and contracts.
- Use `record` for DTOs, Value Objects, and immutable messages.
- Use `var` for local variables where readability is preserved.
- **Strictly avoid `null`**: Use `Optional<T>` or empty collections.
- Use **Constructor Injection** exclusively; field injection is forbidden.
- Use **Switch Expressions** and **Pattern Matching** where applicable.
- Use **Text Blocks** (`"""`) for multi-line strings (SQL, etc.).

## 3. SOLID & GRASP Compliance

- Low Coupling: Client depends only on the Facade.
- Indirection (GRASP): Facade provides a layer of indirection.
- Pure Fabrication: Facade is often a pure fabrication to manage complexity.

## 4. Mandatory Constraints for LLM

1. **Don't turn Facade into a God Object**: If the Facade becomes too complex, consider splitting it.
2. **Client can still access subsystem**: Facade provides a simplified view, but doesn't necessarily block access to underlying classes.

## 5. Structural Template (Java)

```java
public class VideoConverterFacade {
    public File convert(String filename, String format) {
        VideoFile file = new VideoFile(filename);
        Codec sourceCodec = CodecFactory.extract(file);
        Codec destinationCodec = (format.equals("mp4")) ? new MPEG4Codec() : new OggCodec();
        VideoFile buffer = BitrateReader.read(file, sourceCodec);
        VideoFile result = BitrateReader.convert(buffer, destinationCodec);
        return new AudioMixer().fix(result);
    }
}
```
