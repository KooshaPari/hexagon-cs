# Functional Requirements — hexagon-cs

## FR-STRUCT-001
The repository SHALL contain a `.sln` file or a root `Directory.Build.props` that orchestrates all projects.

## FR-STRUCT-002
Projects SHALL be separated by layer: `Domain`, `Application`, `Ports`, `Adapters`, `Host`.

## FR-STRUCT-003
The `Domain` project SHALL have no project references to `Application`, `Ports`, or `Adapters`.

## FR-STRUCT-004
The `Application` project SHALL reference only `Domain` and `Ports`.

## FR-STRUCT-005
The `Adapters` project SHALL implement interfaces from `Ports`.

## FR-DOMAIN-001
Domain entities SHALL be C# `record` or `class` types with no infrastructure NuGet dependencies.

## FR-DOMAIN-002
Domain errors SHALL be typed exception classes or a `Result<T, TError>` type — not raw `Exception`.

## FR-PORTS-001
Inbound port interfaces SHALL be defined in `Ports/Inbound/` with naming convention `I{UseCase}`.

## FR-PORTS-002
Outbound port interfaces SHALL be defined in `Ports/Outbound/` with naming convention `I{ResourceRepository}` or `I{ServiceGateway}`.

## FR-PORTS-003
All port methods that perform I/O SHALL return `Task<T>` or `ValueTask<T>`.

## FR-APP-001
Application use case classes SHALL receive port interfaces via constructor parameters only.

## FR-APP-002
Application use cases SHALL be registered as transient or scoped services, not singleton, unless explicitly justified.

## FR-ADAPTER-001
At least one in-memory implementation of the outbound port interface SHALL exist in `Adapters/Outbound/`.

## FR-ADAPTER-002
At least one inbound adapter (minimal API handler or controller) SHALL exist in `Adapters/Inbound/` or `Host/`.

## FR-DI-001
`Program.cs` SHALL register all adapters bound to their port interfaces.

## FR-TEST-001
`dotnet test` SHALL exit 0 with all tests passing.

## FR-TEST-002
Each use case SHALL have a corresponding test class using a mocked port interface.

## FR-LINT-001
`<Nullable>enable</Nullable>` SHALL be set in all project files.

## FR-LINT-002
`<TreatWarningsAsErrors>true</TreatWarningsAsErrors>` SHALL be set in `Directory.Build.props` or each `.csproj`.
