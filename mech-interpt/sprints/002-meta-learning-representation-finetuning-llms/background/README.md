# Background Index

**Sprint:** 002 - Representation Engineering, ReFT, and Meta-Learning in Small Language Models

This folder collects the background needed to turn representation-level reliability into concrete experiments.

The goal is not to review every fine-tuning method. The goal is to understand enough about probes, ReFT, RepE, concept activation vectors, steering vectors, activation patching, calibration, and task adaptation to design a focused sprint.

## Files

- `01-foundations.md`: conceptual foundation, representation engineering, ReFT, reasons-responsive behavior, and why hidden states matter.
- `02-methods-map.md`: prompt baselines, LoRA/adapters, ReFT-style interventions, RepE steering, concept vectors, targeted activation patching, and meta-learned policies.

## Files To Add

- `03-evaluation.md`: calibration, selective prediction, abstention, contradiction sensitivity, false-premise resistance, and retention checks.
- `04-related-work-map.md`: related areas and papers to study before implementation.

## How To Use This Folder

Use this folder as a research scratchpad before implementation:

1. Define the epistemic behavior being trained.
2. Pick one small model and one small dataset.
3. Choose one ordinary fine-tuning baseline.
4. Choose one representation-engineering method: ReFT, RepE steering, concept activation vectors, or targeted activation patching.
5. Decide what evidence would show a real improvement rather than cosmetic caution.

The rule:

**Do not trust behavior alone when the claim is mechanistic. Inspect the representation.**
