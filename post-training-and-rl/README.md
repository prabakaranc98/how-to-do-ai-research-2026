# Post-Training and RL

This initiative studies how language models are shaped after pretraining through supervised fine-tuning, preference optimization, reward modeling, RL-style policy improvement, rejection sampling, process supervision, and mechanistic audits.

The goal is not just to optimize a reward score. The goal is to understand how feedback signals change representations, behavior, reasoning quality, sycophancy, uncertainty, and agent control.

## Core Question

**Can post-training and RL improve reasoning, reliability, and agent behavior without teaching reward hacking, polished overconfidence, or judge-pleasing shortcuts?**

## Research Stack

This initiative connects:

- **Representation learning:** what internal states change after post-training?
- **Reward modeling:** what behavior does the reward model actually prefer?
- **RL / policy optimization:** does optimization improve real decision quality or exploit the reward?
- **Mechanistic interpretability:** can hidden-state audits detect sycophancy, uncertainty, judge-awareness, or reward hacking?
- **Agent control:** do post-trained policies make better sequential decisions under feedback?

## Sprint Index

- [`000-reward-modeling/`](sprints/000-reward-modeling/): reward modeling, preference data, RL-style post-training, process supervision, and reward-hacking analysis for small language models.

