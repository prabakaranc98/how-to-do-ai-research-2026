# Background Index

**Sprint:** 002 - Deep Learning for Port-Hamiltonian and Compositional Systems

This folder collects the background needed to think clearly before implementing experiments.

The goal is not to become a full control theorist before building. The goal is to understand enough port-Hamiltonian structure, scientific ML methods, and compositional modeling to design a focused research sprint.

## Files

- `01-foundations.md`: conceptual foundation, port-Hamiltonian mental model, compositional systems, and neurosymbolic framing.
- `02-notation-and-modeling-map.md`: variables, equations, model families, and what each method can and cannot express.
- `03-methods-and-experiments.md`: experimental ladder, model losses, metrics, ablations, and failure analysis.
- `04-related-work-and-papers.md`: curated references, why each matters, and possible gaps.
- `05-benchmark-selection.md`: criteria and shortlist for choosing physical benchmarks that are both structurally useful and contemporary in AI/ML.
- `06-compositional-benchmarks.md`: component, graph, power-grid, physical-reasoning, and biological benchmark sources for lego-block-style systems.

## How To Use This Folder

Use this folder as the research scratchpad before implementation:

1. Read the foundations to understand the modeling lens.
2. Use the notation file to make the sprint measurable.
3. Use the methods file to choose the first small experiment.
4. Use the related-work file to avoid rediscovering known ideas.
5. Use the benchmark-selection file to keep the sprint connected to contemporary SciML and AI-for-science work.
6. Use the compositional-benchmarks file when the question is subsystem reuse, graph structure, ports, or lego-block composition.
7. Add notes as the sprint narrows from broad question to concrete experiment.

The rule:

**Preserve the big ambition, but make the first experiment small enough to finish.**
