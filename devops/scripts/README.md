# DevOps scripts

Deploy, validation, and health check scripts. Add yours here as you build them.

Suggested contents as the project matures:

| Script | What it does |
|---|---|
| `preflight.sh` | Pre-deploy validation — task definitions, env vars, dependencies |
| `canary.sh` | Post-deploy end-to-end smoke test for one user/account |
| `health-check.sh` | Nightly or on-demand infrastructure health check |
| `seed-staging.sh` | Seed staging with production-like data for testing |

Scripts here should be idempotent and safe to run repeatedly. Every script should have a `--dry-run` flag or equivalent where applicable.
