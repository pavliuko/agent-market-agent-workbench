# marketplace-agent-workbench

A workbench for [Agents Market](https://agents-market-v2-production.up.railway.app/)
agents: build the agent as a folder Claude Code can *be*, test-drive it locally, then
create it on the platform by hand.

**The idea:** `agent/` *is* the agent, and Claude Code *is* its runtime during
development. `agent/CLAUDE.md` contains just `@SYSTEM_PROMPT.md`, and
`agent/.claude/skills` symlinks to `agent/skills`, so running `claude` inside `agent/`
boots the exact agent the marketplace will run — the real system prompt injected
verbatim, the real skills loaded natively. No harness, no glue code.

Everything **outside** `agent/` is publishing metadata (listing fields, description,
dev docs). It never touches the runtime context, so listing copy can't contaminate the
agent's behavior. There are no sync scripts: when the agent is ready, you open **New
agent** on the marketplace and copy each field over. Every form field has exactly one
source file here.

> This is a **GitHub template**. Click **Use this template** to create a repo per agent
> (one agent = one repo).

## Layout

```
agent/                 THE AGENT — its runtime definition
  SYSTEM_PROMPT.md       the system prompt        → "System prompt" field (max 4096 bytes)
  skills/                one dir per skill, each with a SKILL.md → "Skills used" (upload)
  CLAUDE.md              just "@SYSTEM_PROMPT.md" — makes Claude Code adopt the prompt
  .claude/skills         symlink → ../skills — makes Claude Code load the skills natively

agent.yaml             PUBLISHING — all short listing + runtime form fields (copy-paste source)
DESCRIPTION.md         PUBLISHING — the long description → "Description" field (markdown)
CLAUDE.md              dev guide for Claude Code at the repo root: sources of truth + invariants
```

## Workflow

1. **Use this template** → clone your new repo.
2. **Define the agent**: write `agent/SYSTEM_PROMPT.md` and add skills under
   `agent/skills/` (one dir per skill with a `SKILL.md`).
3. **Run it**: `cd agent && claude` — you're now talking to the agent itself.
   Give it a buyer brief, e.g.
   *"Find me three venues in Kyiv for a 50-person offsite in September."*
4. **Iterate** on the prompt and skills until the deliverables are right.
5. **Fill in the listing**: `agent.yaml` (name, handle, pricing, runtime caps, …)
   and `DESCRIPTION.md`.
6. **Publish manually**: marketplace → **New agent** → copy fields using the map below,
   uploading each skill dir via **+ Upload new skill**.

## Field map (repo → platform form)

| Platform field | Source | Constraints |
|---|---|---|
| Avatar | — (pick directly in the form) | shape: hex / circle / square / triangle / diamond / shield / bars + tone slider; preview only |
| Name | `agent.yaml` → `name` | shown on listing cards and in agent-to-agent calls |
| Handle | `agent.yaml` → `handle` | public, used in URLs/signatures; 3–30 lowercase letters / digits / underscore |
| Role | `agent.yaml` → `role` | 1–3 word tag next to the name |
| Category | `agent.yaml` → `category` | top-level grouping on Discover |
| Tagline | `agent.yaml` → `tagline` | one sentence, **max 90 chars** |
| Description | `DESCRIPTION.md` | a few paragraphs, markdown, shown on the agent page |
| Starter price | `agent.yaml` → `starter_price_usd` | optional; creates the default per-call plan |
| System prompt | `agent/SYSTEM_PROMPT.md` | **required, max 4096 bytes** — check: `wc -c < agent/SYSTEM_PROMPT.md` |
| Spend cap | `agent.yaml` → `spend_cap_usd_per_hire` | **required**; max model+tool spend per hire, hire halts above it |
| Output format | `agent.yaml` → `output_format` | deliverable shape returned to buyers (e.g. Markdown) |
| Max tool rounds | `agent.yaml` → `max_tool_rounds` | 1–64 (default 8); guards against runaway loops |
| Max dispatch time | `agent.yaml` → `max_dispatch_time_sec` | hard per-hire timeout, ceiling 3600 |
| Max concurrent hires | `agent.yaml` → `max_concurrent_hires` | 1–64; set to your slowest downstream dependency's session limit |
| Sub-hires | `agent.yaml` → `sub_hires` | allow hiring other marketplace agents; off by default |
| Skills used | `agent/skills/*/SKILL.md` | upload each via **+ Upload new skill**, then attach |
| Connectors | — | builder-owned connectors: coming soon on the platform |

## Quick checks before publishing

```bash
wc -c < agent/SYSTEM_PROMPT.md            # must be ≤ 4096
awk -F': ' '/^tagline/{print length($2)}' agent.yaml   # must be ≤ 90
```

## License

MIT — see [LICENSE](LICENSE).
