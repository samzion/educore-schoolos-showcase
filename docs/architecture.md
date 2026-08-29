# Architecture Overview

EduCoreOS is a Domain-Driven Design modular monolith built with Spring Boot and Spring Modulith.

The architectural goal is not “microservices without microservices”. It is to make ownership and dependency direction explicit while keeping deployment and operations simple enough for an early school pilot.

## Core rules

### Bounded contexts own their data

Each module owns its aggregate rules, persistence tables and application services. A module does not query another module's repository directly.

### Facts cross boundaries through published APIs

When Assessment needs to know whether a student or subject exists, it asks a published lookup contract. The consuming module receives a fact or ID-oriented projection, not another module's aggregate object.

Examples include contracts such as:

- `StudentLookup`
- `AcademicSessionLookup`
- `AcademicTermLookup`
- `ClassGroupLookup`
- `SubjectLookup`
- `TeacherAssignmentLookup`
- `UserIdentityLookup`

### Reactions can use application events

Where a module reacts to an event elsewhere rather than querying a fact synchronously, Spring Modulith application events are preferred over a direct service dependency.

### Domain code stays framework-independent

The usual direction is:

```text
domain → application → infrastructure / presentation
```

The domain layer contains invariants and named lifecycle methods. HTTP, JPA and security concerns remain outside it.

## Why a modular monolith

For the GIA pilot, the important operational properties are:

- one deployable backend;
- one database platform;
- transactional consistency where needed;
- simpler observability and rollback than a distributed system;
- strong internal boundaries so modules can evolve independently.

This gives the project architectural discipline without paying the operational cost of premature service decomposition.

## Current major modules

```text
identity       authentication, users, roles, identity lookups
school         school profile and school-owned configuration
academic       sessions, terms, levels, classes, subjects, calendar
student        student records, guardians, enrollment
attendance     attendance session/record lifecycle and health
assessment     policies, templates, assessments, scoring, moderation
finance        fee structures, invoices, payments, credit and audit
setup          readiness orchestration across bounded contexts
dashboard      role-tailored operational summaries
shared         cross-cutting primitives and infrastructure support
```

Spring Modulith verification tests guard dependency direction as the codebase grows.
