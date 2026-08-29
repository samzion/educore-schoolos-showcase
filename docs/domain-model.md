# Representative Domain Patterns

These examples are intentionally simplified representations of patterns used in the private EduCoreOS repositories. They communicate the design without publishing client-sensitive or proprietary implementation detail.

## Assessment lifecycle: named transitions, not status setters

```java
public void submit(UUID submittedBy, Instant now, Set<UUID> requiredStudentIds) {
    assertTransitionAllowed(AssessmentStatus.SUBMITTED);
    assertSubmissionComplete(requiredStudentIds);
    approval.recordSubmission(submittedBy, now);
    status = AssessmentStatus.SUBMITTED;
}
```

Submission is not equivalent to `setStatus(SUBMITTED)`. The aggregate verifies that the transition is legal and that the roster is complete before state changes.

## AssessmentPolicy: atomic structural configuration

An Assessment Policy must be structurally valid whenever it exists. Enabled component weightings total exactly 100%.

An earlier implementation attempted to validate that invariant after each individual component mutation. That was flawed: during construction, the first component naturally creates a temporary total below 100%.

The corrected design treats structural policy definition/replacement as an atomic operation:

```java
public static AssessmentPolicy create(
        UUID schoolId,
        UUID educationLevelId,
        PolicyName name,
        List<ComponentDefinition> definitions) {

    AssessmentPolicy policy = new AssessmentPolicy(...);
    policy.replaceComponents(definitions);
    policy.assertValidStructure();
    return policy;
}
```

Once a policy has historical dependants, structural replacement is guarded. A safe human-readable rename can remain possible without changing historical score meaning.

## Cross-module facts through published contracts

Assessment does not load a Student aggregate from Student internals. The application layer asks a published lookup for the fact it needs:

```java
Set<UUID> requiredStudents =
        studentLookup.activeStudentIdsInClassGroup(classGroupId, schoolId);

assessment.submit(currentUserId, clock.instant(), requiredStudents);
```

The aggregate receives the information required to enforce its own rule; it does not reach into another module.

## Tenant-scoped retrieval

```java
@Transactional(readOnly = true)
public Assessment getAssessment(UUID assessmentId, UUID schoolId) {
    return repository.findById(assessmentId, schoolId)
            .orElseThrow(() -> new ResourceNotFoundException("Assessment not found"));
}
```

Tenant scope is part of retrieval, not a UI convention.

## Finance: correction as explicit lifecycle

A paid invoice cannot simply disappear. A correction may require releasing allocation back to credit before the invoice is voided and a corrected obligation is created.

```text
payment receipt (preserved)
        ↓
allocation released
        ↓
student credit
        ↓
wrong invoice VOID
        ↓
correct invoice
        ↓
credit reallocated
```

The important domain property is not the syntax of a specific method. It is that the final ledger can still explain the entire story.
