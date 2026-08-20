# 3. Domain and Database Design

Database design should begin after the core journey and its important business rules are clear enough. It does not need to wait until the entire product roadmap or final UI design is complete.

The goal is to identify what the product means, what the system must remember, and how those facts relate.

## Domain model and database model are different

The **domain model** describes important business concepts, relationships, rules, and state transitions.

The **database model** describes how persistent information is represented in storage.

> The domain model describes the meaning. The database model describes the stored representation of that meaning.

A single domain object may use several tables. Several domain concepts may sometimes be represented together. A domain concept may also be calculated rather than stored directly.

For example, a domain concept called `Order` might be stored using:

```text
orders
order_items
payments
shipments
```

The domain model starts the conversation, but the database model must also consider history, query patterns, constraints, transactions, and performance.

## Start from the core journey

Walk through the primary journey and ask three questions at every step:

1. What information is needed before this step?
2. What information is created or changed by this step?
3. What information must still exist afterward?

Example:

| Journey step | Information needed | Information created or changed |
|---|---|---|
| User starts | User identity and permissions | User or session record |
| User chooses options | Available items and rules | Draft selection |
| User submits | Valid draft and current state | Immutable submission or transaction |
| System processes | Submission and source data | Derived result |
| User views result | Stored result and access rights | Read history or activity record, if needed |

This process turns a user journey into a first list of durable business facts.

## Identify durable business nouns

Look for nouns that represent things the business must remember, protect, query, or use later.

Typical examples include:

```text
User
Account
Organization
Project
Item
Order
Payment
Message
Submission
Result
Permission
Event
```

Do not create a table for every noun automatically. First ask whether it has its own identity, lifecycle, relationships, rules, or history.

## Decide what belongs in persistent storage

A useful test is:

> If the server restarted or the user closed the browser, would this information need to exist afterward?

| Usually belongs in persistent storage | Usually does not need a database record |
|---|---|
| User identity and permissions | Open or closed modal state |
| Business records | Current browser tab |
| User-created content | Hover state |
| Submissions and transactions | Temporary form text before submission |
| Deadlines and status | Loading spinner state |
| Historical results | A value used only during one render |
| Audit and processing records | Short-lived UI preferences, unless they must sync across devices |

Some information is derived but still worth storing for performance, such as a leaderboard snapshot or a search index. Mark it as derived and keep the underlying source facts available when correctness matters.

Secrets should be managed through an appropriate secret-management system, not treated as ordinary application data. Large files should normally use object storage with metadata in the database.

## Define relationships and cardinality

For each relationship, ask how many records can exist on each side.

| Relationship | Typical database representation |
|---|---|
| One user has many projects | `projects.user_id` foreign key |
| Many users belong to many groups | `group_members` join table |
| One order has many items | `order_items.order_id` foreign key |
| One item belongs to one category | `items.category_id` foreign key |
| One user has many submissions | `submissions.user_id` foreign key |
| One submission contains many items | `submission_items` join table |

Writing these relationships down prevents accidental duplication and unclear ownership.

## Model state transitions explicitly

Many products have records that move through states. Write the valid transitions before designing the table.

```text
Draft → Submitted → Processing → Completed
                    ↘ Failed
```

Then decide:

- Which states are valid?
- Who can cause each transition?
- Can a record move backward?
- What happens if the same request is sent twice?
- Which timestamp or actor should be recorded?
- What data becomes immutable after submission?

State should be enforced by the backend and database constraints where practical. A disabled button in the UI is not sufficient protection.

## Separate current state from history

Current state changes. History often must remain accurate.

If a user can edit a draft after submitting it, do not calculate the historical result from the current draft. Store a submission snapshot or immutable event record.

A useful interview statement is:

> The editable object represents the current state. The submitted object represents what was actually committed at that time, so later edits cannot rewrite history.

This distinction is important for orders, payments, financial records, approvals, published content, game rounds, and many other domains.

## Normalize first, optimize with evidence

Start with a clear normalized model where each important fact has one logical source. This reduces inconsistent updates and makes business rules easier to understand.

Denormalized or derived data can be added later for a measured need:

| Need | Possible solution |
|---|---|
| Expensive repeated calculation | Store a derived summary or materialized view |
| Frequently requested leaderboard | Maintain a leaderboard snapshot |
| Large result set | Add pagination and appropriate indexes |
| Slow search | Add a search index or specialized search service |
| Repeated read-heavy query | Add a cache after measuring the bottleneck |
| Heavy asynchronous work | Move processing to a background job |

Keep the original source facts when the derived value must be recalculated or explained.

## Think about common queries

Database design is not only about tables. It is also about how the product reads data.

Write down the most common queries for the core journey:

```text
Load the user’s current records.
Load available items with filters.
Load one submitted record and its details.
Load a user’s history.
Load the top results for a group.
```

Then consider:

- Which columns are used in filters?
- Which columns are used for sorting?
- Which relationships are joined often?
- Does the query need pagination?
- Can an index help?
- Does the result need to be strongly consistent?
- Is a derived read model justified?

Do not add indexes blindly. Add them to support known access patterns and review them as the product grows.

## Keep the first schema small

A first version should contain the entities required by the primary journey and its rules. Do not add tables for unconfirmed features, future integrations, or speculative scale.

A useful first schema often includes:

```text
users
teams or projects
items
memberships, if collaboration is required
submissions or transactions
results
```

Add history, audit, processing, and derived tables when the domain or operational requirements justify them.

## Database-design thinking sequence

Use this sequence in projects and interviews:

```text
1. Clarify the core journey.
2. Identify durable business concepts.
3. Define relationships and ownership.
4. Define states and valid transitions.
5. Decide what must remain historically correct.
6. Identify important queries.
7. Add constraints and validation rules.
8. Create the smallest schema that supports the journey.
9. Discuss performance and scale tradeoffs.
10. State what you would add only when the product needs it.
```

This is enough structure to reason clearly without overcomplicating the design.

## A simple database-design checklist

Before implementation, confirm:

- Every important record has a stable identity.
- Relationships and ownership are clear.
- Required fields and allowed states are defined.
- Important rules are enforced outside the UI.
- Historical records cannot be accidentally rewritten.
- Common queries have a reasonable path.
- Duplicate requests are handled safely where needed.
- Sensitive data has appropriate access control.
- Derived data is distinguishable from source data.
- The schema supports the current scope without guessing the entire future.
