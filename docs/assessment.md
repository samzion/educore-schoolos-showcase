# Assessment

Three aggregates modeling how a real Nigerian school actually grades — built 
after direct conversation with the pilot school about their real components, 
not an assumed universal model.

## Why Assessment Components Are Free Text, Not an Enum
Real grading components — Classwork, Homework, Weekly Test, Mid-Term Exam, 
Exam — don't match any fixed universal set, and other schools are expected to 
differ further. `ComponentName` is a validated free-text value object 
instead: schools configure their own weighting scheme as data, not code.

## Aggregates
- **AssessmentPolicy** — one per `(school, education level)`. Enabled 
  components must sum to exactly 100% weighting; enforced as a domain 
  invariant.
- **AssessmentTemplate** — a reusable assessment definition (title, type, 
  duration, max score), permanently archivable.
- **Assessment** — the operational aggregate. A 7-state lifecycle 
  (`DRAFT → OPEN → SUBMITTED → REOPENED → APPROVED → LOCKED → ARCHIVED`), 
  with `StudentScore` as a child entity tracking raw and weighted scores per 
  student, and an append-only `ModerationRecord` for score corrections.

## Design Principle: Policy Is Static, Scoring Is Progressive
Policy configuration is one atomic admin action (always fully valid, or it 
doesn't exist). Individual score entry happens progressively across a term. 
Two separate concerns, deliberately not conflated.

Role-based authorization on all mutating endpoints. Full Tier 1/2/3 test 
coverage across all three aggregates.