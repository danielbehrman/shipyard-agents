# Shipyard Agents

Global agent team and execution framework — reusable across all projects.

## What This Is

Shipyard Agents is the global execution infrastructure for all of Dan's projects. It defines the agent team, pipeline rules, and project brief template that Claude Code uses across every project.

## Contents

- `CLAUDE.md` — Global agent team definition. Copy to `~/.claude/CLAUDE.md` on any machine to activate.
- `BRIEF-TEMPLATE.md` — Reusable project brief template. Copy to any project repo as `BRIEF.md` and populate before starting a Claude Code session.

## How It Works

### Agent Team
Every Claude Code session reads `~/.claude/CLAUDE.md` automatically. The agent team runs as:

**PM → Architect → Dev → Reviewer → QA → Dan**

All gates are hard. Dan only sees work that has passed Reviewer and QA. UI components always require Dan sign-off.

### Project Brief
Every project has a `BRIEF.md` in its root. Claude Code reads it on session start, surfaces a briefing to Dan, and waits for confirmation before any work begins.

Briefs are generated and updated via Claude.ai planning sessions and committed to the project repo before execution starts.

### Phase Gates
Phases complete when all features pass QA. Dan reviews and confirms before the next phase begins. Nothing starts automatically.

## Setup on a New Machine

1. Clone this repo
2. Copy `CLAUDE.md` to `~/.claude/CLAUDE.md`
3. Install Claude Code: `npm install -g @anthropic-ai/claude-code`
4. Install Superpowers: `/plugin install superpowers@claude-plugins-official`

## Starting a New Project

1. Create a new GitHub repo for the project
2. Clone it locally
3. Copy `BRIEF-TEMPLATE.md` into the project root as `BRIEF.md`
4. Run a planning session in Claude.ai to populate the brief
5. Commit `BRIEF.md` to GitHub
6. `cd` into the project folder and run `claude` to start building
