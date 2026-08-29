# Security, Authorization & Tenant Isolation

EduCoreOS is multi-tenant from the beginning. School isolation and role authority are treated as domain/application concerns, not only UI concerns.

## Tenant isolation

The authenticated user carries a `schoolId`. Tenant-scoped services and repository queries use that school context when loading data.

A request for a resource outside the authenticated school's boundary is treated as “not found” rather than exposing that another tenant's resource exists.

## Role-aware operations

Current operational roles include:

- Administrator
- Academic Head
- Teacher
- Bursar
- General Staff
- Applicant-related identity scope where applicable

Authorization is enforced server-side. Hiding a frontend button is never considered the security boundary.

Examples of tightened rules include:

- subject assignment alone does not grant whole-class attendance authority;
- protected Admin authority cannot be casually granted or removed through normal staff-role management;
- Admin self-deactivation is blocked;
- teacher assessment operations require the appropriate class/subject assignment context;
- medical student information has a stricter read-permission tier than ordinary directory/profile data.

## Audit integrity

Where historical actor identity matters, immutable actor IDs are retained. Human-readable names are enrichment, not replacement of the audit source of truth.

## External rollout gates

A locally working application is not declared production-ready without direct evidence for:

- TLS/reverse proxy configuration;
- externalized secrets;
- bootstrap credential rotation;
- database backup and restore drill;
- retention/encryption responsibility;
- monitoring and logging;
- rollback ownership;
- post-deployment cross-role smoke tests.

Those checks are tracked separately from application feature completion.
