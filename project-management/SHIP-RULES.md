# Agile Playbook

Read before any feature build, deploy decision, or versioning call. This is the rulebook — the patterns you build by and the rules that govern how you ship.

---

## The single invariant

**V(N+1) = V(N) + exactly one surgical change.**

Every artifact in the pipeline tests against this. The lock tests it (invariants inventory). The mock tests it (side-by-side diff). The runbook tests it (tasks mapped to the slot). The code tests it (FILES check). Production tests it (post-deploy comparison).

If a build can't be expressed as a single surgical change, it's two builds. Split it.

---

## The three patterns

These underpin every build decision. Reason from them — don't just follow them.

### Parallel Change (Expand–Contract)

Add new alongside old. Migrate traffic to new. Delete old when migration is complete. Never flip everyone at once.

Use when: replacing an existing surface, changing a data contract, migrating a schema.

The three moves:
1. **Expand** — add the new thing alongside the old. Both exist simultaneously.
2. **Migrate** — route traffic to the new thing incrementally. Feature flag per user or per environment.
3. **Contract** — delete the old thing when legacy traffic is zero and the sunset criterion is met.

[Reference: martinfowler.com/bliki/ParallelChange.html](https://martinfowler.com/bliki/ParallelChange.html)

### Strangler Fig

Wrap the old thing. Route one piece at a time to the new thing. Let the old hollow out. Never rewrite everything.

Use when: replacing a subsystem, migrating an API, replacing a UI section that has users on it right now.

The old system stays in production until the new one has handled every case. Rollback is routing traffic back — no migration, no incident.

[Reference: martinfowler.com/bliki/StranglerFigApplication.html](https://martinfowler.com/bliki/StranglerFigApplication.html)

### Canary Release

Deploy to one before deploying to all. Small controlled exposure catches problems before full rollout.

Use when: shipping anything to production. Every deploy is a canary before it's a full rollout.

Enable the feature for one user or one account. Walk the flow manually. Check logs. If clean for a defined window: expand. If not: flag flip, immediate rollback, no redeploy.

[Reference: martinfowler.com/bliki/CanaryRelease.html](https://martinfowler.com/bliki/CanaryRelease.html)

---

## The N-1 rule

Production runs version N. Version N-1 must stay bootable. N-2 and older get deleted.

Every feature that introduces temporary code — flags, shims, compatibility fallbacks — ships with:
- A **sunset criterion**: the measurable condition under which temporary code gets deleted
- A **kill date**: the hard date by which the criterion must be met or the code gets deleted anyway

Both are written in the pre-build lock before any code is written. Not after. Before.

---

## Versioning

Three axes that must never be conflated:

| Axis | What it versions | Example |
|---|---|---|
| Product version | User-visible product releases | V1, V2, V3 |
| Code version | Code changes following semver | 2.0.0, 2.1.3, 2.1.4 |
| Schema version | Database table/column versions | users_v2, settings_v1 |

**MAJOR** — breaking change. Parallel Change required. New version runs alongside old until migration is complete.

**MINOR** — additive change. Non-breaking. Can ship without parallel run.

**PATCH** — bug fix or wording change. No downstream impact.

**NO BUMP** — data-only change with frozen code. No version increment needed.

When in doubt, ask: does downstream code break if it reads the new version without knowing about the change? If yes: MAJOR. If no: MINOR or lower.

---

## When a feature build starts

1. Read `marketing/canonical/MARKETING-TRUTH.md` and `marketing/canonical/BEHAVIOR-SPEC.md` — confirm the proposed change is consistent with both
2. Pick the pattern (Parallel Change / Strangler Fig / Canary)
3. Fill in `project-management/PRE-BUILD-LOCK-TEMPLATE.md` — the lock is where the abstract rules become concrete decisions for this build
4. No code until the lock is signed off

---

## Your project-specific decisions

Add entries here as you make permanent versioning or shipping decisions.

---

*This playbook is the rulebook. The pipeline is `PRE-BUILD-LOCK-TEMPLATE.md`. The lock is the per-feature form. All three work together — none stands alone.*
