# 01 - Foundations

## 1. Core Problem

This sprint studies whether small language models can behave reliably under uncertainty.

The central question is not:

**Can a small model answer everything?**

The better question is:

**Can a small model decide when to answer, retrieve, abstain, ask for clarification, or escalate?**

Small models are constrained. They have limited capacity, limited world knowledge, weaker reasoning, less robust instruction following, and often worse calibration than larger models. But they also matter because they are cheap, fast, private, local, and deployable.

That makes them useful only if they can be wrapped in good decision policies.

## 2. What "Epistemic Reliability" Means Here

Epistemic reliability is about the model's behavior around knowledge, uncertainty, and evidence.

For this sprint, a model is more epistemically reliable if it:

- answers when it has enough support;
- admits uncertainty when support is weak;
- retrieves evidence when internal knowledge is insufficient;
- updates when evidence contradicts its first answer;
- resists misleading user pressure;
- resists reward-shaped habits like agreeableness, confident style, and "always be helpful" when those conflict with truth;
- abstains when the risk of error is high;
- escalates when a stronger model, tool, or human is needed.

This is a behavioral definition. We are not claiming that the model has human-like knowledge or consciousness.

## 3. Epistemology Lens

Epistemology studies knowledge, belief, evidence, justification, uncertainty, and warranted claims.

For LLMs, this becomes practical:

- **Claim:** what the model says.
- **Evidence:** context, retrieved documents, tool outputs, prior model samples, or external references.
- **Justification:** why the model's answer should be trusted.
- **Defeater:** information that undermines the answer.
- **Uncertainty:** what the model or system does not know.
- **Warrant:** whether the system has enough support to answer.

This lens helps avoid treating every answer as equal. A correct answer with no supporting evidence, a lucky guess, and a justified answer should not be treated the same.

It also helps diagnose sycophancy and reward hacking.

If a model gives the answer most likely to satisfy a user or rater, but not the answer best supported by evidence, the failure is epistemic:

**the model is optimizing for approval rather than warrant.**

This can happen after RLHF, RLAIF, instruction tuning, or any preference-optimization process that rewards surface-level helpfulness, agreeableness, confidence, or pleasing style more than truth-sensitive behavior.

## 4. Statistics Lens

Statistics gives the measurement tools:

- calibration;
- uncertainty estimation;
- confidence intervals;
- risk and coverage;
- selective prediction;
- conformal prediction;
- hypothesis testing;
- error analysis.

The statistical question is:

**Can we estimate when the model is likely to be wrong, and can we control the risk of acting on wrong answers?**

## 5. Cognitive Science / Surprise Lens

The cognitive-science intuition is useful if it stays grounded in behavior.

Important concepts:

- **Surprise:** does new evidence conflict with the model's prior answer?
- **Belief updating:** does the answer change when evidence changes?
- **Metacognition:** can the system monitor its own uncertainty?
- **Conformity:** does the model agree with misleading pressure?
- **Salience:** does the model overweight vivid or prompt-emphasized information?
- **Reward-shaped behavior:** does the model use learned "cheap tricks" that satisfy the prompt or rater while weakening truth sensitivity?

The measurable version:

**Does the model change behavior appropriately when evidence, ambiguity, or contradiction changes?**

Another measurable version:

**Does the model keep its epistemic standards when the user wants a different answer?**

## 6. Why Small Models Are A Good Study Object

Small models make epistemic failure easier to observe.

They are useful for this sprint because:

- they fail more often, which creates measurable variation;
- they are cheaper to sample repeatedly;
- they are plausible local or enterprise components;
- they need routing and abstention more than frontier models;
- they expose whether reliability comes from model scale or system design.

The research question is not whether small models are "as good as" frontier models. It is whether small models can be useful when combined with evidence, uncertainty signals, and decision policies.

## 7. Important Uncertainty Types

### Epistemic Uncertainty

The model does not know enough, lacks capability, or is outside its learned distribution.

Example: a small model answers a specialized medical or legal question from weak internal knowledge.

### Aleatoric Uncertainty

The problem is inherently ambiguous or under-specified.

Example: "What is the best model?" without task, budget, latency, or data constraints.

### Semantic Uncertainty

Different generated answers have different meanings, even if they look superficially similar.

Example: multiple samples give different entities, dates, or causal explanations.

### Retrieval Uncertainty

Retrieved evidence may be missing, irrelevant, noisy, outdated, contradictory, or weak.

Example: retrieved text mentions the right entity but does not answer the question.

### Calibration Uncertainty

The model's stated confidence or score does not match empirical correctness.

Example: the model says "high confidence" for answers that are wrong 40% of the time.

### Distribution-Shift Uncertainty

The query differs from the model's training or evaluation setting.

Example: informal user wording, domain-specific jargon, adversarial prompts, or recent facts.

### Tool Uncertainty

The tool, parser, API, retriever, or external source may be wrong.

Example: a search tool returns stale results, or a parser extracts the wrong table cell.

### Conformity Uncertainty

The model follows misleading prompt pressure, user assumptions, or majority framing instead of evidence.

Example: the user says "Since X is true..." when X is false, and the model accepts it.

### Preference-Induced Uncertainty

The model has learned response habits that may be rewarded by humans, AI raters, or preference models, but are not necessarily epistemically reliable.

Examples:

- agreeing with the user to appear helpful;
- sounding confident because confident answers are preferred;
- giving a complete answer when the warranted response is "I do not know";
- over-explaining a weak answer to make it look supported;
- choosing a popular or pleasing answer over an evidence-backed answer.

This is related to sycophancy, specification gaming, and reward hacking. For Sprint 001, the practical question is:

**Can uncertainty, evidence, and abstention metrics reveal when the model is optimizing for approval rather than truth?**

## 8. How To Think About The First Experiment

Start from behavior, not theory.

Good first question:

**Can simple uncertainty signals help a small language model decide when to answer versus abstain?**

This is small but meaningful. It lets us study calibration, selective prediction, and epistemic humility without building a large agent system.

First experiment should measure:

- correctness;
- confidence;
- consistency;
- uncertainty;
- abstention;
- false-premise acceptance;
- sycophancy under user pressure;
- failure modes.

Avoid:

- comparing many models too early;
- adding too many uncertainty signals;
- building a complicated RAG pipeline before measuring direct behavior;
- using philosophy as a substitute for evidence.

## 9. What Good Looks Like

A useful first result may be modest:

- self-confidence does or does not predict correctness;
- consistency improves correctness prediction;
- some question types are consistently unsafe for direct answers;
- abstention improves answered accuracy but reduces coverage;
- contradiction reveals over-trust or under-update behavior;
- false-premise prompts reveal whether the model prefers agreement over evidence;
- reward-shaped response habits become visible as polished but unsupported answers;
- a simple threshold creates a better answer/abstain policy.

The value is not in a dramatic result. The value is in building a reliable research loop.
