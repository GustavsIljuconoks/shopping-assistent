# Dark Factory — Agent Instructions

See `CLAUDE.md` for the dispatch-flow operating context (Jira ticket handling, `KIND`, env vars, etc.).
See `README.md` for CLI usage (`plan`, `apply`).

## Cursor Cloud specific instructions

### Project overview

Node.js/TypeScript CLI + GitHub Actions workflows that convert markdown product requirements into Jira execution plans. No build step — TypeScript is executed directly via `tsx`. The update script runs `npm ci` to keep dependencies current.

### Running checks

| Check | Command |
|-------|---------|
| Tests | `npm test` |
| Typecheck | `npm run typecheck` |

### Key caveats

- **No build step**: The project uses `tsx` for direct TypeScript execution (`npx tsx scripts/dark-factory.ts ...`). There is no compilation output.
- **Jira credentials not needed for local dev**: `plan` and `test` commands work fully offline. Only `apply --approve` requires `JIRA_BASE_URL`, `JIRA_EMAIL`, and `JIRA_API_TOKEN` env vars.
- **ESM only**: The project uses `"type": "module"`. All test files are `.mjs`. Import paths must include extensions.
- **Node.js 22 required**: The VM image ships with v22. The built-in `node --test` runner is used (no Mocha/Jest).
