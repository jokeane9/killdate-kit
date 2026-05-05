# killdate-kit

The forkable companion to [killdate.dev](https://killdate.dev) — a starter kit for shipping SaaS with AI agents.

Fork this. Open it in Claude Code. Fill in the brackets. Start building.

The blog is the scaffolding. This is the tool.

## What's here

| File / Folder | What it is |
|---|---|
| `CLAUDE.md` | Persistent context for Claude Code. Skeleton — fill in your product, stack, and deploy target before your first session. |
| `.cursor/rules/` | Three rules enforcing the Claude Code / Cursor boundary. These travel project to project unchanged. |
| `project-management/` | State files (log, roadmap, known issues, stack, canonical, architecture) + process templates (pre-build lock, deploy checklist, MVB scope). Stubs — you populate as you work. |
| `feature-builds/` | Per-feature build artifacts. `_playbook/` has the runbook template and build learnings. `_completed/`, `_iterations/`, `_ux-reference/` mirror the Shelf folder pattern. |
| `devops/` | Deploy and validation scripts. Populate as you build them. |

## First session

1. Fork this repo
2. Open it in Claude Code
3. Fill in everything marked `[BRACKETS]` in `CLAUDE.md`
4. Fill in `project-management/CANONICAL.md` — your product truth (this is the most important file)
5. Fill in `project-management/ARCHITECTURE.md` — your stack and how layers connect
6. Run your first session using `docs/mvb-scope.md` as the build scope template

## The methodology

Everything here is documented at [killdate.dev](https://killdate.dev). Start with:

- [What is agentic orchestration?](https://killdate.dev/posts/00-what-is-agentic-orchestration)
- [CLAUDE.md is your OS](https://killdate.dev/posts/02-claude-md-is-your-os)
- [Claude Code vs Cursor](https://killdate.dev/posts/05-claude-code-vs-cursor)
- [Runbooks](https://killdate.dev/posts/06-runbooks)
- [Pre-build lock](https://killdate.dev/posts/04-pre-build-lock)

## The layering

Not everything here is equal weight. Understand the layers before you start:

- **Cursor rules** — fill these in for real. They're generalizable and travel project to project. The PROHIBITED discipline is what makes agentic work safe on a live codebase.
- **Process templates** (`docs/`) — fill these in for real per feature. The five-block runbook structure doesn't change between a Shopify app and a SaaS dashboard.
- **CLAUDE.md** — skeleton with bracketed placeholders. Fill in your product. Leave the structure and rules intact.
- **`project-management/` stubs** — referenced in CLAUDE.md so Claude Code knows they exist. You populate them as you work — starting session one.

## A note on scope

This is a mild guide, not a framework. It doesn't replace developer knowledge. It gives your AI tools the context they need from day one, and gives you a folder structure you harden over time. The cursor rules and runbook template get sharper with each project. The CLAUDE.md gets more accurate as you fill it in. That's the compounding value.
