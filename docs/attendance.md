# Attendance — From Daily Roll to Operational Health

Attendance became the reference bounded context for EduCoreOS because it forced the project to model authorization, lifecycle, school calendars and operational reporting together.

## Authority is contextual

Current MVP rules distinguish between roles and assignment context:

- designated Class Teacher — mark/save/submit own class;
- Academic Head — school-wide mark/save/submit/lock;
- Administrator — school-wide operation plus correction authority;
- subject-assigned teacher only — no whole-class attendance authority.

The backend enforces these rules directly.

## Submission is auditable

Finalized attendance preserves the actual submitter and submission time. Draft/open work does not silently become official attendance.

## School Calendar changes the denominator

A school cannot measure missing attendance correctly without knowing which dates were expected school days.

Academic owns a School Calendar exception model for holidays, closures and makeup/open-day overrides. Attendance health consumes that fact rather than hardcoding “Monday-Friday always counts”.

Attendance itself is not hard-blocked on a closed day: real schools can still hold a makeup session. Calendar status informs expectations; it does not erase valid attendance activity.

## Tracking Effective Date

A school adopting EduCoreOS part-way through a term should not immediately appear non-compliant for all earlier days.

Attendance therefore supports a tracking-effective date. Health calculations begin from the agreed date on which EduCoreOS becomes the authoritative attendance system.

## Health by class

Backend-authoritative health includes concepts such as:

- expected days;
- submitted days;
- in-progress days;
- missing days;
- completion percentage;
- latest submission date;
- states including `HEALTHY`, `NEEDS_ATTENTION`, `NO_ACTIVITY`, `NOT_STARTED` and `NO_STUDENTS`.

## Design lesson

A useful attendance module does not merely store status rows. It models when attendance is expected, who is authorized to finalize it, what counts as official, and where operational follow-up is needed.
