# Current UI Implementation Map

This map is derived from the current private frontend route configuration supplied for the August 2026 showcase refresh. It records what is actually routed in the current application rather than what exists only in approved design files.

## Role workspaces

| Area | Current route | Current page/component |
|---|---|---|
| Admin dashboard | `/app/dashboard` | `AdminDashboardPage` |
| Academic Head dashboard | `/app/academic-head-home` | `AcademicHeadDashboardPage` |
| Bursar dashboard | `/app/bursar-home` | `BursarDashboardPage` |
| General Staff dashboard | `/app/general-staff-home` | `GeneralStaffDashboardPage` |
| Teacher dashboard | `/app/teacher-home` | `TeacherHomePage` |
| My Profile | `/app/profile` | `MyProfilePage` |

## Students

| Area | Current route |
|---|---|
| Student list | `/app/students` |
| Register student | `/app/students/new` |
| Student profile | `/app/students/:id` |
| Edit student | `/app/students/:id/edit` |
| Student import | `/app/students/import` |

## Attendance

| Area | Current route |
|---|---|
| Attendance workspace | `/app/attendance` |

The attendance feature also contains dedicated marking and review page components used inside the workspace flow.

## Assessment

| Area | Current route |
|---|---|
| Assessment list | `/app/assessments` |
| Create assessment | `/app/assessments/new` |
| Assessment detail | `/app/assessments/:assessmentId` |
| Policy list | `/app/assessments/policies` |
| Create policy | `/app/assessments/policies/new` |
| Policy detail/edit | `/app/assessments/policies/:policyId` |
| Template list | `/app/assessments/templates` |
| Create template | `/app/assessments/templates/new` |
| Template detail | `/app/assessments/templates/:templateId` |
| Results workspace | `/app/results` |

The route exists for the Results workspace, but the full report-card lifecycle remains deliberately outside the current MVP claim.

## Finance

| Area | Current route |
|---|---|
| Finance administration | `/app/finance` |
| Student finance account | `/app/finance/students/:studentId` |

## Staff and teacher assignments

| Area | Current route |
|---|---|
| Staff list | `/app/staff` |
| Register staff | `/app/staff/new` |
| Staff detail | `/app/staff/:id` |
| Class teacher assignments | `/app/settings/class-teachers` |
| Teaching assignments | `/app/settings/teaching-assignments` |

## School Setup and Academics

| Area | Current route |
|---|---|
| Setup / readiness | `/app/settings` |
| School profile | `/app/settings/school-profile` |
| Educational levels | `/app/settings/educational-levels` |
| Academics hub | `/app/academics` |
| Academic session & term | `/app/academics/session-term` |
| Class groups | `/app/academics/classes` |
| Subjects | `/app/academics/subjects` |
| School calendar | `/app/academics/calendar` and `/app/settings/calendar` |

The current route table therefore confirms that School Calendar is not merely a backend capability: a real `SchoolCalendarPage` is routed from both the Academics and Setup areas.

## Explicit placeholders / deferred areas

The current router intentionally sends Admissions, Promotion and Reports to `ComingSoon`. These are not presented as completed MVP workflows in the public showcase.

## Showcase rule

A route or component being present is evidence of implementation, but it is not by itself proof of visual alignment or end-to-end product completeness. Approved-screen alignment remains a separate product-quality check.
