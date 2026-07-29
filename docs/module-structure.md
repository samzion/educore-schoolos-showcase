# Module Structure

EduCore is built on Spring Modulith, with module boundaries enforced by an 
automated test that fails the build on any unauthorized cross-module import.

## Boundary Rules
- Each bounded context owns its own database tables — no shared tables, 
  no cross-module foreign keys into another module's schema
- A module needing a *fact* from another module's aggregate consumes a 
  published Named Interface (e.g. `student.api.StudentLookup`, 
  `academic.api.SubjectLookup`) — never a direct object reference, never 
  a shared entity
- A module *reacting* to something happening elsewhere listens for a domain 
  event — never a direct service call across module lines
- IDs cross module boundaries as raw UUIDs, never as domain objects

## Layering (enforced in every module)
`domain` (pure Java, no framework annotations) → `application` 
(orchestration only) → `infrastructure` (JPA, security, external adapters) 
→ `presentation` (thin HTTP controllers, zero business logic)

![Module Strucuture](screenshots/module-structure.png)

## Current Bounded Contexts
Academic Foundation, Student, Attendance, and Assessment/Grading are 
complete and in active pilot use. Each was built domain-first: invariants 
and aggregate design settled before any persistence or API work began.