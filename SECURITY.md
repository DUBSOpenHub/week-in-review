# Security Policy

## Supported Versions

This project is a single-file Copilot CLI agent. Security fixes are applied to `main`; users get them by re-running the one-click installer.

| Branch / version | Supported |
| ---------------- | --------- |
| `main` (current) | :white_check_mark: |
| Older snapshots  | :x: |

## Reporting a Vulnerability

We take security seriously. If you discover a vulnerability in this project — including the installer scripts, the agent prompt, or anything that could expose user data — please report it responsibly.

**Please do NOT create a public GitHub issue for security vulnerabilities.**

### How to Report

Use [GitHub's private vulnerability reporting](https://github.com/DUBSOpenHub/copilot-week-in-review/security/advisories/new) to submit a report directly on this repository.

Include the following information:
- Description of the vulnerability
- Steps to reproduce the issue
- Potential impact
- Any suggested fixes (optional)

### Response Time

We commit to acknowledging security reports within **48 hours** of submission.

After the initial response, we will:
- Confirm the vulnerability and determine its severity
- Work on a fix and coordinate disclosure timing
- Ship a patch and publish a public advisory
- Credit you for the discovery (unless you prefer to remain anonymous)

## Scope

In-scope for security reports:

- The agent prompt in `agents/week-in-review.agent.md` (e.g., prompt-injection patterns that cause exfiltration of user data)
- The installer scripts (`quickstart.sh`, `install.sh`) — including the `curl | bash` flow served from `raw.githubusercontent.com`
- Anything in this repo that could compromise a user who installs it

Out of scope (please report to GitHub directly):

- Vulnerabilities in the GitHub Copilot CLI itself
- Vulnerabilities in the underlying AI models

## Privacy Notes

This agent is intentionally **tool-light**. It uses exactly two host capabilities, and only when the user opts in:

- **`ask_user`** — for the 2-question intake (source + audience). Local to the Copilot session.
- **`gh` (shell)** — only when the user selects the *"pull from my GitHub"* source. The agent runs read-only `gh search prs` and `gh search issues` against the GitHub API using the user's existing local `gh` authentication. No writes are performed. No other shell commands are executed. No third-party endpoints are contacted by this agent.

Any data the user pastes into a Copilot CLI session is subject to the [GitHub Copilot privacy terms](https://github.com/features/copilot). When GitHub-pull mode is used, the GitHub API calls flow under the user's own `gh` auth and are subject to standard [GitHub API terms](https://docs.github.com/en/rest). Nothing additional is sent anywhere by this agent.

Thank you for helping keep Copilot Week in Review and its users safe.
