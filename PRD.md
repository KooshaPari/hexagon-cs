# Product Requirements Document — hexagon-cs

## Overview

`hexagon-cs` is the canonical C# / .NET hexagonal architecture template for the Phenotype ecosystem. It provides a production-ready scaffold for .NET services that enforces ports-and-adapters architecture, clean domain isolation, and idiomatic C# patterns using interfaces and dependency injection.

## Problem Statement

C# services in the Phenotype ecosystem lack a standardised hexagonal starting point. Each service organises layers differently, making governance enforcement and cross-service navigation inconsistent.

## Goals

- Provide a C# project scaffold with enforced hexagonal layer separation using .NET projects/folders.
- Demonstrate interface-based port definitions with .NET DI container wiring.
- Include example domain entities, application use cases, and adapter pairs.
- Target .NET 8+ (latest LTS / current stable) with nullable reference types enabled.

## Non-Goals

- Does not implement production business logic.
- Does not provide Kubernetes or Docker deployment configuration.
- Not a replacement for ASP.NET Core, Entity Framework, or other .NET libraries.

## Epics & User Stories

### E1 — Scaffold Structure
- E1.1: `dotnet build` succeeds immediately after cloning.
- E1.2: Solution structure: `Domain/`, `Application/`, `Ports/`, `Adapters/`, `Host/` projects or folders.
- E1.3: No circular project references in `.csproj` dependency graph.

### E2 — Domain Layer
- E2.1: Domain entities are plain C# records or classes with no infrastructure dependencies.
- E2.2: Domain errors use typed exceptions or Result types (no untyped `Exception`).
- E2.3: Domain project references zero external NuGet packages.

### E3 — Port Interfaces
- E3.1: Inbound ports are C# interfaces in `Ports/Inbound/`.
- E3.2: Outbound ports are C# interfaces in `Ports/Outbound/`.
- E3.3: Ports use `Task<T>` / `ValueTask<T>` for async contracts.

### E4 — Application Layer
- E4.1: Use case handlers accept port interfaces via constructor injection.
- E4.2: Application project references only Domain and Ports projects.

### E5 — Adapters
- E5.1: At least one in-memory outbound adapter implements the outbound port interface.
- E5.2: At least one inbound adapter stub (Controller or minimal API handler) exists.

### E6 — DI Wiring
- E6.1: `Host/` or `Program.cs` wires adapters to ports via `IServiceCollection`.
- E6.2: All registrations use interface types, not concrete types.

### E7 — Testing
- E7.1: `dotnet test` passes with zero failures.
- E7.2: Unit tests use `Moq` or `NSubstitute` for port mocking.

## Acceptance Criteria

- `dotnet build` and `dotnet test` succeed with zero errors.
- No domain or application project directly references an adapter assembly.
- Nullable reference types (`<Nullable>enable</Nullable>`) enabled in all projects.
