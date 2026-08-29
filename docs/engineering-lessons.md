# Engineering Lessons from Building EduCoreOS

## 1. Green tests are not product completeness

Late in the MVP cycle, EduCoreOS had a green backend regression, a green frontend regression and a fresh-school setup flow reaching `OPERATIONALLY_READY`.

Deployment work started.

Then the project asked a different question:

> Have the approved user workflows actually been implemented screen-by-screen?

That led to an Approved Screens vs Current UI audit.

The lesson is simple but expensive to learn late:

```text
Code complete ≠ Integration complete ≠ Product complete ≠ Production ready
```

A backend capability can be correct and well-tested while the corresponding user workflow is missing, inaccessible or inconsistent with the intended design.

## 2. “Delete” is often the wrong business operation

Finance reinforced a domain-design principle: once history matters, correction should usually be explicit.

Voiding, releasing an allocation, reversing, refunding and reallocating all communicate different business truths. A generic delete would erase those distinctions.

## 3. Real school operations should change the model

Several domain decisions came from asking how the pilot school actually works rather than imposing generic SaaS assumptions.

Examples include configurable assessment component names, the school's real academic levels, admission-number handling and the need for a school calendar that can distinguish normal weekdays, holidays, closures and makeup days.

## 4. Module boundaries need automated enforcement

Architectural diagrams are easy to ignore under delivery pressure. A failing Modulith boundary test is harder to ignore.

The project has used those tests to catch real dependency mistakes, not merely to document an idealized architecture.

## 5. Historical truth deserves first-class modeling

Attendance submission actors, assessment moderation records, finance actor/time history and immutable payment references all reflect the same idea:

> operational software should explain what happened, not just show the latest state.

## 6. A pilot is a product strategy, not an excuse for sloppy engineering

The pilot scope is intentionally constrained, but the implemented workflows still need reliable authorization, auditability, tenant isolation, migration safety and recovery planning.

The goal is not “enterprise everything”. It is the smallest system a real school can safely trust.
