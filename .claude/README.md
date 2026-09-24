# Claude Code configuration — GitHub Desktop

Project config (fork `samirhvbr/GITHUB_DESKTOP`). Stack: **TypeScript / Electron**, built with **yarn** (Node 24).

## Files

| File | Role |
|------|------|
| `settings.json` | **Active** profile (versioned). Permissions and effort — it does not choose the model. |
| `settings.local.json` | Local override (gitignored). Accumulates `allow` per session and **takes precedence** over `settings.json`. |

## Model — the user's choice, never the repository's

The model is chosen by the user with `/model`, per session, and a subagent inherits the
session's model. Nothing in this repository chooses it: `settings.json` carries no
`model`, `fallbackModel` or `availableModels`, and no `ANTHROPIC_MODEL`,
`ANTHROPIC_DEFAULT_*_MODEL` or `CLAUDE_CODE_SUBAGENT_MODEL` in its `env`. The stand-by
profiles that used to be copied over `settings.json` to swap models are gone, because
`/model` is what switches a model (repodocs ADR-027).

- **Effort `max` via the** `CLAUDE_CODE_EFFORT_LEVEL` **env var** — the JSON `effortLevel` field only accepts `low/medium/high/xhigh`, so `max` there is ignored.

## Permissions

- `defaultMode: plan`.
- **deny:** `rm -rf`, `git push --force/-f`, `git reset --hard`, `git clean -fd`, `curl|sh`/`wget|sh`, reading `.env*` and `*.pem`.
- **ask (confirms):** `sudo`, `git push`, `yarn add/remove/upgrade`, `yarn clean-slate`, `yarn rebuild-hard`.
- **allow:** read/edit/write, read-only git + `add`/`commit`, `yarn install/lint/prettier/markdownlint/test/compile/build/start/cli`, validations (`validate-changelog`, `validate-electron-version`, `validate-macos-version`), `node -c`, `npx tsc`.
