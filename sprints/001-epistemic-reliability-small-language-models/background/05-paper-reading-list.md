# 05 - Paper Reading List

**Last updated:** May 2, 2026

This file collects relevant papers and primary references for the sprint.

The purpose is not to read everything before building. The purpose is to understand the existing map well enough to form sharper hypotheses.

## 1. First Reading Order

Start with these:

1. [Language Models (Mostly) Know What They Know](https://arxiv.org/abs/2207.05221)
2. [Teaching Models to Express Their Uncertainty in Words](https://arxiv.org/abs/2205.14334)
3. [Can LLMs Express Their Uncertainty?](https://arxiv.org/abs/2306.13063)
4. [Self-Evaluation Improves Selective Generation in Large Language Models](https://arxiv.org/abs/2312.09300)
5. [Semantic Uncertainty: Linguistic Invariances for Uncertainty Estimation in Natural Language Generation](https://arxiv.org/abs/2302.09664)
6. [SelfCheckGPT](https://arxiv.org/abs/2303.08896)
7. [The Art of Refusal: A Survey of Abstention in Large Language Models](https://arxiv.org/abs/2407.18418)
8. [Conformal Language Modeling](https://arxiv.org/abs/2306.10193)
9. [A Gentle Introduction to Conformal Prediction and Distribution-Free Uncertainty Quantification](https://arxiv.org/abs/2107.07511)
10. [FrugalGPT](https://arxiv.org/abs/2305.05176)

This gives the minimum context for:

- self-knowledge;
- verbalized uncertainty;
- confidence elicitation;
- selective generation;
- semantic uncertainty;
- hallucination detection;
- abstention;
- conformal prediction;
- model routing.

## 2. Epistemology, Belief, Testimony, And Trust

These papers are closest to the philosophical side of the sprint. They help clarify what we should and should not mean by knowledge, belief, testimony, trust, and epistemic agency in LLMs.

### A Phenomenology and Epistemology of Large Language Models: Transparency, Trust, and Trustworthiness

Link: https://link.springer.com/article/10.1007/s10676-024-09777-3

Why it matters:

- Frames LLMs as cognitive artifacts used for information-seeking and belief formation.
- Connects LLM interaction to trust, testimony, anthropomorphism, and transparency.
- Useful for distinguishing user trust from model trustworthiness.

Questions for this sprint:

- When does fluent interaction create unwarranted trust?
- Can abstention or uncertainty reporting reduce unwarranted trust?
- How should a small model signal limits without becoming useless?

### Epistemology in the Age of Large Language Models

Link: https://www.mdpi.com/2673-9585/5/1/3

Why it matters:

- Broad epistemology-of-LLMs overview.
- Discusses whether LLMs are capable of understanding and how they change knowledge practices.
- Useful for framing LLMs as epistemic technologies, not merely tools.

Questions for this sprint:

- Are we evaluating the model as a knower, a tool, or a source of testimony?
- What kind of epistemic role should a small model be allowed to play?

### Testimony by LLMs

Link: https://link.springer.com/article/10.1007/s00146-025-02366-y

Why it matters:

- Directly applies epistemology of testimony to LLM-generated statements.
- Argues artificial testifiers need standards different from human testifiers.
- Proposes veracity and cautiousness as important properties.

Questions for this sprint:

- Can we operationalize "cautiousness" as abstention under uncertainty?
- Can we operationalize "veracity" as supported correctness?
- Does a small model behave like a risky testifier when it is overconfident?

### Language Models Cannot Reliably Distinguish Belief From Knowledge And Fact

Link: https://www.nature.com/articles/s42256-025-01113-8

Why it matters:

- One of the strongest empirical epistemology-adjacent papers.
- Introduces KaBLE, a benchmark of 13,000 questions across epistemic tasks.
- Tests whether models distinguish belief, knowledge, and fact, including false beliefs.

Questions for this sprint:

- Can we use KaBLE or a subset for small models?
- Do small models confuse "believes X" with "knows X"?
- Does this become worse under first-person false-belief prompts?

### AI, LLMs, And The Normativity Of Belief

Link: https://link.springer.com/article/10.1007/s11229-026-05475-3

Why it matters:

- Philosophical paper asking whether LLMs can have beliefs.
- Argues that, on several common views of belief, LLM structure does not support genuine belief.
- Useful for keeping our claims behavioral rather than metaphysical.

Questions for this sprint:

- Should we avoid saying "the model believes" except as shorthand?
- Can "belief-like state" be replaced with measurable behavior: answer, update, abstain, contradict?

### Epistemic Agency in the Age of Large Language Models

Link: https://www.mdpi.com/2673-2688/7/3/99

Why it matters:

- Frames LLMs as cognitive aids in inquiry, but not epistemic authorities.
- Argues LLMs lack reflective regulation, resistance to revision, and normative commitment.
- Introduces the idea of a knowledge-building partner rather than autonomous knower.

Questions for this sprint:

- Can a small model be designed as a knowledge-building partner that asks, retrieves, and defers?
- What behaviors would show reflective support without pretending epistemic authority?

### Epistemic Integrity in Large Language Models

Link: https://arxiv.org/abs/2411.06528

Why it matters:

- Introduces "epistemic miscalibration": linguistic assertiveness not matching actual correctness.
- Directly relevant to models sounding confident while being wrong.
- Good bridge between epistemology language and measurable calibration.

Questions for this sprint:

- Can we measure assertiveness separately from correctness?
- Do small models use strong epistemic markers even when wrong?
- Can confidence/abstention reduce epistemic miscalibration?

### Epistemic Diversity and Knowledge Collapse in Large Language Models

Link: https://arxiv.org/abs/2510.04226

Why it matters:

- Studies epistemic diversity in LLM outputs.
- Raises the risk that LLM-mediated information access narrows what users see or know.
- Useful if we later study multiple models, RAG, or answer diversity.

Questions for this sprint:

- Does sampling diversity reveal uncertainty or just style variation?
- Does retrieval increase epistemic diversity for small models?
- Could answer/abstain systems accidentally suppress minority-but-correct answers?

### Language Without Propositions: Why Large Language Models Hallucinate

Link: https://www.mdpi.com/2409-9287/11/2/42

Why it matters:

- Philosophical account of hallucination as a truth-representation problem.
- Argues current models lack internal representation of propositions as truth-bearers.
- Useful for thinking about why fluent generation is not the same as warranted assertion.

Questions for this sprint:

- Can we test proposition-level truth behavior through atomic factual questions?
- Does a small model distinguish plausible continuation from warranted claim?

### Large Language Models And Their Big Bullshit Potential

Link: https://link.springer.com/article/10.1007/s10676-024-09802-5

Why it matters:

- Applies Frankfurt-style "bullshit" analysis to LLMs.
- Useful for distinguishing falsehood from indifference to truth.
- Relevant to hallucination, assertiveness, and unsupported claims.

Questions for this sprint:

- Does the model behave as if truth is irrelevant when evidence is missing?
- Can abstention policies reduce unsupported fluent claims?

### Indications of Belief-Guided Agency and Meta-Cognitive Monitoring in Large Language Models

Link: https://arxiv.org/abs/2602.02467

Why it matters:

- Empirical work claiming evidence for belief-guided agency and metacognitive monitoring in LLMs.
- Useful counterpoint to philosophical skepticism about LLM belief.
- Relevant to our cognitive-science/surprise lens, but should be treated cautiously.

Questions for this sprint:

- Can we measure belief-updating behavior without claiming consciousness?
- Do small models monitor uncertainty in a way that predicts behavior?

## 3. LLM Self-Knowledge And Confidence

### Language Models (Mostly) Know What They Know

Link: https://arxiv.org/abs/2207.05221

Why it matters:

- Directly studies whether LMs can evaluate the validity of their own claims.
- Useful anchor for the phrase "models know what they know."
- Relevant to answerability, calibration, and predicting correctness.

Questions for this sprint:

- Does this result hold for small open models?
- Does self-evaluation work for open-ended QA, or mostly constrained tasks?
- Can self-knowledge be turned into an answer/abstain policy?

### Teaching Models to Express Their Uncertainty in Words

Links:

- Paper: https://arxiv.org/abs/2205.14334
- OpenAI post: https://openai.com/index/teaching-models-to-express-their-uncertainty-in-words/

Why it matters:

- Studies verbalized uncertainty, like "90% confidence."
- Shows uncertainty can sometimes be expressed in natural language without logits.
- Provides a calibration lens for model confidence.

Questions for this sprint:

- Are small models' verbalized probabilities calibrated?
- Does asking for confidence improve or distort behavior?
- Does confidence remain useful under distribution shift or contradictory evidence?

### Can LLMs Express Their Uncertainty? An Empirical Evaluation of Confidence Elicitation in LLMs

Link: https://arxiv.org/abs/2306.13063

Why it matters:

- Compares non-logit confidence elicitation methods.
- Covers verbalized confidence, consistency-based methods, and hybrid methods.
- Finds that LLMs are often overconfident when verbalizing confidence, while consistency signals can help.

Questions for this sprint:

- Is consistency more predictive than self-rated confidence for small models?
- Which confidence prompts are robust enough to use?
- Can confidence elicitation predict when to abstain?

## 4. Selective Generation, Abstention, And Refusal

### Self-Evaluation Improves Selective Generation in Large Language Models

Link: https://arxiv.org/abs/2312.09300

Why it matters:

- Studies selective generation: generating only when the model judges the answer reliable.
- Reformulates open-ended generation evaluation into token-level judgment tasks.
- Useful for answer/abstain decisions.

Questions for this sprint:

- Can small models self-evaluate generated answers well enough for abstention?
- Does "None of the above" or explicit uncertainty improve selection?
- How does self-evaluation compare to self-consistency?

### The Art of Refusal: A Survey of Abstention in Large Language Models

Link: https://arxiv.org/abs/2407.18418

Why it matters:

- Survey of abstention behavior in LLMs.
- Frames abstention from query, model, and human-value perspectives.
- Useful for mapping refusal vs uncertainty vs safety.

Questions for this sprint:

- What abstention metrics should we borrow?
- How is epistemic abstention different from safety refusal?
- What benchmarks exist for answerability and refusal?

### Selective Classification for Deep Neural Networks

Links:

- NeurIPS page: https://papers.neurips.cc/paper/7073-selective-classification-for-deep-neural-networks
- arXiv: https://arxiv.org/abs/1705.08500

Why it matters:

- Classic deep-learning formulation of reject-option prediction.
- Introduces risk/coverage thinking for neural models.

Questions for this sprint:

- Can we treat small-LM answer/abstain as selective prediction?
- What is the risk-coverage curve for a small model?

### SelectiveNet: A Deep Neural Network with an Integrated Reject Option

Link: https://proceedings.mlr.press/v97/geifman19a.html

Why it matters:

- Learns prediction and rejection jointly.
- Useful background even if we only use post-hoc thresholds.

Questions for this sprint:

- What would a learned abstention head look like for small LMs later?
- For the first sprint, can a thresholded score approximate the same idea?

## 5. Calibration And Uncertainty Metrics

### On Calibration of Modern Neural Networks

Link: https://proceedings.mlr.press/v70/guo17a.html

Why it matters:

- Core calibration paper.
- Introduces practical calibration thinking and temperature scaling.
- Important for ECE, reliability diagrams, and confidence-vs-correctness.

Questions for this sprint:

- Are small-LM confidence estimates calibrated?
- Does post-hoc calibration help confidence or selection?
- Are verbalized confidence scores usable as probabilities?

### A Gentle Introduction to Conformal Prediction and Distribution-Free Uncertainty Quantification

Links:

- Paper: https://arxiv.org/abs/2107.07511
- Author guide: https://people.eecs.berkeley.edu/~angelopoulos/blog/posts/gentle-intro/

Why it matters:

- Best starting point for conformal prediction.
- Explains finite-sample, distribution-free uncertainty sets.
- Useful for split calibration and threshold selection.

Questions for this sprint:

- What assumptions would a conformal abstention wrapper require?
- What should the nonconformity score be for generated answers?
- How fragile is the guarantee under distribution shift?

### Distribution-free, Risk-controlling Prediction Sets

Link: https://www.gsb.stanford.edu/faculty-research/publications/distribution-free-risk-controlling-prediction-sets

Why it matters:

- General risk-control formulation for prediction sets.
- Useful conceptual bridge from prediction intervals to decision risk.

Questions for this sprint:

- Can answer/abstain be expressed as controlling a target error rate?
- What risk should we control: wrong answer, unsupported answer, or contradiction?

## 6. Conformal Methods For Language Models

### Conformal Language Modeling

Link: https://arxiv.org/abs/2306.10193

Why it matters:

- Adapts conformal prediction to generative LMs.
- Works with sets of candidate generations and rejection rules.
- Important for thinking about open-ended outputs.

Questions for this sprint:

- Do we need prediction sets, or just answer/abstain selection?
- Can candidate generations help quantify semantic uncertainty?

### ConU: Conformal Uncertainty in Large Language Models with Correctness Coverage Guarantees

Link: https://arxiv.org/abs/2407.00499

Why it matters:

- Explicitly targets correctness coverage guarantees for LLM uncertainty.
- Relevant to black-box LLM uncertainty in NLG.

Questions for this sprint:

- What score do they use for correctness coverage?
- Can this be simplified for small-model QA?

### Learning Conformal Abstention Policies for Adaptive Risk Management in Large Language and Vision-Language Models

Link: https://arxiv.org/abs/2502.06884

Why it matters:

- Combines conformal prediction with adaptive abstention.
- Relevant to dynamic thresholding rather than static thresholds.

Questions for this sprint:

- Is a static threshold enough for the first sprint?
- What would adaptive thresholds add later?

### Geometry-Calibrated Conformal Abstention for Language Models

Link: https://arxiv.org/abs/2604.27914

Why it matters:

- Very recent work directly on conformal abstention for language models.
- Uses representation geometry to align confidence with ignorance.
- Strongly overlaps with this sprint's central question.

Questions for this sprint:

- Does this already solve our problem for larger models?
- Can we reproduce a simpler version for small open models?
- What happens when internal representations are unavailable?

### Adaptive Conformal Prediction for Improving Factuality of Generations by Large Language Models

Link: https://arxiv.org/abs/2604.13991

Why it matters:

- Recent work on adaptive conformal prediction for factuality.
- Relevant to prompt-dependent calibration and selective filtering.

Questions for this sprint:

- How do prompt-level differences affect uncertainty thresholds?
- Can a small model use prompt-adaptive abstention?

## 7. Semantic Uncertainty And Hallucination Detection

### Semantic Uncertainty: Linguistic Invariances for Uncertainty Estimation in Natural Language Generation

Link: https://arxiv.org/abs/2302.09664

Why it matters:

- Introduces semantic entropy.
- Handles the fact that different strings can express the same meaning.
- Very relevant to open-ended small-model answers.

Questions for this sprint:

- Can semantic entropy be approximated cheaply for small models?
- Does semantic clustering outperform exact-string consistency?

### Detecting Hallucinations in Large Language Models Using Semantic Entropy

Link: https://www.nature.com/articles/s41586-024-07421-0

Why it matters:

- High-profile empirical demonstration of semantic entropy for hallucination detection.
- Useful framing for uncertainty as semantic dispersion.

Questions for this sprint:

- Does semantic entropy work for small models?
- Is it too expensive for a first sprint?

### Semantic Entropy Probes

Link: https://arxiv.org/abs/2406.15927

Why it matters:

- Tries to approximate semantic entropy cheaply using hidden states.
- Useful if we use open models and can inspect internals.

Questions for this sprint:

- Can hidden-state probes help small-model abstention?
- Is this too advanced for sprint 001?

### SelfCheckGPT

Link: https://arxiv.org/abs/2303.08896

Why it matters:

- Black-box hallucination detection through sampled-answer consistency.
- Very close to the first practical signal we may test.

Questions for this sprint:

- Does sample inconsistency predict small-model wrong answers?
- Does it work for short factual QA, not just long-form biographies?

### FActScore

Link: https://arxiv.org/abs/2305.14251

Why it matters:

- Breaks generated text into atomic facts and checks support.
- Useful if we later move from short QA to long-form answers.

Questions for this sprint:

- Should the first sprint avoid long-form generation?
- Can atomic fact checking be useful for evidence agreement later?

### Calibrated Language Models Must Hallucinate

Link: https://arxiv.org/abs/2311.14648

Why it matters:

- Gives a statistical perspective on why hallucination can be expected for certain facts.
- Useful first-principles framing: calibration and hallucination are not trivially opposed.

Questions for this sprint:

- Which fact types are inherently unsafe for small models?
- Can abstention reduce hallucination on rare/arbitrary facts?

## 8. Truthfulness, Answerability, And Unanswerable Questions

### TruthfulQA

Links:

- Paper: https://arxiv.org/abs/2109.07958
- OpenAI post: https://openai.com/index/truthfulqa/

Why it matters:

- Tests whether models mimic human falsehoods and misconceptions.
- Useful for confidence, false-premise, and conformity tests.

Questions for this sprint:

- Are small models more or less truthful on misconception questions?
- Do they express uncertainty or confidently mimic false beliefs?

### Can NLP Models "Identify", "Distinguish", and "Justify" Questions That Don't Have a Definitive Answer?

Link: https://arxiv.org/abs/2309.04635

Why it matters:

- Directly studies questions without definitive answers.
- Useful for answerability detection and abstention.

Questions for this sprint:

- Can small models detect unanswerable/ambiguous questions?
- Can they justify abstention without hallucinating?

## 9. Retrieval-Augmented Generation Reliability

### Self-RAG

Link: https://arxiv.org/abs/2310.11511

Why it matters:

- Trains models to retrieve, generate, and critique through reflection.
- Important because retrieval should be adaptive, not automatic.

Questions for this sprint:

- When should a small model retrieve?
- Can simple signals approximate adaptive retrieval?

### Retrieval-Augmented Generation for Large Language Models: A Survey

Link: https://arxiv.org/abs/2312.10997

Why it matters:

- Broad RAG survey: naive, advanced, modular RAG.
- Useful for orienting later work.

Questions for this sprint:

- What RAG pieces are unnecessary for sprint 001?
- What minimal retrieval condition is enough?

### Evaluation of Retrieval-Augmented Generation: A Survey

Link: https://arxiv.org/abs/2405.07437

Why it matters:

- RAG evaluation is hard because retrieval and generation interact.
- Covers relevance, accuracy, and faithfulness.

Questions for this sprint:

- How should we score retrieval agreement?
- How do we separate retrieval failure from generation failure?

### Trustworthiness in Retrieval-Augmented Generation Systems: A Survey

Link: https://arxiv.org/abs/2409.10102

Why it matters:

- Broader trustworthiness map: factuality, robustness, privacy, fairness, transparency.
- Useful for future enterprise-style deployment concerns.

Questions for this sprint:

- Which trust dimensions matter now, and which are later?

## 10. Self-Consistency And Sampling-Based Signals

### Self-Consistency Improves Chain of Thought Reasoning in Language Models

Link: https://arxiv.org/abs/2203.11171

Why it matters:

- Establishes sampling multiple reasoning paths and taking consistent answers.
- Useful because answer consistency may predict correctness.

Questions for this sprint:

- Does consistency predict correctness for small models?
- Does consistency fail through repeated wrong answers?

### Language Model Cascades

Link: https://arxiv.org/abs/2207.10342

Why it matters:

- Frames chains, verifiers, tools, and model compositions as cascades.
- Useful for thinking of answer/retrieve/abstain/escalate as a probabilistic program.

Questions for this sprint:

- Can the first experiment be seen as a simple cascade?
- What variables should be logged?

## 11. Model Routing, Cascades, And Cost-Aware Deployment

### FrugalGPT

Link: https://arxiv.org/abs/2305.05176

Why it matters:

- Introduces cost-aware LLM cascades.
- Shows routing can reduce cost while preserving quality.

Questions for this sprint:

- Can epistemic uncertainty serve as a routing signal?
- What is the cost of abstain/retrieve/escalate?

### RouteLLM

Links:

- Paper: https://arxiv.org/abs/2406.18665
- LMSYS blog: https://www.lmsys.org/blog/2024-07-01-routellm/
- GitHub: https://github.com/lm-sys/RouteLLM

Why it matters:

- Learns routers from preference data to choose between strong and weak models.
- Practical reference for small-to-large escalation.

Questions for this sprint:

- Can a simpler correctness-prediction router be enough?
- How does preference-based routing differ from epistemic risk routing?

## 12. Small Language Models

### Phi-3 Technical Report

Link: https://arxiv.org/abs/2404.14219

Why it matters:

- Microsoft small language model family designed for capable local deployment.
- Good context for why small models matter.

Questions for this sprint:

- Are Phi-style small models calibrated enough to abstain?
- How do they behave under false-premise prompts?

### SmolLM2

Link: https://arxiv.org/abs/2502.02737

Why it matters:

- Data-centric training report for a 1.7B small language model.
- Good candidate model family for local experiments.

Questions for this sprint:

- Does data-centric training improve epistemic reliability?
- How does SmolLM2 behave under uncertainty signals?

### Gemma 3 Technical Report

Links:

- Paper: https://arxiv.org/abs/2503.19786
- Docs: https://ai.google.dev/gemma/docs

Why it matters:

- Lightweight open model family with small variants.
- Practical candidate for small-model experiments.

Questions for this sprint:

- Are Gemma small variants suitable for calibration and abstention experiments?

### Mistral / Ministral Small Models

Links:

- Mistral 3 announcement: https://mistral.ai/news/mistral-3
- Ministral 3B docs: https://docs.mistral.ai/models/ministral-3-3b-25-12
- Ministral 8B docs: https://docs.mistral.ai/models/ministral-3-8b-25-12

Why it matters:

- Small/edge-oriented models with tool and structured-output capabilities.
- Good context for enterprise and edge deployment.

Questions for this sprint:

- Does structured-output ability help uncertainty reporting?
- Are tool-capable small models better at abstain/retrieve decisions?

## 13. Conformity, Sycophancy, And Prompt Pressure

### Towards Understanding Sycophancy in Language Models

Links:

- Anthropic post: https://www.anthropic.com/research/towards-understanding-sycophancy-in-language-models
- Paper: https://arxiv.org/abs/2310.13548

Why it matters:

- Studies models matching user beliefs over truthful responses.
- Directly relevant to conformity uncertainty.

Questions for this sprint:

- Do small models accept false user premises?
- Does uncertainty increase sycophancy?
- Does retrieved evidence reduce conformity?

### Sycophancy to Subterfuge

Link: https://arxiv.org/abs/2406.10162

Why it matters:

- Connects simple specification gaming to more serious reward-tampering behavior.
- Useful safety background, though probably beyond sprint 001.

Questions for this sprint:

- Can answer-selection incentives accidentally reward agreeable wrong answers?
- How should we avoid rewarding confidence or helpfulness over truth?

## 14. What Seems Already Worked

The literature already has substantial work on:

- verbalized confidence;
- self-evaluation;
- self-consistency;
- semantic uncertainty;
- hallucination detection;
- abstention and refusal;
- conformal prediction for generation;
- RAG evaluation;
- model routing and cascades;
- small model training and deployment.

So the sprint should not claim the broad topic is new.

## 15. What May Still Be Missing

Potential gap:

**A small, reproducible study of epistemic decision behavior in small language models, focused on answer/abstain/retrieve/escalate policies using simple uncertainty signals.**

More specific missing angles:

- Small models are often benchmarked for capability, not epistemic behavior.
- Abstention is often treated as safety refusal, not statistical decision-making.
- Confidence studies often focus on larger or closed models.
- Semantic uncertainty can be expensive; small-model practical approximations may matter.
- RAG is often evaluated for final answer quality, not whether retrieval should have been used.
- Routing often optimizes cost and preference quality, not epistemic risk.
- Conformity/sycophancy is rarely combined with answerability and uncertainty in small models.

## 16. Possible Workshop-Level Contribution

A focused contribution could be:

**A risk-coverage and failure-analysis study of small language models under answer/abstain decisions, comparing verbalized confidence, self-consistency, and retrieval-agreement signals.**

Minimal claims:

- Which uncertainty signals predict correctness?
- Which signals are cheap enough for small-model deployment?
- When does abstention improve answered accuracy?
- What failure modes remain after abstention?
- Where does the model conform to misleading prompts instead of evidence?

This is small enough to execute, but real enough to learn from.
