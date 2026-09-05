# Netlist Decomposition agent guidance

Inherit the project guidance from `../AGENTS.md`. Before work here, read
`../MANIFESTO.md`, `../ONTOLOME.md`, local `ONTOLOME.md`, local `README.md`, and
local `unit.toml`, then inspect the relevant implementation and tests.

This unit owns explainable structural-to-functional rules, composition passes,
tagging sets, and dependency metadata over the `spice-canonical` circuit
contract. Keep raw syntax parsing, simulation-input editing, and behavioral
claims unsupported by structural evidence outside this boundary. Update the
local ontology when this being changes; place a changed cross-unit canonical
contract in the closest containing ontology.

## Where to read, and what to trust

Three surfaces, deliberately separate. Know which one you are in.

| Surface | Where | Maintained against the code? |
| --- | --- | --- |
| **Self-study** — evolving understanding, including commitments, evidence, assumptions, and open questions | `ONTOLOME.md` | **Yes.** Refine it when work yields useful insight; update commitments explicitly when they change. |
| **Documentation** — what the rules do and how the paper was read | `docs/`; built by `python composition.py docs` from the repository root | **Yes.** Everything under `docs/` is published to the Sphinx site. |
| **Design record** — the handoff and the executed refactor plan | `design/` | **No.** Written on a date, never edited to stay true, never published. |

The rule that follows: **do not cite a `design/` file as evidence of current
behaviour, and do not update one to match the code.** Both files there name
modules that no longer exist (`hl3.py`, `hl4.py`, `HIERARCHY_LEVELS`,
`diode_connected_mos`). If something in there is still right and load-bearing,
promote it into `docs/` or `ONTOLOME.md`. `design/README.md` says what each is.

**`docs/rule-dependencies.md` is generated — never edit it by hand.** It is
rendered from the catalog in `src/netlist_decomposition/dependencies.py` by
`scripts/generate_decomposition_dependencies.py`. Change the catalog, then
regenerate; a hand edit is reverted by the next run and fails `--check`.

## Standing constraints

Rescued from the 2026-07-13 plan in `design/plan-composition-passes.md`, which
is otherwise spent. These are still in force:

- **Never run pytest, and never run `scripts/verify_ota_decomposition.py`.** The
  user runs both. The verifier additionally needs a sibling
  `../sky130-analog-workspace` that is not part of this repository. `README.md`
  documents the commands for the user; that is not permission for an agent.
- The permitted checks after touching code are `python -m py_compile` on every
  touched file and, when the dependency catalog changed,
  `python scripts/generate_decomposition_dependencies.py --check`.
- Do not render figures or PDF pages from the paper.
- Do not revert the `spice_canonical` refactor.
