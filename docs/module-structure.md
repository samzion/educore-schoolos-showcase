# Module Structure

EduCoreOS is built as a Spring Modulith modular monolith. Module boundaries are part of the executable architecture: verification tests fail when implementation code introduces unauthorized cross-module dependencies.

## Boundary rules

- Each bounded context owns its aggregates and persistence.
- Other modules consume published interfaces rather than internal repositories/services.
- Cross-module facts are ID/projection oriented; aggregate objects do not travel between contexts.
- Cross-module reactions use application events where appropriate.
- Domain rules remain inside domain/application layers rather than controllers.
- Tenant scope is propagated across published lookups.

![Module structure](../assets/architecture/module-structure.png)

> The diagram is retained from an earlier milestone and will be replaced with a current bounded-context diagram in the next visual refresh.

## Current major bounded contexts

- Identity
- School
- Academic
- Student
- Attendance
- Assessment
- Finance
- Setup
- Dashboard
- Shared infrastructure

## Why this matters

The project has grown substantially since the original four-module showcase. Without enforceable boundaries, Finance, Setup and Dashboard aggregation could easily become shortcuts that reach directly into other modules' repositories.

Instead, each new cross-module requirement is treated as an API-design decision.
