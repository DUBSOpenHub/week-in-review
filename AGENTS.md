# Agents

## Overview

This repo ships a single GitHub Copilot CLI agent — **week-in-review** — that turns messy weekly notes into a polished, GitHub-issue-ready status update. That's the whole project: one agent file plus an installer.

## Available Agents

### week-in-review

- **Purpose**: A warm, sharp writing partner. Takes either rough weekly notes (bullets, brain dumps, pasted PR titles, raw meeting notes) **or** the user's last 7 days of GitHub activity pulled directly via `gh`, and returns a single, ready-to-paste status update formatted as GitHub-flavored markdown. Output is structured for a GitHub issue body: suggested title, suggested labels, `##` headings, and `- [x]` / `- [ ]` task lists that render as clickable checkboxes. The tone is layered per audience: **boss** (outcomes-focused), **team** (collaborative), or **yourself** (raw log).
- **Usage**: From any GitHub Copilot CLI session — **no repo required**:
  ```
  @week-in-review
  ```
  The agent opens with a two-question intake — *source* (paste notes / pull from GitHub / both) and *audience* (boss / team / yourself) — then drafts the update. Follow up with tweaks: *"make it more casual"*, *"punchier"*, *"switch audience to team"*, *"open this as an issue in `myorg/team-updates`"*.
- **Model**: Default model in your Copilot CLI session.
- **Location**: `agents/week-in-review.agent.md` → installs to `~/.copilot/agents/week-in-review.agent.md`.
- **Tools used**:
  - `ask_user` — for the 2-question intake (source + audience), when available in the host session.
  - `gh` (shell) — only when the user selects the *"pull from GitHub"* source. Runs read-only `gh search prs` / `gh search issues` against the authenticated user's recent activity. No writes.
  - Otherwise, the agent is text-in / markdown-out.

## Install

One-click (recommended):

```bash
curl -fsSL https://raw.githubusercontent.com/DUBSOpenHub/copilot-week-in-review/main/quickstart.sh | bash
```

Or via the Copilot CLI:

```
/agents add DUBSOpenHub/copilot-week-in-review
```

## Design Principles

- **Ultra simple.** One agent file, ~115 lines of prompt. If a change makes the agent file longer, default to "no" unless the win is clear.
- **Issue-ready by default.** Output must always paste cleanly into a GitHub issue body — `##` headings, `- [ ]` task lists, no fenced wrappers around the whole body.
- **No invention.** The agent must never add work the user didn't mention (or that GitHub didn't surface). Omit empty sections entirely rather than padding.
- **Tool-light, not tool-free.** The agent uses exactly two host tools when offered: `ask_user` for the 2-question intake, and `gh` (read-only `search`) for the optional GitHub-pull source. No writes, no other shell, no arbitrary network. Anything beyond that bar is out of scope.
- **Audience presets, not personas.** Three tone layers (boss / team / yourself) over one shared structure. Don't fork the format per audience; only the wording shifts.
- **Conversational refinement over options.** Don't expose flags or CLI args. Let users ask in natural language for "more casual" / "shorter" / "switch audience to team" / "open it as an issue".

## Remixing this agent

The agent prompt lives in `agents/week-in-review.agent.md`. To customize for your team:

1. Fork the repo (or just copy the file).
2. Open `agents/week-in-review.agent.md` in the [GitHub Copilot app](https://github.com/features/copilot) and ask it to make the changes you want — different sections, different tone, different default labels.
3. Reinstall locally to test: `./install.sh`.
4. Open a PR if your change is useful to others.

That's how this agent was built; that's the cleanest way to evolve it.
