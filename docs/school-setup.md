# School Setup & Operational Readiness

School Setup is not meant to become another owner of school data.

Its job is to answer a different question:

> **Can this school actually operate in EduCoreOS yet?**

## Readiness as orchestration

The Setup module composes facts from the bounded contexts that own them:

- school profile;
- educational levels;
- active academic session;
- active term;
- operational class groups;
- subjects;
- active staff/teacher roles;
- teacher coverage;
- current-session student enrollment;
- assessment policy;
- active-term finance setup.

The flow exposes ordered readiness steps and higher-level readiness states such as `SETUP_INCOMPLETE`, `ACADEMICALLY_READY` and `OPERATIONALLY_READY`.

## Why this is not just a checklist

A trustworthy readiness engine must understand operational semantics.

Examples:

- a class attached to an inactive educational level should not create a false teacher-assignment blocker;
- historical enrollment should not satisfy the active session's student-readiness requirement;
- inactive staff should not satisfy current operational coverage;
- finance readiness must refer to the current operational term, not any historical fee structure.

## Fresh-school verification

The private project contains an end-to-end fresh-school flow that verifies readiness transitions as the required configuration is added.

The current release-candidate baseline completed all 11 readiness transitions through `OPERATIONALLY_READY`.

## Current UI-audit lesson

Backend readiness and a working aggregate Setup page are not enough to prove that every approved setup screen exists and is reachable.

Before external rollout, EduCoreOS is performing a separate screen-level audit covering School Profile, Educational Levels, academic period configuration, Class Groups, Subjects, School Calendar and the remaining operational setup workflows.

This distinction is intentional:

```text
Backend capability ≠ complete user workflow
```
