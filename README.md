# Shipyard Agents

A structured agent pipeline for building software with Claude Code. This repo contains the global conventions, templates, and project instructions that govern how every Shipyard project gets built.

---

## The Core Idea

Most people using AI to build software are prompt-and-pray. They write a vague spec, hand it to an agent, and hope for the best. Shipyard is the opposite: a disciplined operating system for shipping products with agents — where the human is in the loop at the right moments, and agents handle the rest.

The key insight: **the bottleneck isn't the AI. It's the lack of structure around it.**

---

## The Pipeline

Every project runs through the same agent sequence in Claude Code:

```
PM → Architect → Dev → Reviewer → QA → "Project Owner"
```

Each gate is hard. No agent proceeds without the prior agent's explicit pass. Failures route back to Dev — never to "Project Owner". "Project Owner" only sees work that has cleared Reviewer and QA. UI components always require "Project Owner" sign-off regardless of QA status.

These are conventions enforced by discipline, not automation. That's intentional — the structure is the system.

---

## What's in This Repo

```
shipyard-agents/
├── CLAUDE.md            # Global agent instructions — loaded into every Claude Code session
├── BRIEF-TEMPLATE.md    # Source-of-truth template for all project scoping
└── README.md            # This file
```

Individual projects live at `~/Shipyard/[project-name]/` and each contain their own `BRIEF.md` and `CLAUDE.md`. The global files here are inherited by all of them.

---

## BRIEF.md

`BRIEF.md` is the source of truth for all build work. Every feature must include:

- **Goal** — what this solves and for whom
- **Acceptance criteria** — specific and testable
- **Constraints** — what's explicitly out of scope
- **Definition of done** — no ambiguity

Features are ordered by highest value, lowest complexity first. BRIEF.md is updated at the end of every planning conversation and after any mid-phase scope change — before any agent session resumes.

---

## Stack Defaults

Unless `BRIEF.md` specifies otherwise:

| Layer | Default |
|---|---|
| Frontend | Next.js |
| Database | Supabase (Postgres) |
| Hosting | Vercel |
| Repo | GitHub |

Any deviation is documented by the Architect in `BRIEF.md` before Dev begins.

---

## Session Protocol

Every Claude Code session starts the same way:

1. Agent reads `BRIEF.md`
2. Agent surfaces a briefing: what's being built, estimated complexity, risks and blockers
3. "Project Owner" confirms explicitly before any work begins

No implied approvals. No auto-starting. No scope creep.

---

## Phase Gates

Phases complete when all features in that phase pass QA. "Project Owner" reviews a phase summary and explicitly confirms before the next phase begins. "Project Owner" initiates — nothing starts automatically.

When starting a new phase, the agent asks for a summary of prior completions and learnings before defining new features.

---

## Why This Exists

I spent years as a PM working with engineering teams. One thing that became obvious: you can't — and shouldn't try to — replicate a team of different-minded individuals with a single agent.

There's a reason architects, QA engineers, and reviewers exist in the real world. They're not redundant — they're adversarial in the best sense. They have different goals, different definitions of success, and different failure modes they're trained to catch. An architect who writes the spec and the dev who builds it are supposed to think differently. A reviewer who didn't write the code catches things the author is blind to.

A single agent has none of that tension. It will happily build on top of its own assumptions, compound its own gaps, and deliver something that passes a naive read but fails in the ways that matter. By the time you catch it, you've built several layers on top of a wrong turn.

Shipyard replicates the structure that makes real teams trustworthy. Each agent in the pipeline has a specific mandate and a hard gate before the next one starts. Gaps get called out before they get built on. Assumptions get surfaced before they compound. I only get pulled in when something actually requires a human — and when I do, the work that reaches me has already been stress-tested by agents whose job is to find the problems, not paper over them.

---

## Setup on a New Machine

1. Clone this repo
2. Copy `CLAUDE.md` to `~/.claude/CLAUDE.md`
3. Install Claude Code: `npm install -g @anthropic-ai/claude-code`

---

## Starting a New Project

1. Create a new GitHub repo for the project
2. Clone it locally into `~/Shipyard/[project-name]/`
3. Copy `BRIEF-TEMPLATE.md` into the project root as `BRIEF.md`
4. Run a planning session in Claude.ai to populate the brief
5. Commit `BRIEF.md` to GitHub
6. `cd` into the project folder and run `claude` to start building

---

## Changelog

### v2 — System hardening based on live project feedback
*Derived from audits across three active Shipyard projects*

**What changed and why:**

**Assumption tracking** — BRIEF.md now has a dedicated Assumptions section where unvalidated decisions are flagged with ⚠️. Previously, assumptions and confirmed decisions looked identical on paper — until one of them broke mid-build. The Architect is now required to resolve all ⚠️ items before Dev starts.

**Architect gate hardened** — The Architect must now validate external dependencies (APIs, data sources, quota limits) before committing to an implementation plan, and must lock all stack and architecture decisions before Dev begins. In practice these were being resolved on the fly in conversation, which meant Dev was building against open questions.

**QA expanded** — QA now tests all variants and permutations, not just the happy path. Data quality assumptions are also in scope — if a feature depends on an external data source, QA verifies the data actually covers the intended use case. User-visible bugs were reaching "Project Owner" that a more thorough QA pass would have caught.

**Session start tightened** — The briefing now explicitly surfaces unresolved assumptions and Carry Forward items from prior phases. Answering clarifying questions is no longer treated as confirmation to start — "Project Owner" must confirm explicitly after the briefing is complete.

**Phase gate completion requirements** — A phase no longer closes with a vague summary. The completion record must include a feature-by-feature pass/fail/deferred breakdown, all deferred items with severity and priority, known regressions, and a first draft of the next phase BRIEF. That last one is a blocking deliverable — phase doesn't close without it.

**Carry Forward** — Deferred items from a closed phase are now first-priority in the next session briefing, not background items. High-severity bugs may not be deferred past the phase in which they are found without explicit "Project Owner" approval.

**In-session decisions must be committed** — Any decision made during a session that isn't in BRIEF.md must be captured before the session ends. Decisions that live only in chat are not decisions.

**BRIEF-TEMPLATE additions** — Stack table (with confirmed vs. assumption status), Assumptions block, Carry Forward table, and Completion Record section added. The template now reflects the full lifecycle of a phase, not just the pre-build spec.

---

### v1 — Initial release
Core pipeline (PM → Architect → Dev → Reviewer → QA → "Project Owner"), BRIEF.md as source of truth, hard gate structure, stack defaults, session protocol.

---

## Related

- [Claude Code](https://claude.ai/code) — where the pipeline runs
- Inspired by agent patterns in BMAD and similar frameworks, but purpose-built for this workflow
