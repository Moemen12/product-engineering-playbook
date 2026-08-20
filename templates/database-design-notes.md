# Database Design Notes: [Project Name]

## 1. Core journey

What complete user journey are we supporting first?

> [A target user can perform the main action and receive the meaningful result.]

## 2. Journey steps and data changes

| Step | Information needed | Information created or changed | Must it remain later? |
|---|---|---|---|
| [Step] | [Data needed] | [Data changed] | [Yes / No] |
| [Step] | [Data needed] | [Data changed] | [Yes / No] |
| [Step] | [Data needed] | [Data changed] | [Yes / No] |

## 3. Domain objects

List the important business concepts, not screens.

```text
[User]
[Main business object]
[Related object]
[Submission or transaction]
[Result]
```

For each object, ask whether it has its own identity, lifecycle, relationships, rules, or history.

## 4. Relationships

| Relationship | Cardinality | Ownership or meaning |
|---|---|---|
| [Object A] → [Object B] | [One-to-one / one-to-many / many-to-many] | [Meaning] |
| [Object A] → [Object B] | [Cardinality] | [Meaning] |

## 5. State transitions

```text
[State A] → [State B] → [State C]
                 ↘ [Failure state]
```

For each transition, define who can cause it, what must be true before it happens, and whether it can be repeated.

## 6. History requirements

What can change? What must remain historically correct? Do we need an immutable snapshot, event, transaction, or audit record?

[Write here.]

## 7. Persistent data

| Information | Store? | Why? | Source or derived? |
|---|---:|---|---|
| [Information] | [Yes / No] | [Reason] | [Source / Derived] |
| [Information] | [Yes / No] | [Reason] | [Source / Derived] |

## 8. First database model

| Table or collection | Purpose | Important fields | Main relationships |
|---|---|---|---|
| [Name] | [Purpose] | [Fields] | [Relationships] |
| [Name] | [Purpose] | [Fields] | [Relationships] |

## 9. Constraints and invariants

What must always be true?

- [Constraint.]
- [Constraint.]
- [Constraint.]

## 10. Common queries

- [Load the current user’s records.]
- [Load the available items with filters.]
- [Create or update a draft.]
- [Submit a record.]
- [Load historical results.]
- [List or rank records for a group.]

## 11. Index and performance thoughts

Which fields are used for filtering, sorting, joining, or pagination?

[Write here. Do not add indexes without a query or workload reason.]

## 12. Failure cases

| Failure case | Expected behavior |
|---|---|
| Duplicate request | [Behavior] |
| Unauthorized access | [Behavior] |
| Invalid state transition | [Behavior] |
| Dependency failure | [Behavior] |
| Partial processing | [Behavior] |

## 13. Tradeoffs and future changes

| Decision | Why now? | Tradeoff | Revisit when |
|---|---|---|---|
| [Decision] | [Reason] | [Tradeoff] | [Trigger] |
