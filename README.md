# How to Do AI Research 2026

An open living lab for learning, building, researching, and publishing serious AI/ML/DS work in 2026.

This repo is not a collection of prompted demos or surface-level apps. It is a public research-engineering system for attacking important problems, building real artifacts, evaluating evidence, and developing the craft required for frontier labs and serious applied AI organizations.

## Document Stack

- [`README.md`](README.md): the front door, shared language, and operating loop.
- [`policy.md`](policy.md): the philosophy, principles, and research policy.
- [`northstar.md`](northstar.md): the strategic map of problem spaces, company signals, skill stacks, and candidate programs.

## North Star

Build and evaluate agentic, world-model, and scalable ML systems with open-source discipline and causal-scientific rigor.

The work should compound across three layers:

- **Frontier AI**: agents, reasoning, world models, multimodal systems, alignment, interpretability, robotics, and advanced architectures.
- **Scalable ML**: training/inference systems, recommendation, ranking, forecasting, online learning, personalization, and production evaluation.
- **Evidence Data Science**: causal inference, probabilistic modeling, simulation, counterfactual reasoning, uncertainty, statistical validation, and ablation discipline.

## Operating Loop

1. Map the landscape: understand frontier labs, open model labs, applied AI companies, and evidence-heavy data science teams.
2. Choose an important hypothesis: start from a real problem surface, not a tutorial topic.
3. Build the system: implement the model, agent, simulator, eval harness, or decision pipeline.
4. Evaluate honestly: compare baselines, run ablations, inspect failures, quantify uncertainty, and test counterfactuals.
5. Publish the artifact: code, logs, notes, reports, model cards, eval cards, and lessons learned.
6. Revise the map: let evidence sharpen the next question.

## Active Research Sprint

- [`Sprint 001: Epistemic Reliability in Small Language Models`](sprints/001-epistemic-reliability-small-language-models/note-1-getting-started.md)

## Shared Research Craft

- [`Research Sprint Review Checklist`](research-craft/review-checklist.md): reusable technical and research-craft review for every sprint.

## Research Sprint Method

Research should be steered through focused experimental sprints: phase by phase, hypothesis by hypothesis, with intermediate learning at each step.

1. **Pick an important area**
   Choose an area that matters across research, academia, industry, or the real world. The area should feel personally relevant, but it should also connect to a genuine external problem surface.

2. **Develop intuition before experiments**
   Study the area well enough to understand the fundamentals, the current research directions, the practical constraints, and how the system actually behaves. Intuition is what prevents experiments from becoming random parameter changes.

3. **Learn to question and form hypotheses**
   Good research is not permutation and combination. Simply changing values, swapping models, or trying every option is not enough. The goal is to see the problem differently, identify a better logic, and form a hypothesis that explains why an approach should work.

4. **Explore the intersections**
   Once an intuition and hypothesis emerge, explore nearby intersections: methods, datasets, theory, systems, applications, and adjacent fields. This helps reveal what already exists, what is missing, and where the real opening may be.

5. **Narrow the research surface**
   Shorten the scope into a focused question. Draft experiments that can prove, disprove, or refine the hypothesis. The narrower surface should still connect to the larger problem.

6. **Run research sprints**
   Treat the work as a lineage of experiments, not a scattered marathon. Each sprint should test one hypothesis, produce one concrete artifact, and sharpen the next sprint.

7. **Remove distractions**
   Knowing what not to do is part of the craft. Avoid unnecessary complexity, ornamental tooling, weak side quests, and experiments that do not change the answer. This keeps the work less chaotic and less emotionally draining.

8. **Capture intermediate learning**
   Each phase should produce learning even if the result fails. Write down what changed, what broke, what was ruled out, and what should be tried next.

The practical rhythm:

- choose an important area;
- build intuition;
- ask sharper questions;
- form a hypothesis;
- scan the adjacent landscape;
- narrow the surface;
- run a focused sprint;
- publish the evidence;
- use the result to steer the next sprint.

## Research Advice References

This repo should stay open to advice from people who have already done serious research, while still adapting that advice to this lab's own style.

### Neel Nanda: Research Sprints

