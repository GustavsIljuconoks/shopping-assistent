# Dark Factory Initial Benchmark

Date: 2026-05-09
Repo: [cursor-hack/dark_factory](https://github.com/cursor-hack/dark_factory)
Jira project: [KAN — CursorHackatonRiga](https://darkfactoryhack.atlassian.net/jira/software/projects/KAN/boards/1)
Auth model: built-in `GITHUB_TOKEN` only (no personal PAT, no machine user)

## End-to-end smoke results

| # | Jira | GH run | Conclusion | Runtime | Dispatch latency | PR |
|---|---|---|---|---|---|---|
| BOOT-1 | KAN-7 (PR mode, Task) | [25597920082](https://github.com/cursor-hack/dark_factory/actions/runs/25597920082) | ✅ success | ~60s | <10s | [#1](https://github.com/cursor-hack/dark_factory/pull/1) |

## What this proves

- Jira manual-button (Flow "Dark Factory dispatch") fires `repository_dispatch` to GitHub Actions
- Workflow `Handle Jira Manual Trigger` (`jira-dispatch.yml`) runs to completion on `ubuntu-latest`
- Claude Code runs (Opus + high effort), session caching works
- Output is committed to a per-issue branch (`tdf/<issue-key-lower>`) and a PR is opened
- A bot comment is posted back on the Jira issue with the run + PR link

## Targets

| Metric | Target | BOOT-1 |
|---|---|---|
| Dispatch latency (Jira click → GH run starts) | < 30s | <10s ✓ |
| Pipeline runtime per ticket | < 5 min | 60s ✓ |
| Pipeline cost per ticket | < $0.50 | (TBD, see Claude usage) |

## Operational notes

- **Token strategy**: `GITHUB_TOKEN` only. Org `cursor-hack` has Workflow permissions = `Read and write` and `Allow GitHub Actions to create and approve pull requests` enabled.
- **Secrets** (5 total in `cursor-hack/dark_factory`): `JIRA_BASE_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN`, `CLAUDE_CODE_OAUTH_TOKEN` plus the auto `GITHUB_TOKEN`.
- **Issue type → mode mapping**: see `architecture.md` §2.2.
- **No ngrok / no self-hosted runner / no Supabase**: deploy-on-merge workflow is committed but inert until those are wired.
