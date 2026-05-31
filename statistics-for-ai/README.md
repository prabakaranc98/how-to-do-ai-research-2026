# Statistics for AI

This domain is for research that treats modern AI systems as statistical objects: systems whose behavior, preferences, uncertainty, failures, safety properties, and training data effects must be measured, modeled, and validated under uncertainty.

The core direction:

**Use statistical inference, algorithmic statistics, asymptotic thinking, uncertainty quantification, and experimental design to make AI/LLM behavior more measurable, reliable, and alignable.**

This is separate from [`../LLMs-cogs/`](../LLMs-cogs/), which stays focused on cognitive science, predictive processing, memory, reasoning, and agent behavior. This folder is the statistics layer around AI systems.

## What Belongs Here

- statistical foundations for LLM alignment and preference aggregation;
- uncertainty quantification, calibration, conformal prediction, and selective prediction;
- evaluation science for frontier models, safety tests, benchmark uncertainty, and capability thresholds;
- statistical alignment, preference collapse, reward-model validity, and RLHF/RLAIF audits;
- watermarking, provenance, detection, and hypothesis testing for generated text;
- data mixture optimization, scaling behavior, sampling, and training-data effects;
- bias, fairness, subgroup reliability, minority-preference preservation, and distribution shift;
- knowledge transfer, distillation, and teacher-student reliability as statistical estimation problems;
- statistically disciplined eval cards, model cards, ablations, confidence intervals, and failure analyses.

## Boundary With Other Domains

- [`../LLMs-cogs/`](../LLMs-cogs/) studies cognition, memory, predictive processing, reasoning, and agent behavior in language models.
- [`../mech-interpt/`](../mech-interpt/) studies internal representations, circuits, causal tracing, and representation-level interventions.
- [`../post-training-and-rl/`](../post-training-and-rl/) studies reward modeling, preference optimization, RL-style training loops, and reward hacking.
- `statistics-for-ai/` studies the inferential layer: what is being estimated, what uncertainty exists, what evidence supports a claim, and what guarantees or limits are possible.

## Initiatives

### Statistical Foundations for Frontier LLM Reliability

This initiative asks:

**Can statistical thinking turn LLM reliability, alignment, uncertainty, evaluation, and safety into measurable research objects instead of loose impressions?**

Start with [`sprints/000-statistical-foundations-for-frontier-llms/`](sprints/000-statistical-foundations-for-frontier-llms/).

## Sprint Index

- [`Sprint 000: Statistical Foundations for Frontier LLM Reliability`](sprints/000-statistical-foundations-for-frontier-llms/): a first research sprint that maps the field, narrows the statistical surface, and defines a minimal experiment around uncertainty-aware LLM evaluation.

