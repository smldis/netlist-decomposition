# Design record — working material, not documentation

Nothing in this directory is published. `composition.py docs` stages only
`netlist-decomposition/docs/`, so these files never reach the Sphinx site, and
that is the point: they are correspondence and instructions, written on a date,
about a tree that has since moved. **They describe what was asked for, not what
the code now does.** For what the code now does, read
[`docs/paper-alignment.md`](../docs/paper-alignment.md) and `ONTOLOME.md`.

Kept in the repository rather than deleted because the reasoning behind a rule
outlives the session that wrote it — in particular why the pipeline stopped
being one stage per paper hierarchy level. The paths and module names inside
these files are as they were written; several no longer exist.

## What is here

| File | What it is | Status |
| --- | --- | --- |
| `functional-decomposition-claude-handoff.md` | The agent handoff that commissioned paper-aligned HL1 classification and Equation 9 stacks, with its acceptance criteria and its own SKY130 OTA expectations. | Executed. Everything it required is implemented and described in `docs/paper-alignment.md`; the file names it lists (`hl3.py`, `hl4.py`, `diode_connected_mos`) are gone. |
| `plan-composition-passes.md` | The 2026-07-13 plan that replaced `HIERARCHY_LEVELS` with `KIND_LEVELS` plus `COMPOSITION_PASSES`, and merged `hl3.py` and `hl4.py` into `stages.py`. | Executed. Read it as a record of intent, never as a backlog. Its "Constraints (standing, do not violate)" section is the one part still in force; it now also lives in `AGENTS.md`. |

## The rule

A file here is never edited to stay true. If something in it is now wrong, that
is expected — it was written before the change. If something in it is still
*right and load-bearing*, it belongs in `docs/` or in `ONTOLOME.md`, not here.
