# Product Engineering Playbook

A simple, reusable guide for starting a product from an unclear idea and turning it into a small, useful, and reliable first release.

This playbook is written for engineers, team leads, product managers, designers, and anyone who needs to think clearly before building. It is intentionally **project-agnostic**: replace the examples with the real product, users, rules, and constraints of each new project.

## The whole process in one sentence

> Understand the problem, identify the user’s most important journey, define the smallest useful version, model the business domain, design the data that must be remembered, build one complete path, validate it, and expand carefully.

## How to use this repository

Start with the [Product Brief](templates/product-brief.md). Fill in only what you know. Mark uncertain information as **assumed** or **unknown** instead of treating guesses as requirements.

Then use the [MVP and User Journey Guide](docs/02-mvp-and-user-journeys.md) to choose the first complete user experience. Use the [Domain and Database Design Guide](docs/03-domain-and-database-design.md) once the core journey and business rules are clear enough. The [Interview Thinking Guide](docs/04-interview-thinking-framework.md) helps you explain the same reasoning aloud during technical interviews.

## Recommended order

| Stage | Main question | Main output |
|---|---|---|
| 1. Product discovery | What are we building, for whom, and why? | Product brief and assumptions |
| 2. User journey | What is the shortest path to real user value? | Core journey and acceptance criteria |
| 3. MVP scope | What must exist now, later, or not yet? | Build-now and out-of-scope lists |
| 4. Domain model | What business concepts and rules exist? | Domain objects, relationships, and states |
| 5. Data model | What must the system remember? | Persistent data model and constraints |
| 6. Vertical slice | Can one user complete the full path? | Working end-to-end feature |
| 7. Validation | Does it work for real users and failure cases? | Feedback, fixes, and next priorities |

## Key principles

### Start with uncertainty

Do not begin by choosing technologies or drawing tables from vague ideas. First clarify the business goal, target user, main user action, release type, constraints, and success criteria.

### Use one primary journey for the first release

A real product can have many user journeys. The first release should focus on the one complete journey that proves the product’s main value. This does not limit the future product; it gives the team a clear starting point.

### Keep the UI flexible while the behavior is clear

Product thinking defines what the user must be able to accomplish and which rules must be respected. UI and UX design decide how that experience should look and feel. The exact layout can change without changing the product goal.

### Model the domain before the screens

Business objects such as users, orders, accounts, projects, messages, or submissions should come from the product’s meaning and rules—not from the names of pages in the interface.

### Store durable business facts

Persist information that the system must remember, protect, query, audit, or use later. Do not create database records for temporary UI state such as open modals, selected tabs, or loading indicators.

### Design the smallest useful data model

Start with the facts and relationships required by the core journey. Normalize the source data first. Add cached, aggregated, or denormalized data only when a real query or performance requirement justifies it.

### Build one vertical slice

A vertical slice proves the whole path together: user interface, server behavior, database, validation, authorization, and result. This is more valuable than building many disconnected screens or database tables.

### Preserve history when history matters

Current state can change. Historical facts often should not. When a user submits, pays, publishes, or completes an important action, decide whether an immutable snapshot or event record is required.

### Explain tradeoffs honestly

A small prototype does not need to claim production scale. State what is implemented now, what is intentionally simplified, and how the design could evolve if the product grows.

## Repository structure

```text
.
├── AGENTS.md
├── README.md
├── docs/
│   ├── 01-product-discovery.md
│   ├── 02-mvp-and-user-journeys.md
│   ├── 03-domain-and-database-design.md
│   ├── 04-interview-thinking-framework.md
│   └── 05-failure-cases-and-tradeoffs.md
└── templates/
    ├── product-brief.md
    └── database-design-notes.md
```

## Quick start for a new project

Copy the product brief and fill in the project-specific information. Do not try to answer every question immediately. Mark unanswered questions as **unknown**, identify the person who can answer them, and avoid building around them until they are validated.

After the core journey is clear, write the journey’s acceptance criteria. Then identify the durable business objects and rules involved in that journey. Only after that should you create the first database model and implementation plan.
