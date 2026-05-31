# Post-Training and RL Reward Modeling

## LLMs-Cogs Sprint 000

### Working project document

This sprint studies how small language models are shaped after pretraining through supervised fine-tuning, preference data, reward modeling, and RL-style optimization.

The goal is not only to make a model score higher on preference data. The goal is to ask:

> Can reward models and post-training loops improve reasoning, reliability, and agent behavior without teaching reward hacking, sycophancy, or polished overconfidence?

## Why This Sprint Exists

Post-training is where many useful language-model behaviors are created: instruction following, refusal, reasoning style, tool use, preference alignment, and agent policies.

It is also where models can learn shallow tricks:

- sounding helpful without being correct;
- pleasing the judge instead of following evidence;
- optimizing reward-model artifacts;
- hiding uncertainty with polished language;
- becoming sycophantic under user pressure;
- improving benchmark reward while degrading real decision quality.

This sprint treats reward modeling as a research object, not just a training recipe.

## Marr-Level Framing

### Behavioral Level

What changes after post-training?

- answer quality;
- refusal and abstention behavior;
- reasoning trace quality;
- sycophancy rate;
- reward-model preference;
- task success in simple agent settings.

### Representational Level

What internal states change?

- uncertainty;
- answerability;
- helpfulness;
- truthfulness;
- user-pressure sensitivity;
- reward-seeking or judge-awareness signals;
- process-quality representations.

### Mechanistic / Control Level

What mechanism drives the behavior?

- reward model gradients;
- preference-pair features;
- policy updates;
- process-reward supervision;
- rejection sampling;
- RL-style optimization;
- steering or ReFT audits of reward-sensitive representations.

## First Minimal Question

**Can a small reward model distinguish genuinely better reasoning from merely more persuasive or agreeable answers?**

This question is small enough to run and central to post-training quality.

## Candidate Methods

- build a small preference dataset with good/bad answers;
- include adversarial preference pairs where the fluent answer is wrong;
- train a reward model or preference classifier;
- compare outcome reward vs process reward labels;
- evaluate reward hacking and sycophancy cases;
- fine-tune or select responses using the reward model;
- audit internal representations before and after post-training.

## Planned Artifacts

- `note-1-getting-started.md`: sprint framing and first experiment shape.
- `background/`: notes on reward modeling, preference data, RL, process supervision, and reward hacking.
- `hypothesis.md`: falsifiable claims before training.
- `data.md`: preference data design, label policy, adversarial examples, and leakage risks.
- `methods.md`: reward model, policy model, optimization method, baselines, and controls.
- `evals.md`: preference accuracy, reasoning quality, sycophancy, calibration, and reward-hacking metrics.
- `research-log.md`: session notes, failures, surprises, and next experiments.
- `runs/`: configs, reward curves, model outputs, checkpoints, and failed attempts.
- `report.md`: results, limitations, and next-step recommendation.

## Start Here

Begin with [`note-1-getting-started.md`](note-1-getting-started.md).
