# Deploy checklist

Run through this every time you ship to production. No skipped steps. The cost of skipping is always higher than the cost of running it.

---

## Before deploying

- [ ] All tests pass locally: `[YOUR TEST COMMAND]`
- [ ] No uncommitted changes: `git status`
- [ ] On the correct branch: `git branch --show-current`
- [ ] Pre-build lock invariants verified — every surface in the invariants table has been manually checked
- [ ] Any pending database migrations identified: check `db/migrations/` (or equivalent) for unapplied files

---

## Database migrations

**If the deploy touches the schema:**

- [ ] Migration script reviewed line by line
- [ ] Migration tested on staging with production-like data volume
- [ ] Rollback script written: "To roll back, run `[specific command]`"
- [ ] Migration runs **before** new code deploys — never after

**If no schema changes:** write `N/A` and continue.

---

## Staging verification

- [ ] Deploy to staging first: `[YOUR STAGING DEPLOY COMMAND]`
- [ ] Walk through the affected flow manually on staging
- [ ] Automated test suite passes against staging
- [ ] Invariants from pre-build lock verified on staging (not just locally)

---

## Production deploy

- [ ] Push to main / trigger deploy: `[YOUR DEPLOY COMMAND]`
- [ ] Monitor CI/CD pipeline — confirm it completes without errors
- [ ] Confirm new version is running: `[YOUR HEALTH CHECK URL OR COMMAND]`

---

## Canary

- [ ] Enable the new feature for one user or one test account only
- [ ] Walk through the affected flow manually as that user
- [ ] Check logs for errors: `[YOUR LOG COMMAND]`
- [ ] Confirm OUT list items from pre-build lock are unchanged
- [ ] If clean for 15–30 minutes: expand rollout

---

## Rollback procedure

If something is wrong after deploy:

1. **Feature flag off** (if applicable) — no redeploy needed, this is instant
2. If no flag: `git revert HEAD && git push origin main`
3. If schema was migrated: run rollback migration **before** reverting code
4. Document in `project-management/KNOWN-ISSUES.md` immediately

---

## Post-deploy

- [ ] Log the deploy in `project-management/_log.md` — what shipped, what was verified
- [ ] Check kill dates: open any active `feature-builds/*/PRE-BUILD-LOCK.md` files and verify no kill dates have passed
- [ ] Update `project-management/ROADMAP.md` if this completes a milestone
