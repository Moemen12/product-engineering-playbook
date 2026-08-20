# 2. MVP Scope and Core User Journeys

A product can have many user journeys. An MVP should usually begin with one **primary journey**: the shortest complete path through which a user receives the product’s main value.

## What a user journey is

A user journey describes what a user is trying to accomplish from entry to result.

```text
User enters
→ User performs the main action
→ Product produces a meaningful result
```

A journey is not the same thing as a feature. A journey normally combines several features.

| Concept | Meaning | Example |
|---|---|---|
| Product goal | The overall value the product provides. | Help a customer complete a task confidently. |
| Core journey | The complete path to the main value. | Start a request, provide information, submit it, receive confirmation. |
| Feature | One capability that supports the journey. | Form, search, payment, scoring, or notification. |
| Feature flow | The smaller interaction inside one feature. | Open form, enter fields, see validation, save progress. |
| Acceptance criterion | A condition that must be true for the journey to work. | A valid submission is saved and visible in the user’s history. |

## Choose the primary MVP journey

Ask these questions:

1. What is the user’s main goal?
2. What action creates the product’s central value?
3. What result tells the user that the action worked?
4. What is the shortest complete path from entry to that result?
5. Would the product lose its purpose if this path did not work?
6. Can the team demonstrate the path from beginning to end?

Write the journey in one sentence:

> A **[target user]** can **[main action]** and receive **[meaningful result]**.

For example:

> A new user can create a request, submit the required information, and receive a clear confirmation with the next step.

The exact interface can be designed later. At this stage, define the user’s goal, the necessary actions, and the expected result.

## Break the journey into steps

| Step | User action | Product responsibility |
|---|---|---|
| Enter | Start or access the product. | Provide onboarding or authentication. |
| Prepare | Provide information or choose options. | Show available choices and explain rules. |
| Decide | Make the main product decision. | Validate the decision and show its consequences. |
| Commit | Submit, purchase, publish, or confirm. | Enforce rules and persist the action. |
| Receive feedback | See the result. | Return a useful, understandable response. |
| Continue | Review history or start the next cycle. | Give the user a reason and a path to return. |

Not every product needs every label. The purpose is to make the path visible.

## Write acceptance criteria

Acceptance criteria turn a vague journey into testable behavior.

| Requirement | Acceptance condition |
|---|---|
| User can enter | A new user can reach the first meaningful screen. |
| Input is valid | Invalid values are rejected with an understandable message. |
| Main action works | A valid action creates the expected business result. |
| Result is visible | The user can see what happened and what to do next. |
| Data remains correct | Refreshing the page or reopening the product does not lose important information. |
| Access is safe | A user cannot read or change another user’s private data. |
| Failure is handled | A failed request does not create a misleading success state. |

## Scope the MVP

Use three lists. The names can be changed, but the separation is important.

| Build now | Build later | Do not build yet |
|---|---|---|
| Directly supports the primary journey. | Useful but not required to prove the main value. | Unconfirmed, high-risk, or unrelated work. |
| Required business rules. | Advanced convenience features. | Features added only because they sound impressive. |
| Essential empty, loading, and error states. | Additional user journeys. | Large infrastructure without a current need. |

A feature belongs in version one when removing it would prevent the user from receiving the main value or would make the product unsafe or unusable.

## A product can have many journeys

The first release has one primary journey for focus, not because the product permanently has only one journey. A mature product may later include journeys for onboarding, collaboration, payments, history, administration, support, invitations, and settings.

At a real company, map important journeys at a high level and then prioritize one or two for the first release using user value, business value, risk, dependency, and effort.

## Do not confuse journey definition with UI design

The journey defines **what the user must accomplish**. UX design explores **how the interface should help them accomplish it**.

The product statement might say:

> The user must be able to submit a valid request before the deadline.

The designer can explore whether that uses a wizard, a single form, a card layout, a table, or another interaction. The behavior is a requirement; the visual solution is still open.

A practical workflow is:

```text
Core journey
→ Rough wireframe
→ Design discussion
→ Technical discussion
→ Prototype
→ Feedback
→ Refined journey and UI
```

## Build one vertical slice

Once the primary journey is understood, build a complete thin version of it. The slice should include the user interface, server behavior, persistence, validation, authorization, and result.

A complete small path is more useful than many disconnected screens. It also exposes unclear requirements early, when they are cheaper to change.
