# Netlist Decomposition

This unit recognizes transistor stacks, bias structures, differential pairs,
stages, and related functional blocks over the canonical circuit contract owned
by the sibling `spice-canonical` distribution.

For a source checkout, install the dependency and this unit explicitly:

```bash
python -m pip install -e ../spice-canonical -e .
python -m pytest -q tests
python scripts/generate_decomposition_dependencies.py --check
```

The optional OTA verifier consumes a sibling SKY130 workspace:

```bash
python scripts/verify_ota_decomposition.py [workspace-dir]
```

## Documentation

[`docs/`](docs/index.md) is the guide, built into the project's Sphinx site by
`python composition.py docs` from the repository root. Start at
[Abel et al. (2021) alignment](docs/paper-alignment.md) for exactly which paper
rules are implemented and which are not; the
[rule dependency table](docs/rule-dependencies.md) is generated from
`netlist_decomposition.dependencies` and must never be edited by hand.

Two neighbouring surfaces are deliberately not part of that site.
[`ONTOLOME.md`](ONTOLOME.md) states the contracts this unit currently guarantees
and its exclusions, and is where a change to a contract must be recorded.
[`design/`](design/README.md) holds the handoff and the plan that produced the
current pipeline — written on a date, not maintained against the code, and never
to be cited as evidence of how it now behaves.
