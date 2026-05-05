# [AppName] — Architecture

> Fill this in before your first deploy. Read it before any deploy after that. The "Current deployment status" section needs updating after every deploy.

---

## Stack

- **Frontend:** [framework — e.g. Remix, Next.js, SvelteKit]
- **Backend:** [language and runtime — e.g. Node 22, Python 3.12]
- **Database:** [e.g. PostgreSQL on RDS, PlanetScale, Supabase]
- **Deploy target:** [e.g. AWS ECS, Vercel, Railway, Fly.io]
- **CI/CD:** [e.g. GitHub Actions]
- **Key services:** [e.g. Stripe for billing, Resend for email, Cloudflare for DNS]

---

## How the layers connect

[3–5 sentences or a short ASCII diagram. What calls what. Where data flows. What the seams are between layers.]

```
[optional ASCII diagram]
```

---

## Data model

Key tables or collections. Not a full schema — just enough to understand what the core entities are and how they relate.

| Entity | Description | Key fields |
|---|---|---|
| [table] | [what it represents] | [id, important columns] |
| [table] | [what it represents] | [id, important columns] |

Full schema: `[path/to/schema.sql or schema file]`

---

## Current deployment status

Update this after every deploy.

- **Production URL:** [URL]
- **Staging URL:** [URL or "none yet"]
- **Last deploy:** [YYYY-MM-DD]
- **Current version:** [version or commit SHA]

---

## Hard constraints

Things that can't change without significant work or careful migration. List them explicitly so future builds don't accidentally violate them.

- [constraint 1 — e.g. "users table cannot be dropped — OAuth tokens reference it"]
- [constraint 2]

---

## Known failure modes

What breaks and how. Updated when incidents happen.

| Layer | Common failure | How to detect | Recovery |
|---|---|---|---|
| [layer] | [failure mode] | [signal] | [steps] |
