# Architecture Decision Records — hexagon-cs

## ADR-001: Interface-Based Dependency Injection for Ports

**Status:** Accepted

**Context:** .NET has a built-in DI container. Ports must be expressed as C# interfaces to enable constructor injection and test mocking.

**Decision:** All ports are C# interfaces. The `Host` project wires concrete adapters to port interfaces using `IServiceCollection.AddScoped<IPort, ConcreteAdapter>()`.

**Rationale:** Native .NET DI is zero-dependency and sufficient for all Phenotype service patterns. Third-party containers (Autofac, Castle Windsor) add complexity without benefit at template level.

**Alternatives Considered:**
- Abstract base classes: less flexible; prevents multiple inheritance of ports.
- Property injection: less explicit; harder to trace in tests.

**Consequences:** All adapters must be registered before application startup. Missing registrations throw at runtime (or at test setup with `IServiceProvider.GetRequiredService`).

---

## ADR-002: Separate Projects per Layer (Not Folders)

**Status:** Accepted

**Context:** .NET supports both folder-based and project-based layer separation. Projects enforce compile-time dependency boundaries; folders do not.

**Decision:** Each layer is a separate `.csproj` project. Project references encode allowed dependencies and are checked by the compiler.

**Rationale:** Circular reference detection is compile-time. `dep-guard` tooling can inspect `.csproj` reference graphs directly.

**Alternatives Considered:**
- Single project with namespace folders: no compile-time boundary enforcement.
- NuGet package per layer: excessive overhead for a template.

**Consequences:** Solution file required. Slightly more setup overhead, but enforces boundaries rigorously.

---

## ADR-003: Records for Domain Entities

**Status:** Accepted

**Context:** C# offers classes, structs, and records. Domain entities benefit from value semantics and immutability.

**Decision:** Use `record` types for domain entities and value objects. Use `class` only when mutation is explicitly required.

**Rationale:** Records provide structural equality, `with` expression copies, and immutability by default — all consistent with domain model best practices.

**Consequences:** Adapters that map to mutable EF Core entities must perform explicit mapping; this is intentional (anti-corruption layer).
