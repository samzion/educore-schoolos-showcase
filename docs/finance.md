# Finance — Preserving the Financial Story

The Finance module became one of the clearest examples of why business software needs more than CRUD.

A school can correct a mistake without pretending the mistake never happened.

## Core capabilities

The current MVP includes:

- fee structures by term and educational level;
- immutable invoice-item snapshots;
- bulk obligation generation;
- independent payments;
- partial settlement;
- allocations;
- overpayment credit;
- discounts and waivers;
- refunds;
- reversals;
- student ledger and term summaries;
- Admin/Bursar authorization boundaries.

## Correction instead of destructive deletion

Once an invoice has financial activity, “delete it and start again” is not acceptable.

The correction flow distinguishes the commercial obligation from the genuine receipt:

```text
Wrong invoice
   ↓
Release allocation from the genuine payment
   ↓
Return value to student credit
   ↓
VOID the incorrect invoice with actor/time/reason
   ↓
Create the corrected obligation
   ↓
Allocate the preserved credit correctly
```

The receipt remains historically true while the receivable is corrected.

## Transaction time vs system-recorded time

A payment may have happened earlier than it was entered into EduCoreOS.

Finance therefore distinguishes:

- `transactionOccurredAt` — when the real-world transaction happened;
- `recordedAt` — when EduCoreOS recorded it.

The frontend renders finance audit times explicitly in the pilot school's timezone rather than depending on a browser's local timezone.

## Human-readable audit actors

Audit records preserve immutable actor UUIDs as the source of truth. For presentation, Finance batch-resolves human-readable names through Identity's published `UserIdentityLookup`.

If an old user can no longer be resolved, the UUID remains available rather than losing the audit attribution.

## Controlled adjustment reasons

Discounts and waivers use stable reason codes with optional notes. “OTHER” requires explanatory free text. This keeps reporting and audit meaning consistent without pretending every school decision fits a single fixed taxonomy.

## Design lesson

Financial systems should model corrections as events and state transitions, not as opportunities to erase inconvenient history.
