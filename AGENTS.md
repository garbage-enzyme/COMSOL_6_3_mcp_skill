# AGENTS.md — Codex / cross-tool entry point

This repository ships an **agent skill** at:

```
skills/comsol-63-operations/SKILL.md
```

It is a focused instruction document (with YAML frontmatter) that teaches coding agents how to drive **COMSOL Multiphysics 6.3** through the [comsol MCP server](https://github.com/garbage-enzyme/COMSOL_Multiphysics_MCP_6_3_Calibrated) (MPh 1.3.1 standalone / `clientapi`), and how to write or fix code in `src/tools/` when the clientapi wrapper's API differs from the direct `com.comsol.model.Model` API.

## When to read it

Read `skills/comsol-63-operations/SKILL.md` **before** doing any of these:

- Driving COMSOL 6.3 via the comsol MCP server (any `comsol_*` tool call).
- Writing or modifying code in `C:\Users\陆星\COMSOL_Multiphysics_MCP\src\tools\`.
- Debugging errors like `No matching overloads`, `Operation_cannot_be_created_in_this_context`, or `'ComponentGeomListClient' object is not subscriptable` on 6.3 standalone.

The skill's frontmatter `description` is the matching signal for agents that support skill auto-loading (opencode, Claude Code). Codex reads AGENTS.md directly, so: **treat the skill file as required reading for COMSOL-related tasks.**

## Why this skill exists

Under `mph.Client(cores=...)` (MPh 1.3+ standalone), `model.java` returns `com.comsol.clientapi.impl.ModelClient` — a wrapper whose method overloads differ from the direct `com.comsol.model.Model` the upstream MCP code was written against. The skill documents every mismatch found + fixed in the companion fork, plus 6.3-specific traps (Electrostatics `fsp1` FreeSpace ignoring material ε_r, Block boundary numbering, `Terminal` V0 unreliable, `(1[V])^2` expression syntax, missing `m.study()`).

Verified end-to-end: parallel-plate capacitor **C = 1.8593794414 pF** vs theory 1.8593794407 pF (err 4 × 10⁻¹⁰ pF).

## Companion code

The calibrated MCP server source lives at: https://github.com/garbage-enzyme/COMSOL_Multiphysics_MCP_6_3_Calibrated
