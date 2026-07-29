# Attendance

The reference bounded context for EduCore's aggregate pattern — every module 
since follows the design established here.

## Aggregate Design
Aggregate root owns child entities as in-memory lists. Child mutation methods 
are package-private, reachable only through the root's own invariant-checked 
methods — never a generic setter. State transitions are named domain methods.

## Cross-Module Communication
Two distinct patterns, used deliberately:
- **Facts** (e.g. "does this student exist?") → Spring Modulith Named 
  Interfaces (`student.api.StudentLookup`)
- **Reactions** (e.g. "something happened elsewhere, react to it") → 
  `@ApplicationModuleListener` / `ApplicationEventPublisher`, never a direct 
  service call

## School-Scope Isolation
Every service method loading an entity by ID verifies the requesting user's 
`schoolId` against the entity's. Mismatches return 404, never 403 — the 
system doesn't reveal that a resource exists outside a school's boundary.

Test coverage: Tier 1 domain, Tier 2 Mockito, Tier 3 Testcontainers.