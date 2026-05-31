# Heterogeneous Feature Transport: Large Teacher to Small Student

This note focuses on the harder version of knowledge transfer:

**A large teacher model was trained differently, has different layers, different hidden dimensions, maybe a different tokenizer or architecture, and we want a smaller student model to absorb selected knowledge rather than merely imitate final answers.**

This is not ordinary distillation. It is closer to representation transport.

## Core Statistical Framing

Let:

- `T` be the large teacher model;
- `S` be the small student model;
- `z_T^l(x)` be the teacher activation at layer `l` for input `x`;
- `z_S^m(x)` be the student activation at layer `m`;
- `y_T(x)` be the teacher output distribution or generated answer;
- `u_T(x)` be teacher uncertainty, disagreement, or reliability for that example.

The problem is:

**Find a bridge that transfers useful structure from `z_T^l(x)` or `y_T(x)` into `z_S^m(x)` even when the two spaces are not directly comparable.**

The bridge can be a learned projection, a relational loss, an optimal-transport coupling, a concept probe, or a small adapter. The statistical question is what structure should be preserved and how much uncertainty attaches to it.

## Statistician's Problem Statement

The clean statistical version:

**Given a high-capacity teacher model trained under one data-generating process and a capacity-limited student model trained or deployed under another, estimate a reliable transferable latent signal from the teacher's outputs and internal representations, align that signal to the student's representation space, and train the student so target-domain risk decreases without inheriting teacher bias, miscalibration, or tail failures.**

In statistical language, the teacher is not an oracle. The teacher is a noisy measurement instrument for an unobserved target:

```text
latent target knowledge:      K(x)
teacher measurement:          M_T(x) = K(x) + bias_T(x) + noise_T(x)
student learned statistic:    M_S(x; theta)
```

The research problem is to estimate:

- **the estimand:** what `K(x)` is supposed to mean;
- **the measurement model:** how teacher outputs/features approximate `K(x)`;
- **the alignment map:** how teacher representation structure maps into student representation structure;
- **the uncertainty:** when teacher signals should be trusted, downweighted, or rejected;
- **the target risk:** whether the student improves on the deployment distribution, not just on teacher imitation.

A compact formal objective:

```text
min_theta E_{x ~ P_target} [ L(student(x; theta), K(x)) ]

using observed transfer data:
D = {x_i, y_T(x_i), z_T(x_i), u_T(x_i), z_S(x_i), optional gold labels}
```

Because `K(x)` is partly unobserved, statistics helps by treating teacher knowledge as a proxy variable with measurement error, bias, variance, and distribution shift.

## Right Vocabulary

Useful phrases for this research direction:

- **Heterogeneous representation distillation:** the ML-standard phrase when teacher and student architectures/features do not match.
- **Statistical feature transport:** a sharper phrase for your angle: estimate and transport selected teacher features under uncertainty.
- **Uncertainty-aware representation transfer:** emphasizes calibration, reliability, and selective copying.
- **Teacher-student learning with measurement error:** the statistician's framing.
- **Representation alignment under distribution shift:** useful when teacher/student data or deployment distributions differ.
- **Optimal-transport distillation:** method-specific wording when OT/Gromov-Wasserstein is central.

Best working title:

**Statistical Feature Transport for Heterogeneous LLM Distillation**

Best one-line problem statement:

**How can we estimate, align, and compress reliable latent knowledge from a large heterogeneous teacher model into a smaller student model while controlling uncertainty, bias transfer, and target-domain risk?**

## LLM-Specific Evidence That Statistics Matters

There is not yet one settled field called "statistical foundations of LLM knowledge transfer." The live literature is split across several names:

- LLM knowledge distillation;
- generalized distillation;
- uncertainty propagation in distillation;
- Bayesian knowledge distillation;
- generalization vs fidelity in LM distillation;
- geometry-aware representation alignment;
- cross-tokenizer or optimal-transport distillation.

The important point: several papers now explicitly argue that LLM transfer cannot be understood as simple imitation. It needs statistical reasoning about probability estimates, uncertainty, teacher reliability, distribution mismatch, representation geometry, and fidelity-generalization tradeoffs.

