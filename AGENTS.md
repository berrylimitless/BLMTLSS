# AGENTS.md — Berry Limitless AI Team Protocol

Read this before touching the repo. Last verified: 2026-10-10.

## Roles
| Agent | Owns | Does NOT do |
|---|---|---|
| Founder (Dante) | All approvals: deploys, prices, Stripe, phone numbers, SMS sends, spend | — |
| Claude Code | Repo integration, code review, tests, PRs, approved deployments | Deploy, change prices, or send messages without founder approval |
| Muse AI | Voice agent design and deliverables (scripts, flows, config specs) | Treated as having NO live link to this repo until verified (see INTEGRATIONS.md) |
| ChatGPT | Requirements, research, product review | Same: no verified live link. Works through issues/PR comments pasted or connected by founder |

## Collaboration protocol
1. **One source of truth:** this repo. Actionable work = GitHub Issues (template: `Task`). No second task board.
2. **Branch per task:** `task/<TASK-ID>-<slug>`. Never commit to `main` directly. One owner per branch.
3. **File ownership:** an issue lists the files/paths it touches. If two open issues list the same path, the second waits (label `blocked`) or coordinates in the issue. No silent overwrites.
4. **Handoff:** the finishing agent (a) pushes its branch, (b) opens/updates a PR, (c) posts test evidence, (d) appends an entry to `docs/ai-team/HANDOFFS.md`, (e) sets the next owner via the issue `owner:` label/assignee and an @-mention. See HANDOFFS.md.
5. **Status honesty:** `STATUS.md` only states what was verified, with date and evidence. "Unverified" is a valid status.
6. **No claim of agent-to-agent messaging** until the path is configured and tested end to end (INTEGRATIONS.md records the test).

## Hard rules
- No secrets in the repo, issues, PRs, docs or chat. Reference env var names only. Never ask the founder to paste secrets into chat.
- **This repository is public.** No customer data, prices not yet published, transcripts, phone numbers, or credentials.
- Protected (do not change without explicit founder approval recorded in DECISIONS.md): homepage gold master, existing Stripe prices, AI OS checkout, member authentication, Limitless World invite-only controls, production deployments.
- No unattended code execution or auto-deploy until the founder authorizes it in DECISIONS.md.
- No SMS/calls/purchases/charges/Stripe product creation without founder approval AND consent/compliance safeguards in place.
