# 4. Interview Thinking Framework

When an interviewer asks you to design a database or system, they are usually evaluating your reasoning more than your ability to name many technologies.

The goal is to make your thinking visible, structured, and connected to the user’s core journey.

## Begin by stating your assumption

If the prompt is incomplete, do not silently guess. Say:

> “I will first define the core operation. I’ll assume a user creates a record, submits it, and later views the result. I would confirm the exact workflow with the product requirements.”

This shows that you know the difference between a requirement and an assumption.

## Use this order

### 1. Clarify the user and journey

Ask who performs the action, what they are trying to accomplish, and what successful completion means.

### 2. Identify durable business objects

Say:

> “The important durable entities appear to be the user, the main business record, the items related to it, the submission, and the result.”

Do not list tables yet. Start with concepts.

### 3. Describe relationships

Say:

> “A user can have many records. A record can contain many items, so I need a relationship between the record and its items. If users can collaborate, membership is a many-to-many relationship and needs its own representation.”

### 4. Describe state transitions

Say:

> “The record starts as a draft, can be submitted while it is valid, and becomes locked after submission. I would enforce those transitions in the backend rather than relying only on the UI.”

### 5. Discuss history

Say:

> “The current editable state may change, but the submitted state must remain historically correct. I would store an immutable snapshot or committed record for the completed action.”

### 6. Discuss important reads and writes

Mention the common operations:

```text
Create a record.
Update a draft.
Submit a record.
Load the current user’s records.
Load historical results.
List or rank records for a group.
```

Then explain that indexes and read models should support real query patterns.

### 7. State constraints and failure handling

Discuss duplicate submissions, unauthorized access, invalid input, concurrent updates, and partial failures.

### 8. Discuss scale only after correctness

Say:

> “I would start with a normalized source model. If a measured query becomes expensive, I would add a derived summary, cache, search index, or asynchronous process while keeping the source facts authoritative.”

## Example: a generic financial-product prompt

Suppose an interviewer asks about a product where a user completes onboarding, receives a recommendation, adds money, and views the result.

A clear response could be:

> “I’ll model the user, onboarding responses, recommendation, account, money movement, and historical valuations as separate concepts. I would separate the target recommendation from actual transactions because they have different lifecycles. Transactions should be immutable, while the recommendation or target allocation may change. Historical valuations should be stored as time-based records rather than overwritten. I would also make money movement idempotent so a retry cannot create a duplicate deposit.”

This demonstrates domain thinking, persistence boundaries, history, and reliability without requiring a perfect schema immediately.

## Example: a generic marketplace prompt

For a marketplace journey:

```text
Buyer searches → views item → adds to cart → checks out → receives order confirmation
```

The durable concepts might be:

```text
User
Product
Inventory
Cart
CartItem
Order
OrderItem
Payment
Shipment
```

A strong thought process would include:

> “The cart can change, but the order must preserve the exact items and prices at checkout. I would therefore snapshot order items and prices instead of calculating past orders from the current product table. Inventory updates must protect against overselling. Payment attempts need an idempotency key because external retries are possible.”

## What not to do in an interview

Avoid jumping directly to:

- A list of framework names.
- A large microservice diagram.
- Dozens of tables without explaining their meaning.
- A cache before describing the queries.
- A message queue before describing the business operation.
- A database choice without discussing consistency and access patterns.

These choices may become relevant, but they should follow the domain and workload discussion.

## A short answer you can memorize

> “I’ll start from the core user journey and identify the durable business facts it creates. Then I’ll define relationships, state transitions, and history requirements. I’ll create a small normalized model with constraints for correctness. After that I’ll list the most common queries and discuss indexes or derived data if needed. I’ll also cover authorization, duplicate requests, and what changes if the product scales.”

This answer gives you a reliable structure when you need to think aloud.

## Keep asking what must be true

For every important action, ask:

| Question | Example |
|---|---|
| What must be true before it starts? | The record belongs to the user and is still editable. |
| What changes when it succeeds? | A submission is created and the draft becomes locked. |
| What must never happen? | A user must not submit twice or edit another user’s record. |
| What must remain true later? | The historical result must not change after the source record changes. |
| What happens if the request is repeated? | The second request returns the existing result or is safely rejected. |

This turns vague architecture questions into concrete product rules.
