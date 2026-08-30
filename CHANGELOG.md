# Showcase Changelog

## 2026-08 — Showcase v3

The showcase was reconciled again against the latest supplied frontend/backend/Screens workspace rather than relying only on historical tracker state.

### Added
- Current EduCoreOS product branding asset from the live frontend (`og-image.png`).
- Current frontend route/component implementation map.
- Source-verified product-surface table in the README.
- Explicit route evidence for Admin, Academic Head, Bursar, Teacher and General Staff workspaces.
- Explicit route evidence for Students, Attendance, Assessment, Finance, Staff, School Setup and Academics.

### Corrected
- School Calendar is now documented as **implemented and routed** in the current frontend through both `/app/academics/calendar` and `/app/settings/calendar`.
- The showcase no longer describes School Calendar as merely a backend capability awaiting a UI implementation check.
- The remaining Approved Screens vs Current UI work is framed correctly as an **alignment/product-quality audit**, not proof that the route is absent.

### Visual-proof policy
- No generated/mock UI is presented as implementation evidence.
- Historical engineering screenshots remain clearly identified as milestones.
- Current live product screenshots will be added only from a running release candidate using sanitized/demo data.

## 2026-08 — Showcase v2

The public showcase was rewritten to reflect the current EduCoreOS product and engineering state rather than the July architecture-only snapshot.

### Added
- Current product-position matrix.
- Backend/frontend verification snapshot.
- Finance case study.
- School Setup/readiness case study.
- Security and tenancy overview.
- Architecture overview.
- Engineering lessons from the MVP closure process.
- Explicit distinction between pilot-ready application work and external production-rollout evidence.
- Explicit approved-screens/current-UI audit status.

### Corrected
- Removed the outdated claim that only Academic, Student, Attendance and Assessment represent the current product.
- Removed the outdated 492-test figure from the README's current-state claim.
- Removed wording implying all completed modules are already in active pilot use.
- Corrected the AssessmentPolicy explanation: structural policy configuration is atomic; the system does not rely on temporarily-invalid incremental weighting mutations.