Neel Nanda's ["How To Become A Mechanistic Interpretability Researcher"](https://www.alignmentforum.org/posts/jP9KDyMkchuv6tHwm/how-to-become-a-mechanistic-interpretability-researcher) is a useful concrete model for learning research by doing.

Key notes to keep:

- Learn the minimum viable basics, then start doing research. Do not wait until you feel "done" learning.
- Treat early work as practice for research skills, not as a final identity-defining project.
- Use 1-5 day mini-projects to build fast feedback-loop skills: running experiments, debugging, reading outputs, and getting unstuck.
- Move toward 1-2 week research sprints. After each sprint, write a postmortem and either continue because there is momentum or pivot because the evidence is weak.
- Think of research as phases: ideation, exploration, understanding, and distillation.
- Separate fast skills from slow skills. Fast skills include running and debugging experiments; slower skills include prioritization, taste, and knowing when to pivot.
- Do good science, not flashy science: use baselines, read the data, avoid cherry-picking, show limitations, and write the work clearly.
- Public writeups are not decoration. They are part of the research process and become proof of ability.

### Other Advice To Keep Close

- [Richard Hamming, "You and Your Research"](https://www.cs.virginia.edu/~robins/YouAndYourResearch.html): keep asking what the important problems are and why you are not working on them.
- [Michael Nielsen, "Principles of Effective Research"](https://michaelnielsen.org/blog/principles-of-effective-research/): research effectiveness comes from habits, vision, proactive responsibility, environment design, and developing unique combinations of ability.
- [Andrej Karpathy, "A Survival Guide to a PhD"](https://karpathy.github.io/2016/09/07/phd/): research is an outer-loop problem of choosing what is worth solving, not only an inner-loop problem of solving what is already given.
- [Jason Wei, "Practicing AI research"](https://www.jasonwei.net/blog/practicing-ai-research): AI research skill can be decomposed into idea selection, experiment design/execution, writing, and maximizing impact.
- [Richard Feynman, "Cargo Cult Science"](https://calteches.library.caltech.edu/3043/): scientific integrity means actively exposing what could make your result invalid, not only presenting what supports it.
- [Simon Peyton Jones, "How to write a great research paper"](https://www.microsoft.com/en-us/research/academic-program/write-great-research-paper/): writing is part of thinking; a paper needs one clear idea, a strong story, and explicit contributions.
- [John Schulman, "An Opinionated Guide to ML Research"](http://joschu.net/blog/opinionated-guide-ml-research.html): develop research taste, choose problems with headroom, keep experiment notes, and make continual progress.

### Crisp Takeaways From The References

**Neel Nanda**

- Learn just enough to begin; do not hide in endless preparation.
- Use short projects to build the fast skills: coding, debugging, inspecting outputs, and getting unstuck.
- Treat early projects as skill practice, not as proof of identity.
- Work in 1-2 week sprints once the basics are in place.
- Postmortem every sprint: continue only if the evidence and momentum are strong.
- Research has phases: ideation, exploration, understanding, distillation.
- Good science beats flashy science: baselines, limitations, honest writeups, and no cherry-picking.

**Richard Hamming**

- Important work starts with important problems.
- Importance alone is not enough; there must be a plausible attack.
- Keep asking what the important problems in the field are.
- Courage matters: serious work often begins before the path is obvious.
- Luck favors prepared minds, so prepare continuously.
- Presentation and communication are part of doing important work.

**Michael Nielsen**

- Research effectiveness is built through habits, not occasional inspiration.
- Take responsibility for your own research environment and output.
- Build a clear vision, but revise it as evidence accumulates.
- Develop unique combinations of ability; intersections create advantage.
- Read deeply where it matters instead of skimming everything forever.
- Balance contribution with self-development.

**Andrej Karpathy**

- Research is mostly an outer-loop problem: choosing what is worth solving.
- Develop taste by asking what a result would unlock if it worked.
- Aim higher than small incremental improvements when possible.
- A good research direction is fertile: it can produce a lineage of work.
- The best projects are ambitious but still attackable.
- Ownership matters: become "the person who did X."

**Jason Wei**

- AI research is a learnable skill, like music or sport.
- The core skills are idea selection, experiment design/execution, writing, and impact.
- Idea selection is a multiplier; weak topics cap the upside.
- Prefer simple, general, durable ideas over complicated, narrow, short-lived ones.
- Speed matters, but not at the cost of rigor.
- Writing and framing can massively change how work is received.
- Maximize impact through clear communication, open artifacts, talks, posts, and follow-up work.

**Richard Feynman**

- Do not fool yourself; you are the easiest person to fool.
- Report what could invalidate the result, not only what supports it.
- Scientific integrity requires bending over backwards to expose weaknesses.
- Publish negative or inconvenient results when they matter.
- Avoid cargo-cult research: copying the form of science without the substance.

**Simon Peyton Jones**

- Do not wait to write; writing clarifies thinking.
- A paper should have one clear main idea.
- Tell a story: problem, idea, evidence, contribution.
- State contributions explicitly and early.
- Put the reader first; clarity is not optional.
- Use feedback as signal about what the writing failed to explain.

**John Schulman**

- Research taste is more important than raw technical speed.
- Choose problems with headroom, not merely easy next steps.
- Keep daily experiment notes and review them regularly.
- Make continual progress through habits and follow-through.
- Avoid switching projects too often, but know when evidence says to pivot.
- Reimplement important algorithms and papers to build real understanding.
- Split growth between current project execution and broader self-development.

The synthesis:

- Pick important problems with a plausible attack.
- Learn enough to start, then learn through the work.
- Keep feedback loops short.
- Treat experiments as steering signals.
- Maintain scientific honesty.
- Write continuously.
- Build public artifacts that show taste, rigor, and execution.

## Ways to Think, Learn, and Build

- **Core Research Engineering**: systems design, math, architectures, methods, algorithms, and implementation craft.
- **Frontier Research Engineering**: agents, world models, multimodal systems, RL, post-training, interpretability, and safety.
- **Scalable ML Engineering**: data pipelines, training systems, inference, observability, reproducibility, and deployment.
- **Decision Engineering**: end-to-end problem solving under uncertainty, from metrics to deployment feedback.
- **Value Engineering**: finding where intelligence, automation, modeling, or systems design creates measurable leverage.
- **Agentic Decision Sciences**: studying AI agents, humans, markets, institutions, trust, incentives, and coexistence.
- **Evidence and Inference Data Science**: causal ML, causal inference, statistics, experiment design, and probabilistic modeling.
- **Applied and Interdisciplinary Engineering**: complexity science, social science, cognition, behavior, networks, and simulation.
- **Scaling**: understanding what improves, breaks, or emerges through data, compute, agents, search, simulation, and repeated experimentation.

## Hamming Notes: You and Your Research

Inspired by Richard Hamming's 1986 Bellcore talk, ["You and Your Research"](https://www.cs.virginia.edu/~robins/YouAndYourResearch).

- Work on important problems. If the problem does not matter, the work is unlikely to matter.
- Keep a live list of the most important problems in AI/ML/DS, and revisit it often.
- Match important problems with the right timing, tools, methods, and personal preparation.
- Build a vision for where the field is going; without direction, effort becomes random motion.
- Compound knowledge through sustained work. Taste, depth, and technical power accumulate over years.
- Use courage. Serious research requires attacking problems before the path is obvious.
- Work hard, but not blindly. Effort must be applied with strategy, feedback, and judgment.
- Convert failures into better questions. Failed experiments are useful when they sharpen the next hypothesis.
- Talk to serious people about serious problems. Research taste improves through high-quality conversations.
- Learn to communicate the work. Writing, talks, informal explanation, and public artifacts are part of the research.
- Do not hide behind technical depth alone. Important work also requires framing, selling, timing, and clarity.
- Use the system without becoming trapped by it. Learn how institutions, tools, collaborators, and workflows can amplify the work.
- Build for leverage. Choose problems where one good result opens many future paths.
- Stay honest about evidence. Claims should be earned through experiments, ablations, evaluation, and reproducibility.
- Keep asking: What are the most important problems in my field, and why am I not working on them?

## Current Standard

Every serious project in this repo should leave behind:

- a clear hypothesis;
- a working implementation;
- baselines and ablations;
- evidence logs and failure analysis;
- reproducible commands or notebooks;
- a short technical report explaining what changed, what failed, and what should happen next.
