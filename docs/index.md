# Netlist Decomposition

Recognize what a transistor netlist *is for* — which devices form a stack, a
bias, a mirror, a differential pair, an amplification stage — from structure
alone, with the evidence for every answer attached to it.

This unit reads canonical `Circuit` and `Device` objects from the sibling
`spice-canonical` distribution and returns overlapping `BlockTag`s. Each tag
carries its member devices, their roles, the nets involved, its hierarchy level,
and the rule that produced it, so a match can be argued with rather than
believed.

```python
from netlist_decomposition import decompose, suppress_false_stacks

tags = decompose(circuit, vdd_nets={"VDD"}, vss_nets={"VSS"})
for tag in suppress_false_stacks(tags):
    print(tag.kind, tag.level, tag.rule)
```

Three ideas carry the rest of these pages:

- **Structure only, never behaviour.** Everything here is read off connectivity
  and declared device polarity. Nothing is inferred from waveforms, from device
  names, or from a simulation.
- **Supply nets are declared, never guessed.** `decompose` takes `vdd_nets` and
  `vss_nets`; without them the rules that need a rail simply find less.
- **Tags overlap and are not a partition.** One device is normally a
  `normal_transistor`, a one-device `transistor_stack`, and a member of larger
  blocks at the same time. That overlap is the paper's model, not a defect.

## What is implemented, and what is not

The reference is Abel, Neuner and Graeb (2021), *A Functional Block
Decomposition Method for Automatic Op-Amp Design*. **This is not the full paper
decomposition, and does not claim to be.**

Implemented: hierarchy level 1 (Eq. 7/8), transistor stacks (Eq. 9), voltage and
current bias and current mirrors via Algorithm 1, the Eq. 19 deletion of
irrelevant multiple assignments, differential pairs and cascode pairs
(Eq. 13–17), the analog inverter (Eq. 18), non-inverting and inverting
transconductances, loads via Algorithm 3, stage biases, amplification stages and
circuit bias via Algorithm 2 (Eq. 30–37), and the compensation and load
capacitors (Eq. 38/39). A source follower is recognized as a deliberate
extension with no paper counterpart.

Not implemented: op-amp classification above hierarchy level 4 — the paper's
final composition step; the exact load definitions Eq. 24/25 and the multi-bias
Eq. 26 structure; bias-dependent stack and current-mirror variant names
(cp, mp1, mp2, vr1, vr2, ccm, 4cm, wcm, wscm, iwcm); complementary and CMFB
transconductances over cascoded pairs; cascoded source followers; and Section
4.6 false-stack suppression beyond the differential-pair cases.
[Abel et al. (2021) alignment](paper-alignment.md) states each of these
precisely, including the two places where the implementation reads the paper's
prose rather than its equations.

## Start here

| If you want to | Read |
| --- | --- |
| know exactly which paper rules run, and how each was read | [Abel et al. (2021) alignment](paper-alignment.md) |
| know what a tag kind depends on, and in which pass it appears | [Functional decomposition rule dependencies](rule-dependencies.md) |
| know the conventions — member ordering, polarity, declared rails | the *Conventions* section of [Abel et al. (2021) alignment](paper-alignment.md) |
| know what the unit refuses to do | the *Exclusions* section of `ONTOLOME.md`, next to this directory's parent |

## Development state

`ONTOLOME.md` records this unit's development state as **prototype**, and that
is meant literally: the rules here are runnable evidence for the hypothesis that
explicit, inspectable structural evidence can support useful functional
interpretation. Matches, false positives, missed structures and dependency
friction are all expected to change rules, passes, the sibling canonical
contract, or the unit's boundary. The tag taxonomy is an implementation, not a
settled claim of completeness.

The corresponding promise is narrow and worth stating: a tag is only emitted
where its full structural predicate holds. Where the predicate is weaker than
the paper's, the kind says so in its name — `differential_pair_candidate` is a
candidate, and the *Exact versus candidate names* section lists every such case.

```{toctree}
:maxdepth: 2
:caption: Netlist Decomposition

paper-alignment
rule-dependencies
api
```