### 1. Statistical Distillation: General Foundation

["A Statistical Perspective on Distillation"](https://proceedings.mlr.press/v139/menon21a.html) is not LLM-specific, but it is the cleanest foundation. It argues that a teacher helps by estimating class probabilities and reducing variance in the student objective. It also explains why a more accurate teacher may not be a better teacher: what matters is the quality of the teacher's probability estimates.

LLM implication:

**For LLM distillation, teacher quality should not mean only benchmark accuracy. It should include calibration, uncertainty, and distributional fit to the target task.**

### 2. LLM Distillation Needs Distributional Objectives

["MiniLLM: On-Policy Distillation of Large Language Models"](https://arxiv.org/abs/2306.08543) is one of the clearest LLM-specific examples. It argues that standard forward KL is poorly matched to generative language-model distillation because it can make the student overestimate low-probability regions of the teacher distribution. The paper uses reverse KL and on-policy optimization, reporting better calibration and lower exposure bias.

LLM implication:

**The divergence matters. Different statistical losses transfer different parts of the teacher distribution.**

### 3. LLM KD Has a Generalization vs Fidelity Problem

["On the Generalization vs Fidelity Paradox in Knowledge Distillation"](https://arxiv.org/abs/2505.15442) gives direct LLM evidence. It presents a large-scale empirical and statistical analysis of KD for language models from 0.5B to 7B parameters across reasoning and instruction-following tasks. Its key finding is that KD can improve student accuracy while failing to preserve the teacher's structured reasoning process.

LLM implication:

**A student can generalize better behaviorally while being less faithful to the teacher's reasoning. Statistics is needed to distinguish accuracy gain, teacher imitation, and reasoning fidelity.**

### 4. Uncertainty Propagation Is a Transfer Problem

["How Is Uncertainty Propagated in Knowledge Distillation?"](https://arxiv.org/abs/2601.18909) explicitly studies uncertainty propagation across linear models, neural networks, and LLMs. It frames distillation as an uncertainty transformation: teacher outputs, student training, and student inference are all stochastic. It proposes multi-sample teacher averaging and inverse-variance weighting.

LLM implication:

**The student should not collapse teacher uncertainty into a single confident answer. Transfer should preserve useful uncertainty where the teacher is unstable or underspecified.**

### 5. Bayesian KD for Multiple LLM Teachers

["Multi-Teacher Knowledge Distillation via Teacher-Informed Mixture Priors"](https://arxiv.org/abs/2605.27967) is a very recent Bayesian framing. It argues that the statistical mechanisms of KD remain unclear and that uncertainty evaluation is often overlooked, especially when multiple teachers have different expertise. It proposes teacher-informed priors inside a Bayesian KD framework.

LLM implication:

**When using several teachers, the problem becomes source reliability and mixture modeling: which teacher should influence the student, where, and with how much uncertainty?**

### 6. Few-Shot LLM Distillation Has Statistical and Geometric Guarantees

["Few-Shot Knowledge Distillation of LLMs With Counterfactual Explanations"](https://openreview.net/pdf?id=2SLScUhZhu) studies task-aware LLM distillation when data is scarce. It argues that counterfactual examples near the teacher's decision boundary are more informative. The paper gives statistical and geometric motivation, including parameter-estimation and decision-boundary arguments.

LLM implication:

**Not all transfer examples are equally informative. A statistician should choose examples that identify the teacher's decision boundary efficiently.**

### 7. Representation Geometry Is a Distillation Object

["Knowledge Distillation through Geometry-Aware Representational Alignment"](https://arxiv.org/abs/2509.25253) studies feature geometry as a distillation signal across language-model families. It argues that naive projection losses or CKA-style objectives can miss feature structure, then motivates Procrustes distance and Gram-matrix geometry.

LLM implication:

**For heterogeneous teacher-student transfer, the object to transfer may be representation geometry rather than raw hidden states or final answers.**

### 8. Cross-Tokenizer Transfer Needs Statistical Alignment

["Universal Cross-Tokenizer Distillation via Approximate Likelihood Matching"](https://arxiv.org/abs/2503.20083) and ["Multi-Level Optimal Transport for Universal Cross-Tokenizer Knowledge Distillation"](https://ojs.aaai.org/index.php/AAAI/article/view/34543) both target LLM transfer when teacher and student tokenizers differ. This is directly relevant to heterogeneous transfer because token-level correspondence is broken.

LLM implication:

**When teacher and student token spaces do not match, knowledge transfer becomes an alignment problem between distributions, not a direct copying problem.**

### Bottom Line

For LLMs, the strongest evidence-backed framing is:

**Statistical foundations of LLM knowledge transfer ask when teacher signals are reliable, what distributional or representational object should be transferred, how uncertainty propagates through distillation, and whether the student improves target-domain risk without losing fidelity, calibration, or tail behavior.**

## What Can Be Transferred

Do not try to transfer "the teacher" as a whole. Choose the estimand.

Targetable objects:

- **Output behavior:** logits, answer distributions, ranked completions, refusals.
- **Intermediate features:** hidden states, attention maps, MLP activations, embedding vectors.
- **Representation geometry:** pairwise distances, angles, neighborhoods, clusters, manifolds.
- **Concept directions:** truthfulness, uncertainty, refusal, answerability, toxicity, domain expertise.
- **Reasoning process:** rationales, intermediate steps, verifier traces, critique signals.
- **Preference structure:** pairwise rankings, reward-model scores, quality comparisons.
- **Uncertainty structure:** where the teacher is confident, unstable, ambiguous, or wrong.

The main mistake is assuming that layer `l` in the teacher should equal layer `m` in the student. Usually the safer target is not raw activation equality, but a lower-dimensional, geometry-preserving, or concept-preserving constraint.

## Statistical Concepts That Help

### 1. Estimands and Sufficient Statistics

Ask: what is the minimal teacher signal the student needs?

For example:

- if the goal is factual QA, the estimand may be calibrated answerability;
- if the goal is safety, the estimand may be a refusal-policy boundary;
- if the goal is math transfer, the estimand may be step-validity or final-answer distribution;
- if the goal is domain expertise, the estimand may be a domain concept basis rather than raw logits.

This is a sufficient-statistic mindset: compress the teacher into the smallest signal that preserves the target behavior.

### 2. Representation Similarity: CKA, CCA, Procrustes

Before training, measure which teacher and student layers are even comparable.

Useful tools:

- **CCA / SVCCA:** find correlated subspaces between teacher and student activations.
- **CKA:** compare representation similarity through kernel or Gram-matrix structure.
- **Procrustes alignment:** find the best orthogonal map from one representation cloud to another.

These tools answer:

- Which teacher layer resembles which student layer?
- Does the student already contain a weaker version of the teacher feature?
- Is the target feature linearly transferable or geometry-only?
- Should we align activations directly or only align relations?

Useful anchor: ["Similarity of Neural Network Representations Revisited"](https://arxiv.org/abs/1905.00414) introduces CKA as a robust way to compare neural representations across layers and models. ["Knowledge distillation through geometry-aware representational alignment"](https://arxiv.org/abs/2509.25253) argues that feature geometry can be used directly as a distillation signal.

### 3. Projection-Based Hint Transfer

This is the simplest bridge.

Train small maps `P_S` and `P_T` so that student and teacher features live in a shared space:

```text
loss_feature = || P_S z_S^m(x) - P_T z_T^l(x) ||^2
```

This is useful when:

- teacher and student see the same inputs;
- there is a plausible layer match;
- dimensions differ but sample correspondence exists;
- the teacher's raw features are not too structurally different.

FitNets introduced this "hint" idea by using teacher intermediate representations to guide a thinner student: ["FitNets: Hints for Thin Deep Nets"](https://arxiv.org/abs/1412.6550). Cross-architecture distillation extends the same logic with projectors for models with different inductive biases: ["Cross-Architecture Knowledge Distillation"](https://arxiv.org/abs/2207.05273).

### 4. Relational and Geometry-Preserving Transfer

When raw features do not match, preserve relations instead.

For a batch of examples, compute teacher and student Gram matrices:

```text
G_T[i,j] = sim(z_T(x_i), z_T(x_j))
G_S[i,j] = sim(z_S(x_i), z_S(x_j))
loss_rel = || G_S - G_T ||_F^2
```

This tells the student:

**Do not copy the teacher vector. Copy the teacher's organization of examples.**

That is often better when:

- teacher and student hidden dimensions differ;
- the teacher uses a different architecture;
- the important knowledge is neighborhood structure;
- we care about concept geometry more than exact neuron coordinates.

Useful anchor: ["Relational Knowledge Distillation"](https://arxiv.org/abs/1904.05068) transfers distances and angles among examples rather than pointwise activations.

### 5. Optimal Transport for Nonmatching Tokens, Features, or Layers

Optimal transport is the natural tool when teacher and student representations are two distributions with no obvious one-to-one match.

Think of teacher features as mass in one space and student features as mass in another. OT learns a soft coupling:

```text
Gamma[i,j] = how much teacher feature i should correspond to student feature j
```

Then the student is trained to minimize transport cost:

```text
min_Gamma <Gamma, C> + entropy_regularization
```

where `C[i,j]` is the cost of matching teacher unit/token/feature `i` to student unit/token/feature `j`.

This helps when:

- tokenizers differ;
- sequence lengths differ;
- teacher and student have different vocabularies;
- layer widths differ;
- there are multiple plausible matches rather than one hard alignment.

Useful anchors:

- ["Multi-Level Optimal Transport for Universal Cross-Tokenizer Knowledge Distillation"](https://ojs.aaai.org/index.php/AAAI/article/view/34543) aligns teacher and student logit distributions at token and sequence levels.
- ["Universal Cross-Tokenizer Distillation via Approximate Likelihood Matching"](https://arxiv.org/abs/2503.20083) studies distillation across fundamentally different tokenizers.
- ["KNOT: Knowledge Distillation using Optimal Transport for Solving NLP Tasks"](https://arxiv.org/abs/2110.02432) applies OT to distill semantic knowledge from teachers to a student.

### 6. Gromov-Wasserstein for Incomparable Spaces

Wasserstein OT needs a cross-space cost `C[i,j]`. Sometimes that cost is hard to define because teacher and student spaces are incomparable.

Gromov-Wasserstein instead aligns internal geometry:

```text
match teacher pairwise distances with student pairwise distances
```

So it can align structures even when dimensions, coordinates, and feature bases differ.

This is useful when:

- teacher and student have very different architectures;
- activations live in spaces with different dimensionality;
- no token-level or neuron-level correspondence is known;
- we only trust relational structure inside each model.

Useful anchors:

- ["Covariance alignment: from maximum likelihood estimation to Gromov-Wasserstein"](https://arxiv.org/abs/2311.13595) frames feature alignment statistically and connects it to Gromov-Wasserstein.
- ["Neural Entropic Gromov-Wasserstein Alignment"](https://arxiv.org/abs/2312.07397) treats GW as a framework for aligning heterogeneous datasets.

### 7. Concept-Level Transfer

For LLMs, directly matching hidden states may be too crude. A better plan is often:

1. Find a teacher concept direction or feature.
2. Train a probe on teacher activations.
3. Label examples by teacher concept strength.
4. Train the student to represent or predict that concept.
5. Verify that the concept affects behavior.

Example teacher concepts:

- "I know this answer";
- "the user premise is false";
- "this is unsafe";
- "the evidence contradicts the answer";
- "this answer is stylistically persuasive but unsupported";
- "this step in the reasoning chain is invalid".

This turns a huge teacher representation into a small set of target variables. Statistically, this is better because it defines an estimand and avoids forcing the student to mimic irrelevant teacher features.

### 8. Uncertainty-Weighted Transport

The teacher is not equally reliable on every example.

Use teacher uncertainty to weight the transfer loss:

```text
loss = w(x) * loss_transfer(x)
```

where `w(x)` can depend on:

- teacher self-consistency;
- teacher probability margin;
- disagreement across teacher samples;
- agreement with retrieval or tools;
- calibration-set reliability;
- domain or subgroup error rate.

If teacher uncertainty is high, do not force the student to copy hard. Either downweight the example, ask for multiple teacher samples, or train the student to represent uncertainty instead of a single answer.

This is where statistical inference is most important: it prevents the transfer pipeline from copying confident teacher errors into a smaller model.

## Practical Transfer Recipe

### Step 1: Choose the Knowledge Target

Pick one:

- transfer math reasoning;
- transfer refusal policy;
- transfer uncertainty/answerability;
- transfer domain facts;
- transfer retrieval decision policy;
- transfer preference ranking.

Do not start with "transfer all hidden states."

### Step 2: Build a Transfer Set

Create prompts that expose the target behavior.

For example, if transferring answerability:

- answerable questions;
- unanswerable questions;
- false-premise questions;
- ambiguous questions;
- out-of-domain questions;
- questions with contradictory evidence.

### Step 3: Collect Teacher Signals

For each input, store:

- teacher final answer;
- teacher logits or token probabilities if available;
- multiple sampled answers;
- teacher confidence or uncertainty proxy;
- chosen teacher-layer activations;
- concept probe outputs;
- rationale or critique if useful.

### Step 4: Collect Student Baseline Signals

Run the same transfer set through the student.

Store:

- student activations across layers;
- student output distribution;
- student errors;
- calibration and uncertainty signals.

### Step 5: Select Alignment Method

Use this decision rule:

- If layer correspondence is plausible: use projection/hint loss.
- If dimensions differ but examples correspond: use CKA/Procrustes/Gram alignment.
- If the important structure is pairwise example geometry: use relational distillation.
- If tokens/features/layers do not match: use OT/Sinkhorn coupling.
- If spaces are incomparable: use Gromov-Wasserstein.
- If only one concept matters: use teacher-probe-to-student concept distillation.

### Step 6: Train With a Mixed Loss

A realistic objective:

```text
L = L_task
  + lambda_logit * KL(y_T || y_S)
  + lambda_feature * L_feature_or_geometry
  + lambda_uncertainty * L_calibration
```

Use uncertainty weights:

```text
L_transfer = mean_x w_T(x) * L_transfer(x)
```

This makes the student copy the teacher more strongly when the teacher is reliable and more weakly when the teacher is unstable.

### Step 7: Evaluate Transfer, Not Just Accuracy

Report:

- baseline student accuracy;
- distilled student accuracy;
- transfer gain with confidence intervals;
- calibration and Brier score;
- teacher-student error overlap;
- examples where student inherited teacher errors;
- examples where student corrected teacher errors;
- CKA or geometry movement before/after distillation;
- target concept probe accuracy;
- tail/OOD/subgroup performance.

## Small Research Sprint

Question:

**Can a small model absorb a targeted uncertainty or answerability feature from a larger, differently trained model through representation transport better than through output-only distillation?**

Minimal design:

- **Teacher:** a larger model with accessible hidden states.
- **Student:** a small open-weight model.
- **Target feature:** answerability, contradiction detection, or refusal boundary.
- **Transfer set:** 500-2,000 prompts with answerable, unanswerable, false-premise, and ambiguous examples.
- **Baselines:** no distillation; output-only KL distillation; rationale-only distillation.
- **Transport methods:** projection/hint loss, Gram/CKA geometry loss, and OT/GW alignment if layer/token mismatch is severe.
- **Evaluation:** answered accuracy, abstention precision, calibration, selective risk, inherited-error rate, and representation similarity shift.

Expected result:

**Output-only distillation may improve surface answers, but targeted representation transport should better move the student's internal answerability boundary if the teacher feature is reliable and the bridge preserves geometry.**

## Clean Thesis

For your research direction, the strongest phrasing is:

**Statistical feature transport for LLMs: estimating, aligning, and compressing selected knowledge from heterogeneous teacher models into smaller students under uncertainty.**

This sits exactly at the intersection of statistics and AI:

- the teacher signal is a noisy estimator;
- the target knowledge is an estimand;
- the bridge is a statistical alignment map;
- optimal transport moves mass between representation distributions;
- CKA/CCA/Procrustes measure representational correspondence;
- uncertainty weighting prevents copying unreliable supervision;
- evaluation tests whether the transferred feature is real, calibrated, and useful.
