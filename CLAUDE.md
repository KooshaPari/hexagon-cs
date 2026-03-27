# hexagon-cs

Hexagonal Architecture (Ports & Adapters) template for C#/.NET. Clean, SOLID, DDD-ready scaffold for .NET services.

## Stack
- Language: C#
- Framework: .NET (latest stable)
- Key deps: ASP.NET Core, MediatR (or similar CQRS library)
- Docs: `docs/`, `adr/`

## Structure
- Domain layer: entities, value objects, domain events (no framework deps)
- Application layer: use cases, port interfaces
- Adapters: primary (API controllers) and secondary (repositories)
- `adr/`: Architecture Decision Records

## Key Patterns
- Hexagonal architecture with strict dependency inversion
- Ports as C# interfaces; adapters implement via constructor injection
- CQRS pattern preferred for application layer
- DDD naming: entities, aggregates, value objects, domain services

## Adding New Functionality
- Domain: Domain project/namespace
- Use cases: Application project/namespace
- Adapters: Infrastructure or API projects
- Run `dotnet build` and `dotnet test` to verify
