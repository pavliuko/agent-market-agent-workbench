---
# Claude Code wrapper — LOCAL DEV ONLY, never published. Keep Claude-specific frontmatter
# here so it never pollutes the portable agent-skill frontmatter under agent/skills/.
name: _template                  # rename to match the agent skill's directory under agent/skills/
# description drives Claude Code routing in the workbench — describe the REAL skill using
# the directive-description pattern (see CLAUDE.md → Building skills), and keep it in sync
# with the published skill's description at agent/skills/<name>/SKILL.md.
description: <verb-led one-liner on what the skill does>. ALWAYS invoke this skill when the user asks to <trigger 1>, <trigger 2>, or <trigger 3>. Do not <the direct action this skill replaces> — use this skill first.
# Optional Claude Code-only fields (safe here — never shipped to the marketplace):
# allowed-tools: Read Grep
# disable-model-invocation: true
# argument-hint: <city> <headcount>
---

@../../../skills/_template/SKILL.md

<!--
  This is the _template wrapper scaffold. To add a skill named e.g. `venue-finder`:
    1. cp -r agent/skills/_template        agent/skills/venue-finder
    2. cp -r agent/.claude/skills/_template agent/.claude/skills/venue-finder
    3. set `name: venue-finder` in BOTH files, and point the reference below at it.

  The published skill (frontmatter + body) lives at agent/skills/<name>/SKILL.md — that
  is the ONLY file uploaded to the marketplace. This wrapper is never published.

  NOTE: Claude Code does not auto-expand `@` inside a SKILL.md (that is a CLAUDE.md-only
  feature), so the line above is a pointer, not an injection — open the referenced file
  to read/run the real body.
-->
