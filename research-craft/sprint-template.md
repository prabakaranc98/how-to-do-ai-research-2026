# Research Sprint Template

Use this template when starting a new sprint. Copy it into the sprint folder as `README.md` or split it into `hypothesis.md`, `methods.md`, `evals.md`, and `report.md` as the sprint matures.

## Sprint Title

**Domain:**  
**Sprint number:**  
**Status:**  
**Date started:**  

## 1. One-Line Thesis

State the research claim in one sentence.

## 2. Why This Matters

- What important problem does this touch?
- Why is the problem attackable now?
- What would become possible if the hypothesis were true?
- Who would care about the result?

## 3. Marr-Level Framing

### Behavioral Level

What observable behavior will be measured?

- Tasks:
- Success metrics:
- Failure modes:
- Expected empirical bounds:

### Representational Level

What internal states, concepts, or geometries might explain the behavior?

- Candidate representations:
- Layers/tokens/positions to inspect:
- Geometry to measure:
- Probes or feature analyses:

### Computational / Mechanistic Level

What mechanism or control process might transform representation into behavior?

- Candidate circuits or attention paths:
- Activation patching or causal tracing plan:
- Steering / ReFT / RepE intervention:
- Expected causal effect:

## 4. Integrated Stack Position

Which parts of the integrated AI research stack does this sprint touch?

- Representation learning:
- World models / predictive state:
- Mechanistic interpretability:
- Steering / control / ReFT / RepE:
- Post-training / reward modeling / RL:
- Agent behavior / cognitive control:
- Evidence and validation:

## 5. Hypotheses

Write falsifiable claims before running the experiment.

1. 
2. 
3. 

## 6. Minimal Experiment

Define the smallest version that can teach something real.

- Model:
- Dataset or environment:
- Baseline:
- Intervention:
- Evaluation split:
- Compute budget:

## 7. Methods

- Data construction:
- Model setup:
- Probe or representation analysis:
- Steering / activation patching / ReFT method:
- Ablations:
- Controls:

## 8. Evaluation

Behavioral metrics:

- 

Representational metrics:

- 

Mechanistic/control metrics:

- 

Validity checks:

- 

## 9. Failure Taxonomy

Name the likely ways the sprint can fail.

- Behavioral failure:
- Representation failure:
- Mechanistic failure:
- Control failure:
- Evaluation failure:

## 10. Research Log Discipline

Every session should record:

- what changed;
- what surprised you;
- what failed;
- what result you do not trust yet;
- what the next smallest useful experiment is.

## 11. Sprint-End Decision

Choose one:

- **Continue:** evidence is promising and the next experiment is clear.
- **Pivot:** the direction is interesting, but the current framing is weak.
- **Pause:** more intuition, reading, or tooling is needed.
- **Stop:** the hypothesis is not useful, important, or attackable.

## 12. Final Report Shape

The final `report.md` should include:

- thesis;
- method;
- behavioral result;
- representational result;
- mechanistic/control result;
- failures and limitations;
- what changed your mind;
- reusable artifacts;
- next experiment.
