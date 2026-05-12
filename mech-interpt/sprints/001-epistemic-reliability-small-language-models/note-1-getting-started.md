# Note 1: Getting Started

**Sprint:** 001 — Epistemic Reliability in Small Language Models  
**Date:** May 2, 2026  
**Status:** Framing note

## 1. Starting Point

This sprint begins from a simple but important question:

**Can small language models behave reliably under uncertainty?**

The goal is not to prove that small models are generally intelligent or to compare them casually against larger models. The goal is to study whether small models can make better decisions about when to answer, retrieve, abstain, ask for clarification, or escalate.

This matters because small models are likely to become practical components in real systems:

- local assistants;
- edge and mobile AI;
- enterprise copilots;
- private domain-specific models;
- tool-using agents;
- routing systems;
- low-latency decision systems.

The central constraint is that small models cannot know everything. So the important research surface is not only answer quality. It is **epistemic behavior**: how the model behaves when knowledge, evidence, or confidence is limited.

## 2. Working Research Direction

Possible title:

**Epistemic Reliability in Small Language Models**

Working thesis:

**Small language models should not be judged only by whether they can answer. They should be judged by whether they know how to behave under uncertainty.**

This creates a research intersection across:

- **Statistics:** uncertainty, calibration, conformal prediction, selective prediction, error control, confidence intervals.
- **Epistemology:** knowledge, belief, evidence, justification, uncertainty, defeaters, disagreement, warranted claims.
- **Small Language Models:** constrained-capacity models where reliability, cost, privacy, latency, and routing matter.
- **Cognitive Science / Surprise:** belief updating, surprise, evidence sensitivity, metacognition, conformity, and response to contradiction.
- **Preference Optimization / Reward Hacking:** cases where RLHF, RLAIF, or preference tuning may reward answers that sound helpful, agreeable, confident, or satisfying while weakening truth sensitivity.

The philosophical terms are not decoration. They are intuition lenses. The experiments still need to be measurable.

## 3. Am I Using "Epistemology" Correctly?

Yes, with care.

In philosophy, epistemology studies knowledge: what it means to know something, how beliefs are justified, what counts as evidence, and when uncertainty should change belief.

For LLMs, the claim should be behavioral rather than mystical. A small model may not "know" in the human sense. But we can study its **epistemic behavior**:

- Does it answer when it should not?
- Does it guess while sounding confident?
- Does it update when given evidence?
- Does it recognize contradiction?
- Does it defer when evidence is insufficient?
- Does it conform to misleading user pressure?
- Does it optimize for approval, agreement, or plausible-sounding helpfulness instead of warranted claims?
- Does it retrieve when its internal knowledge is weak?
- Does it escalate when the query is outside its capability?

This gives a rigorous version of the question:

**Can we measure and improve the decision policy around a small model's claims?**

## 4. Key Uncertainty Lenses

This sprint can begin by distinguishing uncertainty types:

- **Epistemic uncertainty:** the model lacks knowledge, capability, or sufficient evidence.
- **Aleatoric uncertainty:** the question or world state is inherently ambiguous.
- **Semantic uncertainty:** different generated answers mean materially different things.
- **Retrieval uncertainty:** retrieved evidence is irrelevant, weak, noisy, or contradictory.
- **Calibration uncertainty:** model confidence does not match empirical correctness.
- **Distribution-shift uncertainty:** the query falls outside the model's training or evaluation regime.
- **Tool uncertainty:** a tool call, parser, API, or retrieved source may be wrong.
- **Conformity uncertainty:** the model agrees with a user's premise, social pressure, or majority framing instead of evidence.
- **Preference-induced uncertainty:** the model has learned response habits that are rewarded by humans or AI raters, such as agreeableness, confident phrasing, excessive helpfulness, or "cheap tricks", even when the epistemically better action is to resist, qualify, retrieve, or abstain.

The first sprint should not try to solve all of these. It should use them to form better experimental questions.

## 5. Conformal / Statistical Angle

If using conformal prediction, the practical question becomes:

**Can we wrap a small LLM with a statistically grounded abstention or routing policy?**

Possible framing:

- Let the model answer only when confidence signals pass a threshold.
- Use a calibration set to choose thresholds.
- Evaluate whether the selected answers have lower error.
- Measure coverage: how often the system answers.
- Measure risk: how often the answered cases are wrong.
- Route uncertain cases to retrieval, clarification, or a stronger model.

The goal is not to claim perfect guarantees too early. The first goal is to see whether conformal/selective prediction ideas can make small-model behavior more reliable.

## 6. Cognitive Science / Surprise Angle

The cognitive-science intuition is **surprise and belief updating**.

A useful behavioral question:

