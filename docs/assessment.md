# Assessment — Configurable Policy, Progressive Scoring, Controlled Finality

Assessment was modeled from the pilot school's real grading process rather than from a universal hardcoded “test/exam” enum.

## Three aggregates

### AssessmentPolicy

Defines the weighted component structure for an educational context. Component names are validated school-configured data rather than a closed enum.

A structural policy is atomic and fully valid whenever it exists: enabled weighting totals 100%. Structural replacement is guarded once historical templates/assessments depend on the policy so score meaning cannot silently change later.

### AssessmentTemplate

A reusable definition such as title/type/duration/maximum score. Templates can be archived; deletion is restricted once used by historical assessments.

### Assessment

The operational scoring aggregate. Its lifecycle includes:

```text
DRAFT → OPEN → SUBMITTED → APPROVED → LOCKED → ARCHIVED
                 ↓
             REOPENED → OPEN
```

Scores evolve progressively while editing is allowed. Submission, moderation, approval and locking are explicit business transitions.

## Score history and moderation

Student scores belong to the Assessment aggregate. Moderation records are append-only audit history rather than overwriting the fact that a score changed.

## Contextual authorization

Teacher operations require the correct active class/subject assignment. Academic Head and Administrator workflows have different review/governance authority.

## Historical integrity

The project has repeatedly favored historical truth over convenient mutation:

- policy structure cannot be changed casually after use;
- used templates are retained/archived;
- submitted/approved/locked assessment states become increasingly constrained;
- moderation records explain score corrections.

## Deliberate MVP boundary

The Assessment engine is not the same thing as a complete report-card publication lifecycle. Expected subject coverage, report-card approval/publication and printable report artifacts remain separate product work rather than being fabricated from the current Assessment state.
