# 5. Failure Cases and Technical Tradeoffs

A product is not defined only by its happy path. Professional engineering also asks what happens when users retry, data is missing, permissions are wrong, or an external system fails.

## Think about failure cases early

For each important action, describe the expected behavior when something goes wrong.

| Failure case | Expected behavior |
|---|---|
| User submits twice | The operation is idempotent or the second request is safely rejected. |
| User changes a record after its deadline | The backend rejects the change. |
| Two users attempt the same limited action | The system enforces the business constraint safely. |
| A processing job runs twice | Results are not duplicated. |
| Source data is incomplete | The system records the error and does not publish misleading results. |
| A user loses connection during submission | The user can retry without creating duplicate data. |
| An unauthorized user requests a record | The system denies access without exposing private details. |
| A referenced record is removed | Historical data remains correct or the removal is blocked according to the domain rules. |
| A dependency is unavailable | The product shows a useful error and preserves recoverable work. |
| A user opens the product on a small screen | The primary journey remains usable. |

## Idempotency

An operation is **idempotent** when repeating the same request does not create multiple unintended results.

Important examples include:

- Creating a payment after a network retry.
- Submitting an order.
- Importing the same file twice.
- Processing the same event twice.
- Publishing a result after a background job retries.

A common approach is to identify the operation with a unique key and store the result. A retry can then return the existing result instead of creating another one.

## Source data and derived data

When a system processes data, distinguish between the original facts and the calculated results.

```text
Source records
    ↓
Validation and normalization
    ↓
Business calculation
    ↓
Derived results
    ↓
User-facing summaries
```

If a result can be recalculated, keep enough source information and rule versioning to reproduce it when necessary. This helps with corrections, audits, debugging, and customer support.

## Correctness before optimization

Start with a clear and correct model. Optimize after you understand the real workload.

| Situation | Reasonable next step |
|---|---|
| Repeated expensive calculation | Store a derived result or precomputed summary. |
| Slow filtered query | Review the query and add an appropriate index. |
| Large history list | Add pagination and a stable sort order. |
| Read-heavy public data | Consider a cache after measuring the need. |
| Long-running processing | Move it to a background job. |
| Search requirements grow | Consider a dedicated search index. |
| One database is overloaded | Review workload separation and scaling options. |

Do not add infrastructure because it is fashionable. Explain the problem the component solves.

## Keep source data authoritative

If you store a cached or aggregated value, identify its source of truth.

For example:

```text
Source of truth: individual score records
Derived view: leaderboard totals
```

If a leaderboard total becomes incorrect, the team should know whether it can be rebuilt from the source score records.

## Access control is part of data design

For every important record, ask:

- Who owns it?
- Who can read it?
- Who can create it?
- Who can update it?
- Who can delete it?
- Should deletion be allowed, or should the record be archived?
- Does an administrator have different permissions?

Do not rely only on frontend hiding. Authorization must be enforced where data is accessed and changed.

## Write down important tradeoffs

A short decision record is more useful than keeping every design choice in someone’s memory.

| Decision | Reason | Tradeoff | Revisit when |
|---|---|---|---|
| Keep source and derived scores separate | Allows recalculation and explanation | Requires processing work | Scoring rules change or volume grows |
| Use one relational database first | Keeps the system simple and consistent | May need specialized services later | Query or workload limits are measured |
| Store submission snapshots | Preserves historical correctness | Uses more storage | Domain no longer requires historical state |
| Import data manually for the prototype | Avoids premature external dependency | Not fully automated | Production data source is confirmed |

## A good early system is honest

A prototype can use simplified data and still have professional engineering. State clearly:

- What is real.
- What is seeded or manually entered.
- What is simplified for the prototype.
- What is intentionally not implemented.
- What design boundary allows future integration.
- What would need to change for production scale.

Honest scope is stronger than pretending a small demo is already a global system.
