# 1. Product Discovery: Start by Removing Uncertainty

Before building, make sure the team understands what it is building and why. The first task is not to choose a framework or draw database tables. The first task is to reduce uncertainty about the product.

## Questions to ask first

| Question | Why it matters |
|---|---|
| What exactly are we building? | Prevents the team from building different interpretations of the same idea. |
| Who is the first target user? | Different users have different needs, habits, and expectations. |
| What problem or desire are we addressing? | Keeps the product focused on user value. |
| What should the user accomplish in the first few minutes? | Helps identify the first meaningful experience. |
| What type of release is this? | A prototype, private test, and public product need different levels of reliability and polish. |
| What must be demonstrated to users, customers, or investors? | Prevents work that does not support the immediate goal. |
| What is explicitly out of scope? | Protects the timeline and reduces unnecessary complexity. |
| How will we know the first version is successful? | Creates measurable acceptance criteria instead of vague expectations. |
| What data or integrations are required? | Reveals dependencies, permissions, licensing, and operational risks. |
| Who can make product decisions? | Prevents the team from waiting or building from conflicting opinions. |

## Record assumptions clearly

Early information is often incomplete. Do not silently turn guesses into requirements. Create a short assumptions list and label each important point.

| State | Meaning | How to act |
|---|---|---|
| Confirmed | The product owner or an approved source explicitly supports it. | Build around it if it belongs to the current scope. |
| Assumed | It is a reasonable interpretation but has not been confirmed. | Record it and ask for validation. Avoid expensive work based only on it. |
| Unknown | There is not enough information to make a safe decision. | Keep the decision open and do not build around it yet. |
| Rejected | The team has explicitly decided not to support it. | Keep it out of the current release. |

A useful assumption has an owner and a next action. For example:

| Assumption | State | Owner | Next action |
|---|---|---|---|
| Users will compete in private groups. | Assumed | Product owner | Confirm whether private groups are needed in version one. |
| The first release will use real-world data. | Unknown | Founder and legal adviser | Confirm data rights and source before integration work. |
| Payments are not part of the first release. | Confirmed | Product owner | Keep payments outside the current scope. |

## Define the product in one paragraph

Before writing detailed requirements, write a short product statement:

> We are building **[product]** for **[target user]** so they can **[main outcome]**. The first release will prove this through **[core experience]**. It will not include **[important exclusions]**.

If the team cannot agree on this paragraph, the product is not ready for detailed implementation planning.

## Separate product decisions from implementation decisions

Product decisions describe user value and behavior. Implementation decisions describe how the team will deliver that behavior.

| Product decision | Implementation decision |
|---|---|
| Users can submit an application. | Use a server endpoint and persist the application record. |
| A submitted application cannot be edited. | Enforce a state transition in the backend and database. |
| Users can see their history. | Store historical records and query them by user and date. |
| Results should appear quickly. | Choose an appropriate query, index, cache, or background process. |

Do not solve an implementation problem before confirming that the product behavior is actually required.

## What to produce at the end of discovery

The output does not need to be a large document. A useful first version contains:

1. The product statement.
2. The target user.
3. The main problem or desired outcome.
4. The first core journey.
5. Confirmed, assumed, unknown, and rejected decisions.
6. Version-one success criteria.
7. The explicit out-of-scope list.
8. Open questions and the person responsible for answering them.

This is enough clarity to begin journey design, domain modeling, and technical exploration without pretending that every future decision is already known.
