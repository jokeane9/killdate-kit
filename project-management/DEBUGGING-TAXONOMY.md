# Debugging Taxonomy

Read when something breaks and you're starting a debug session blind. This gives you a classification system so you're diagnosing the right layer before writing any fix.

---

## The first question: which layer broke?

Most debugging time is wasted fixing the wrong layer. Before touching any code, classify the bug.

| Layer | What it is | How it fails |
|---|---|---|
| **Data** | What's stored in the database | Missing row, wrong value, stale data, bad migration |
| **Model** | Business logic operating on data | Wrong query, incorrect calculation, missing null guard |
| **API / Loader** | Server-side data fetching and routing | Wrong response shape, missing auth check, redirect logic |
| **UI** | Frontend rendering | Component crash, wrong state rendered, missing empty state |
| **Integration** | Calls to external services | Auth failure, rate limit, schema mismatch with external API |
| **Config** | Environment variables, feature flags | Wrong value in env, flag in wrong state, missing secret |
| **Infrastructure** | Deployment, networking, containers | Container not running, wrong port, deploy failed silently |

---

## Bug ID prefixes

Use these when logging to `KNOWN-ISSUES.md`:

- `B-` — application logic (model, loader, action)
- `UI-` — frontend rendering
- `INT-` — integration (external API, webhook, third-party)
- `CFG-` — configuration or environment variable
- `INF-` — infrastructure (deploy, networking, container)
- `SCH-` — schema or data model
- `P-` — performance
- `SEC-` — security

---

## Diagnostic sequence

Run these in order. Stop when you find the layer:

1. **Check logs first.** What does the error actually say? Don't guess at the cause before reading the error.
2. **Check the data.** Is the expected row in the database? Does it have the expected values? Many "UI bugs" are missing data.
3. **Check the API/loader.** Is the server returning what you think it is? Check the actual response shape, not what you expect it to be.
4. **Check config.** Are environment variables set correctly? Is the feature flag in the right state?
5. **Check recent changes.** What changed in the last deploy? `git log --oneline -10`. Most bugs are caused by the last thing that changed.
6. **Isolate the layer.** Once you know which layer, narrow to which file and which function before writing any fix.

---

## Common failure patterns

**Symptoms without a clear error:** usually config or infrastructure. Check env vars, check the container is running, check the deploy completed.

**Works locally, fails in production:** almost always config or environment variable mismatch. Check what's different between local and prod.

**Worked yesterday, broken today:** check `git log` for what changed. Check if an external service had an incident.

**Intermittent / flaky:** usually a race condition, a timeout, or a rate limit. Don't write a retry loop until you've confirmed which one.

**Tests pass but production breaks:** mocked too much in tests. The mock diverged from reality. Fix the test to hit real infrastructure or add an integration test.

---

## Your project-specific failure modes

Add entries here as you discover failure patterns specific to your stack.

---

*Once classified: open `feature-builds/_playbook/BUILD-LEARNINGS.md` for stack-specific debug patterns, or `project-management/DEV-HEURISTICS.md` for domain-specific pitfalls.*
