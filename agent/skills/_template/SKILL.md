---
# ── Frontmatter: linted by ./check.sh, kept portable across Claude Code + IronClaw ──
# Required by BOTH Claude Code and IronClaw:
name: _template                  # rename to your skill (lowercase-kebab slug matching its directory)
# description: use the directive-description pattern (see CLAUDE.md → Building skills):
#   <factual lead>. ALWAYS invoke this skill when <triggers>. Do not <direct action> — use this skill first.
description: <verb-led one-liner on what the skill does>. ALWAYS invoke this skill when the user asks to <trigger 1>, <trigger 2>, or <trigger 3>. Do not <the direct action this skill replaces> — use this skill first.

# IronClaw-specific (ignored by Claude Code, see .claude/rules/skills.md in nearai/ironclaw):
version: 0.1.0
activation:
  keywords:                      # ≤ 20 (IronClaw silently drops the rest)
    - example
  patterns: []                   # ≤ 5 regexes (IronClaw silently drops the rest)
  exclude_keywords: []
  tags: []
  max_context_tokens: 2000
requires:
  bins: []                       # binaries that must be on PATH
  env: []                        # env vars that must be set
  config: []                     # config file paths that must exist
  skills: []                     # companion skills to chain-load

# Claude Code-specific (ignored by IronClaw), uncomment if the skill needs it:
# allowed-tools: Read Grep
---

# Example skill

> This is the `_template` scaffold. Copy the whole `_template/` directory to a new
> `agent/skills/<your-skill>/`, rename `name:` above to match the directory, and replace
> everything below. Frontmatter and body live together in this one file — that's exactly
> what the marketplace upload, Claude Code, and IronClaw each consume.

## What this skill does

Describe the procedure, checklist, or domain knowledge the agent should apply. Keep it
focused; long reference material can go in sibling files and be linked from here.

## Steps

1. First step.
2. Second step.
3. Submit the deliverable via `MARKET_SUBMIT_DELIVERABLE` in the configured output format.