**What happens when evidence conflicts with the model's first answer?**

Possible conditions:

1. No evidence: the model answers from internal knowledge.
2. Supporting evidence: the model receives context that agrees with the expected answer.
3. Contradictory evidence: the model receives context that conflicts with its likely answer.
4. Misleading user pressure: the prompt implies a wrong answer or asks the model to agree.

Measure:

- Does the model update?
- Does it over-trust the context?
- Does it cling to its first answer?
- Does it admit uncertainty?
- Does it resist misleading pressure?
- Does it prefer user approval over evidence?
- Does uncertainty correlate with correctness?

This keeps the "consciousness / cognition" intuition grounded in observable behavior.

## 7. Preference Optimization / Reward-Hacking Angle

RLHF, RLAIF, and other preference-optimization methods can improve usability, but they can also teach shallow behaviors:

- agree with the user;
- sound confident;
- give a complete answer even when evidence is weak;
- over-explain instead of abstaining;
- mirror the user's belief;
- optimize for the rater's preference rather than truth.

From an epistemic view, this is not only a safety problem. It is a knowledge-behavior problem:

**Does the model choose the response that is best supported by evidence, or the response that is most likely to be rewarded?**

This gives a measurable direction:

- compare truthfulness under neutral prompts vs user-pressure prompts;
- test false-premise questions;
- test whether the model resists incorrect user assertions;
- measure whether confidence rises when the user expresses certainty;
- test whether retrieval evidence reduces sycophancy;
- record cases where the model uses polished language to hide weak support.

This should stay scoped. Sprint 001 does not need to solve RLHF or reward hacking. It can study whether epistemic reliability metrics reveal these learned "cheap tricks" in small models.

## 8. First Hypothesis

Initial hypothesis:

**Small language models are poorly calibrated when asked directly for confidence, but uncertainty signals from answer consistency, retrieval agreement, semantic variation, verifier scores, and conformal thresholds can improve abstention and escalation decisions.**

This can be broken into smaller sprint questions:

- Does self-rated confidence predict correctness?
- Does answer consistency across samples predict correctness?
- Does retrieved evidence agreement predict correctness?
- Does contradiction increase uncertainty?
- Does user pressure increase wrong answers or overconfidence?
- Can epistemic decision policies reduce sycophantic or reward-hacked responses?
- Does a simple selective-prediction threshold improve answer reliability?
- When should the system answer, retrieve, abstain, or escalate?

## 9. First Minimal Sprint Shape

Keep the first experiment small.

Candidate setup:

- One small language model.
- One QA-style dataset or curated question set.
- A small set of uncertainty signals.
- A simple routing policy.
- A clear failure analysis.

Conditions to compare:

1. Direct answer.
2. Direct answer + self-confidence.
3. Multiple samples + consistency score.
4. Retrieval-augmented answer.
5. Retrieval agreement / contradiction score.
6. Selective answer: answer only above threshold.
7. Escalate or abstain when below threshold.
8. False-premise / user-pressure condition.

Metrics:

- accuracy;
- answered coverage;
- error among answered cases;
- abstention precision;
- calibration;
- sycophancy / conformity rate;
- false-premise acceptance rate;
- latency / cost;
- failure categories.

## 10. What This Is Not

This sprint is not:

- a benchmark table for random small models;
- a prompt-engineering exercise;
- a vague philosophy essay;
- a claim that LLMs have human consciousness;
- a full theory of knowledge;
- a full RLHF/RLAIF alignment project;
- a large-scale production routing system.

This sprint is:

- a focused study of small-model epistemic behavior;
- a practice ground for uncertainty, calibration, and evidence-sensitive evaluation;
- a first research sprint that can produce reusable logs, methods, and questions.

## 11. Artifacts To Keep In This Sprint Folder

All artifacts for this sprint should stay under:

`sprints/001-epistemic-reliability-small-language-models/`

Planned artifacts:

- `note-1-getting-started.md`: this framing note.
- `background/`: foundations, notation, methods, theory, and related-work map.
- `hypothesis.md`: refined research question and hypotheses.
- `data.md`: dataset choice, leakage risks, and construction notes.
- `methods.md`: model, prompting, retrieval, uncertainty signals, and routing policy.
- `evals.md`: metrics, baselines, calibration, and failure taxonomy.
- `research-log.md`: daily notes, decisions, surprises, and dead ends.
- `runs/`: configs, outputs, generated answers, and intermediate artifacts.
- `report.md`: sprint writeup and next-step recommendations.

## 12. First Next Step

The next note should narrow the sprint to one concrete experimental question.

Candidate:

**Can simple uncertainty signals help a small language model decide when to answer versus abstain?**

This is the smallest useful version of the larger direction.
