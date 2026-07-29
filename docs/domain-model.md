# Domain Model — Assessment

Representative excerpts from the `Assessment` bounded context, showing the 
invariant-guarded aggregate pattern used across EduCore.

## Aggregate Root — State Transitions Are Named Methods, Never Setters

```java
public void submit(UUID submittedBy, Instant now, Set<UUID> requiredStudentIds) {
    assertTransitionAllowed(AssessmentStatus.SUBMITTED);
    assertSubmissionComplete(requiredStudentIds);
    approval.recordSubmission(submittedBy, now);
    this.status = AssessmentStatus.SUBMITTED;
}

private void assertTransitionAllowed(AssessmentStatus target) {
    if (!status.canTransitionTo(target)) {
        throw new IllegalStateException(
                "Cannot transition assessment from " + status + " to " + target);
    }
}

private void assertSubmissionComplete(Set<UUID> requiredStudentIds) {
    Set<UUID> scoredStudentIds = studentScores.stream()
            .map(StudentScore::studentId)
            .collect(Collectors.toSet());
    if (!scoredStudentIds.containsAll(requiredStudentIds)) {
        throw new IllegalStateException(
                "Cannot submit: scores are missing for one or more enrolled students");
    }
}
```

Submission isn't just a status flip — it's guarded by a real business rule 
(every enrolled student must have a score) enforced inside the aggregate 
itself, not in a controller or service.

## Invariant Enforcement — AssessmentPolicy

An `AssessmentPolicy`'s enabled components must always sum to exactly 100% 
weighting. This is checked after every mutation that could break it — adding, 
enabling, disabling, or reweighting a component — not just at creation:

```java
public void disableComponent(UUID componentId) {
    assertAtLeastOneEnabledRemainsAfterDisabling(componentId);
    getComponent(componentId).disable();
    assertTotalWeightingValid();
}

private void assertTotalWeightingValid() {
    List<AssessmentComponent> enabledComponents = components.stream()
            .filter(AssessmentComponent::isEnabled)
            .toList();

    if (enabledComponents.isEmpty()) {
        throw new IllegalStateException("At least one assessment component must exist");
    }

    BigDecimal total = enabledComponents.stream()
            .map(c -> c.weighting().percentage())
            .reduce(BigDecimal.ZERO, BigDecimal::add);

    if (total.compareTo(BigDecimal.valueOf(100)) != 0) {
        throw new IllegalStateException(
                "Total weighting of enabled components must equal 100%, currently: " + total + "%");
    }
}
```

A policy is either fully valid or it doesn't exist — there's no intermediate 
"draft" state where weighting can sit at, say, 85%.

## School-Scope Isolation — Application Layer

Every read enforces multi-tenant isolation at the repository query itself, 
and returns 404 rather than 403 on a school mismatch — so the system never 
confirms a resource exists outside a school's boundary:

```java
@Transactional(readOnly = true)
public Assessment getAssessment(UUID assessmentId, UUID schoolId) {
    return assessmentRepository.findById(assessmentId, schoolId)
            .orElseThrow(() -> new ResourceNotFoundException("Assessment not found"));
}
```

## Cross-Module Facts, Not Cross-Module Objects

`AssessmentService` depends on `student.api.StudentLookup` — a published 
Spring Modulith Named Interface — rather than reaching into the Student 
module's internals:

```java
Set<UUID> requiredStudentIds = new HashSet<>(
        studentLookup.activeStudentIdsInClassGroup(assessment.classGroupId()));
assessment.submit(submittedBy, Instant.now(), requiredStudentIds);
```

The aggregate itself never queries another module — the application service 
gathers what it needs and passes it in as a parameter.

---

*Excerpts are taken directly from the private pilot repository's domain and 
application layers. No student, guardian, or school-identifying data is 
included — only structural and business-rule logic.*