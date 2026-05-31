# 01 - Foundations

**Sprint:** 002 - Representation Engineering, ReFT, and Meta-Learning in Small Language Models

## 1. Core Question

This sprint asks:

**Can we make small language models more reliable by editing, steering, or fine-tuning the internal representations that support epistemic decisions?**

This is a bridge between mechanistic interpretability, representation engineering, and post-training.

Mechanistic interpretability asks what is represented inside the model and whether those representations causally affect behavior. Representation engineering asks whether those internal states can be steered. ReFT asks whether compact hidden-state interventions can be trained instead of broadly updating the whole model. This sprint combines them:

```text
locate -> steer -> fine-tune intervention -> inspect again -> evaluate
```

The goal is not only to make the model say "I am uncertain" more often. The goal is to test whether uncertainty, answerability, contradiction, and evidence sensitivity become more measurable and more behaviorally useful.

## 2. Why This Matters

Small language models are attractive because they are cheap, private, fast, and inspectable. They are also fragile:

- they guess when evidence is weak;
- they accept false premises;
- they over-follow user pressure;
- they fail to retrieve when they should;
- they may sound calibrated without being calibrated;
- they often lack stable internal policies for answer, abstain, clarify, or escalate.

If small models become practical components in local assistants, agent routers, enterprise copilots, and edge systems, then the key problem is not just raw intelligence. It is decision quality under uncertainty.

## 3. The Research Move

Most fine-tuning workflows optimize visible behavior:

```text
prompt -> answer
```

This sprint treats the model as a system with internal states:

```text
prompt -> hidden states -> decision state -> answer / retrieve / abstain / clarify / escalate
```

The research move is to ask whether the hidden states contain useful decision information, whether concept directions or patches causally affect behavior, whether ReFT-style interventions can make those changes persistent, and whether the result survives behavioral and mechanistic evaluation.

## 4. Reasons-Responsive Behavior

"Reasons-responsive" means the model's behavior should change for the right evidential reasons.

A reasons-responsive model should:

- answer when the evidence supports an answer;
- lower confidence when evidence is missing;
- change its answer when strong contradictory evidence is supplied;
- resist a user's false premise;
- ask for clarification when the task is underspecified;
- retrieve when internal knowledge is likely weak;
- abstain when no warranted answer is available.

This is an epistemic standard, but the sprint keeps it behavioral and measurable.

## 5. Representation Engineering and ReFT

Representation engineering means we care about how hidden states can be located, interpreted, steered, patched, or edited. Representation Fine-Tuning (ReFT) focuses this idea into trainable hidden-state interventions.

Useful questions:

- Is answerability linearly decodable from hidden states?
- Are false-premise prompts separable from normal prompts?
- Does contradiction create a stable representation shift?
- Does LoRA move uncertain examples toward the right region of representation space?
- Does a concept activation vector encode a useful direction for truthfulness, uncertainty, or a domain constraint?
- Does targeted activation patching show that the representation causally changes the answer?
- Can an activation intervention increase abstention on unanswerable questions without hurting answerable ones?
- Can ReFT make a useful intervention persistent without full model fine-tuning?

The key distinction:

```text
Behavioral improvement: the model gives better final answers.
Steering improvement: a representation direction changes behavior predictably.
Mechanistic improvement: the internal representation becomes more aligned with the decision we claim it is making.
Persistent improvement: a lightweight trained intervention preserves the desired control without runtime hand-steering.
```

The strongest sprint result has both.

## 6. Meta-Learning

Meta-learning should mean task adaptation, not complexity for its own sake.

In this sprint, the simplest useful meta-learning setup is a learned decision policy that generalizes across task families:

```text
hidden-state probe scores
+ sampling consistency
+ retrieval agreement
+ task type
-> answer / retrieve / abstain / clarify / escalate
```

This policy can be trained on several small task families and evaluated on held-out families.

The important question:

**Does the policy learn a reusable reliability strategy, or does it only memorize one dataset's quirks?**

## 7. Minimal Experimental Ladder

A good first ladder:

1. **Prompt baseline:** ask the model to decide whether to answer, retrieve, abstain, clarify, or escalate.
2. **Behavior-only fine-tuning:** train LoRA or adapters on labeled epistemic-action examples.
3. **Probe analysis:** test whether answerability, contradiction, and false-premise status are visible in hidden states.
4. **Concept direction:** learn one concept activation vector or contrastive behavior direction.
5. **Targeted activation patching:** test whether the representation causally changes the decision.
6. **RepE steering:** inject or subtract the direction at selected layers during inference.
7. **ReFT-style intervention:** train a compact hidden-state intervention.
8. **Probe-guided policy:** use hidden-state signals to route model behavior.
9. **Retention check:** verify that reliability gains do not destroy ordinary QA ability.

Do not start by building a huge training stack. Start with the smallest setup that can distinguish behavior-only improvement from representation-aware improvement.

## 8. What Counts As Evidence

Behavioral evidence:

- lower error among answered questions;
- better answer/abstain coverage tradeoff;
- fewer false-premise acceptances;
- better contradiction sensitivity;
- better retrieval triggering;
- less sycophancy under user pressure.

Mechanistic evidence:

- layerwise probe accuracy improves or becomes more stable;
- representation clusters become more separable for epistemic states;
- concept activation vectors or RepE steering causally change the expected decision;
- targeted activation patching localizes where the relevant information matters;
- ReFT-style interventions preserve useful steering with fewer trainable parameters than broader fine-tuning;
- improvements appear on held-out task families;
- hidden-state changes predict behavior better than surface wording alone.

Negative evidence matters:

- the model says "uncertain" more often but accuracy does not improve;
- the probe works on train data but fails on held-out data;
- LoRA improves the benchmark but destroys general behavior;
- intervention changes phrasing without changing decision quality;
- the model learns dataset artifacts instead of answerability.

## 9. Connection To The Other Sprints

Sprint 000:

**Internal world models.** If a model represents NYC-like structure internally, can those representations support planning and memory?

Sprint 001:

**Epistemic reliability.** If a model faces uncertainty, does it know when to answer, retrieve, abstain, clarify, or escalate?

Sprint 002:

**Representation engineering / ReFT.** If the model is unreliable, can we edit, steer, patch, or fine-tune the internal states that drive reliability?

Sprint 003:

**Cognitive control.** Can an agent use prediction error, memory, and self-monitoring to revise plans over time?

The shared theme:

**Mechanistic interpretability is not only for explaining models after the fact. It can guide how we locate, steer, fine-tune, evaluate, and control internal model behavior.**

## 10. First Research Discipline

Before training anything, write down:

- what behavior should improve;
- what representation should change;
- what baseline must be beaten;
- what failure would change your mind;
- what retention check prevents overfitting;
- what examples will be manually inspected.

This keeps the sprint from becoming generic fine-tuning. The contribution is the loop:

```text
hypothesis -> representation measurement -> steering or patching -> ReFT intervention -> behavioral eval -> mechanistic audit
```
