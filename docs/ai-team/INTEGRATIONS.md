# Integrations & connection status (2026-10-10)

Rule: "Connected" requires a recorded end-to-end test. Otherwise it is "Unverified".

| Integration | Status | What is actually possible | Limitation / next test |
|---|---|---|---|
| GitHub (this repo) via Claude Code | Connected (read/write branches, issues) | Branch push, issues, PRs | Repo webhooks API blocked (403) from this session; use Actions or repo settings by founder |
| GitHub `Berry-Limitless/empire-web` | NOT connected | — | Founder must grant this Claude session's GitHub app access to the repo |
| Vercel | Connected (read) | Teams, projects, deployments, domains | Disconnects intermittently; no deploys without approval |
| Stripe | Needs founder authorization in claude.ai connector settings | — | Do not create products/prices regardless |
| Twilio / Retell | Not checked | — | Voice source needed first; secrets stay in env vars |
| Muse AI | Unverified | Unknown whether it has any API/webhook/GitHub access | Test: ask Muse to commit a file to a branch or comment on an issue. Until it passes, Muse works via founder-delivered files committed by Claude Code |
| ChatGPT | Unverified | Unknown whether a GitHub connector is enabled on founder's account | Same test. Until it passes, treat as human-mediated |
| Claude Code cloud sessions | Available | `create_session`, scheduled routines, PR subscriptions via Claude connectors | Needs founder opt-in before any unattended/scheduled run |

## Handoff transport (what we use now)
GitHub Issue + branch + PR + `HANDOFFS.md` entry. Notification = assignee/@mention/label. This is **asynchronous and repo-mediated**, not live agent-to-agent messaging.

## Possible automation (needs founder authorization; none enabled)
- GitHub Action on label `handoff:<agent>` that comments @mention and updates status.
- Claude Code routine / PR-activity subscription to wake a session on review comments.
