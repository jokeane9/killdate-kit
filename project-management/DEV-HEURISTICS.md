# Dev Heuristics

Indexed by domain. "If you're touching X, remember Y." Not permanent architecture decisions (those are in `STACK.md`) — working knowledge that saves time re-debugging the same thing.

Read the relevant section before diving into a task in that domain.

---

## Format

Each entry: the domain, the heuristic, and why it matters.

---

## Authentication

[Add entries here as you learn them.]

> Example: "Every authenticated route must call `authenticate()` before any database reads. Missing this doesn't throw — it silently serves unauthenticated data."

---

## Database

[Add entries here as you learn them.]

> Example: "Migrations run before code deploys, never after. A migration that runs after the new code is live creates a window where the new code is serving requests against the old schema."

---

## API / External integrations

[Add entries here as you learn them.]

> Example: "Always check the actual API response shape against what the docs say. APIs diverge from their docs. Trust what you observe, not what the docs claim."

---

## Deploy / Infrastructure

[Add entries here as you learn them.]

> Example: "A deploy that completes without error doesn't mean the new container started correctly. Always check the health endpoint after deploy, not just the CI status."

---

## Testing

[Add entries here as you learn them.]

> Example: "If a test is flaky, it's almost always a timing issue or a shared state problem between test cases. Fix the isolation before fixing the test."

---

## [Your stack — add domains as needed]

---

*This is a living document. Add entries when you re-learn something that should have been written down. The test: "would I have saved 30 minutes if I'd read this first?" If yes, write it.*
