# Developing an Agents Market agent

This repo defines one agent for the Agents Market
(https://agents-market-v2-production.up.railway.app/). Listing fields live at the
root (`agent.yaml`, `DESCRIPTION.md`); the runtime prompt and skills live under
`agent/`. Everything is published manually by copy-pasting into the marketplace's
"New agent" form — there are no sync scripts and nothing here deploys anything.

## Sources of truth

- `agent.yaml` — all short listing + runtime fields, mirroring the platform form 1:1.
  Never invent fields that don't exist on the form.
- `agent/SYSTEM_PROMPT.md` — the system prompt, copied verbatim into the form.
- `DESCRIPTION.md` — the markdown description shown on the agent's page.
- `agent/skills/<name>/SKILL.md` — the PUBLISHED skill: one self-contained file, YAML
  frontmatter + markdown body, uploaded as-is via "+ Upload new skill". Frontmatter is a
  superset kept portable across Claude Code and IronClaw (`nearai/ironclaw`): `name` +
  `description` (required by both), plus optional IronClaw fields (`version`, `activation`,
  `requires`). `check.sh` lints the frontmatter and that a body is present.
- `agent/.claude/skills/<name>/SKILL.md` — the LOCAL wrapper Claude Code loads. 
  Holds Claude-only frontmatter (`allowed-tools`, `disable-model-invocation`, `argument-hint`, …) 
  so those never leak into the published frontmatter, then a single `@../../../skills/<name>/SKILL.md` 
  reference to the agentskill. Never published.
- `_`-prefixed dirs (e.g. `_template`) in both trees are scaffolds: checked but never
  published. Copy the pair to start a new skill.

## Invariants to keep while editing

- `agent/SYSTEM_PROMPT.md` ≤ 4096 bytes (`wc -c < agent/SYSTEM_PROMPT.md`)
- `tagline` ≤ 90 chars; `handle` matches `[a-z0-9_]{3,30}`
- `max_tool_rounds` and `max_concurrent_hires` in 1–64; `max_dispatch_time_sec` ≤ 3600
- every `agent/skills/<name>/SKILL.md` is one self-contained file: frontmatter (`name`
  matching `<name>` + `description`, optional IronClaw fields) then a non-empty markdown
  body — and has a matching wrapper at `agent/.claude/skills/<name>/SKILL.md` whose `name`
  matches and which references `@../../../skills/<name>/SKILL.md`. `check.sh` enforces both.
- `private_mcp_servers` in `agent.yaml` mirrors the form's "Private MCP servers"
  section: which connectors to tick, plus the name/url/auth needed to re-create
  each one under Building → Connectors. Tokens are entered on the platform only.
- No secrets in this repo; connector tokens are configured on the platform, not here.
  Local-dev exception: copy `agent/.mcp.json.example` to `agent/.mcp.json` and fill
  in your key so `cd agent && claude` gets real connector tools in the workbench.
  That file is gitignored — keep it out of git and never copy the key into any
  tracked file.
