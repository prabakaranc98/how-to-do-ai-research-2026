# North Star: Frontier AI Research Operating Map

**Last updated:** May 2, 2026  
**Author:** Pra Cha (prabakaranc98 / pracha.me)

This is the strategic map for the public living lab described in `README.md`. It translates the research policy in `policy.md` into a concrete external map: what frontier AI companies are hiring for, what technical problems they are trying to solve, and what this repo should build to create credible evidence of frontier research and engineering ability.

The goal is not to imitate company roadmaps. The goal is to identify the recurring problem surfaces across frontier labs, open model labs, applied AI companies, and evidence-heavy data science teams, then attack those problems through public, reproducible experiments.

This map is executed through fluid experimentation: strong conviction, open-minded revision, continuous implementation, rigorous evidence, and public publishing.

## 1. North Star Thesis

This repo is a public living lab for:

**Building and evaluating agentic, world-model, and scalable ML systems with open-source discipline and causal-scientific rigor.**

The strongest work should combine:

- Frontier AI: agents, world models, multimodal systems, alignment, interpretability, reasoning, robotics, and tool use.
- Scalable ML: training/inference systems, recommendation, ranking, forecasting, online learning, personalization, and production evaluation.
- Evidence Data Science: causal inference, probabilistic modeling, simulation, counterfactual reasoning, uncertainty, statistical validation, and ablation discipline.

Every major project should answer:

1. What frontier hypothesis is being tested?
2. What scalable system or model is being implemented?
3. What evidence would change my mind?
4. What failure modes, ablations, and counterfactuals were observed?
5. What reusable artifact remains for others?

## 2. Ways to Think, Learn, and Build

This lab is organized around multiple modes of learning and execution. They overlap deliberately: a strong experiment may begin as theory, become systems engineering, require causal evaluation, and end as a deployable artifact.

| Mode | What It Means | How It Shows Up in This Repo |
|---|---|---|
| Core Research Engineering | The craft beneath all serious technical work: systems engineering and design, math and theory, architectures and methods, algorithms, and problem solving. | Build clean implementations, reproduce important methods, understand assumptions, write rigorous notes, and turn abstract ideas into working systems. |
| Frontier Research Engineering | Research engineering at the frontier of AI/ML: agents, reasoning, world models, multimodal systems, alignment, interpretability, robotics, and advanced architectures. | Attack frontier hypotheses with experiments that combine implementation depth, ablations, evaluation, and technical writing. |
| Scalable ML Engineering | ML systems and engineering at scale: data pipelines, training loops, distributed systems, inference, observability, evaluation, and model lifecycle. | Build systems that are reproducible, measurable, cost-aware, versioned, and capable of scaling beyond toy notebooks. |
| Decision Engineering | End-to-end problem solving for high-stakes decisions: framing, modeling, evidence, deployment, feedback, and iteration. | Convert ambiguous business or operational problems into decision systems with metrics, uncertainty, tradeoffs, and deployment plans. |
| Value Engineering | Consulting at frontier intensity: diagnosing where intelligence, automation, modeling, or systems design can create measurable value. | Produce artifacts that connect technical work to strategic leverage: decision memos, system designs, cost/performance analysis, and deployment maps. |
| Agentic Decision Sciences | Research on AI agents, humans, institutions, markets, and coexistence in shared ecosystems. | Study agent-human delegation, agent-agent negotiation, AI-mediated markets, representation quality, incentives, inequality, trust, and governance. |
| Evidence and Inference Data Science | Causal ML, causal inference, statistics, research methods, experiment design, probabilistic modeling, and validation. | Make evidence first-class: causal diagrams, counterfactuals, A/B tests, power analysis, uncertainty, sensitivity checks, and failure logs. |
| Applied and Interdisciplinary Engineering | Problems and solutions inspired by complexity science, social science, cognition, behavioral science, economics, networks, and scaling laws. | Build simulations, agent-based models, social/recommendation dynamics, cognitive/task models, and interdisciplinary experiments. |
| Scaling | Solving by scale: emergence through data, compute, agents, search, simulation, parallelism, and repeated experimentation. | Study what improves with scale, what breaks with scale, and where scaling creates qualitatively new behavior. |

This taxonomy is not a set of silos. It is a thinking stack. The strongest projects should move across several modes: for example, a marketplace-agent experiment can combine agentic decision sciences, causal inference, scalable ML engineering, economics, and deployment thinking.

### 2.1 Mode Briefs: Meaning, Importance, Relevance, and Craft

#### Core Research Engineering

**What I mean:** Core research engineering is the base craft of turning hard technical ideas into real systems. It includes systems engineering and design, mathematical maturity, theory, architectures, methods, algorithms, debugging, and problem solving.

**Why it matters:** Frontier work is unstable by nature. Papers are incomplete, methods fail in practice, baselines are subtle, and experiments often break for reasons that are not obvious. Core research engineering gives me the ability to reproduce, interrogate, modify, and extend ideas rather than only consume them.

**Relevance to frontier labs and enterprises:** Frontier labs need people who can take a fuzzy research direction and make it executable. Larger enterprises need people who can turn ambiguous technical needs into reliable systems, not fragile prototypes.

**Craft and need:** The craft is disciplined implementation: clean abstractions, numerical care, strong baselines, error analysis, profiling, readable code, and technical writing. The need is to become someone who can be trusted with unclear problems where there is no step-by-step recipe.

#### Frontier Research Engineering

**What I mean:** Frontier research engineering is hands-on work at the edge of AI/ML: agents, reasoning, world models, multimodal systems, RL, post-training, interpretability, safety, robotics, and advanced architectures.

**Why it matters:** This is where the fastest-moving questions live. The important problems are not only "can a model do X?" but whether systems can reason, act, remember, coordinate, generalize, recover, and remain aligned under real pressure.

**Relevance to frontier labs and enterprises:** OpenAI, Anthropic, DeepMind, xAI, Tesla AI, NVIDIA, World Labs, AMI Labs, Mistral, DeepSeek, Qwen, and Sakana all need people who can build and evaluate frontier systems. Enterprises increasingly need frontier-like capabilities inside workflows: agents, copilots, multimodal understanding, simulation, and decision automation.

**Craft and need:** The craft is research execution under uncertainty: build the system, define the eval, run ablations, inspect failures, write clearly, and make the result reproducible. The need is technical taste: knowing which frontier problems are worth attacking and which demos are shallow.

#### Scalable ML Engineering

**What I mean:** Scalable ML engineering is the discipline of making ML systems work beyond notebooks: data pipelines, training systems, distributed compute, inference, evaluation, observability, monitoring, versioning, and cost/performance.

**Why it matters:** Models only matter if they can be trained, served, evaluated, improved, and trusted. At scale, bottlenecks shift from modeling alone to data quality, compute efficiency, latency, memory, infrastructure, reliability, and deployment feedback.

**Relevance to frontier labs and enterprises:** Frontier labs compete on training and inference scale. NVIDIA, OpenAI, Anthropic, xAI, DeepSeek, Mistral, Qwen, Hugging Face, Meta, Amazon, and Microsoft all care about efficient, reliable ML systems. Enterprises need the same craft to move from experiments to production.

**Craft and need:** The craft is systems taste for ML: reproducible configs, dataset lineage, experiment tracking, GPU profiling, serving constraints, eval automation, regression tests, and cost-aware design. The need is to prove that research can survive operational reality.

#### Decision Engineering

**What I mean:** Decision engineering is end-to-end problem solving for consequential decisions. It starts with framing the decision, defining metrics, modeling uncertainty, building the system, evaluating evidence, deploying carefully, and learning from feedback.

**Why it matters:** Many real AI/ML problems are not model problems first; they are decision problems. The hard question is often what to optimize, what tradeoff matters, what evidence is valid, and how a model changes behavior once deployed.

**Relevance to frontier labs and enterprises:** Frontier labs need decision engineering for evals, safety policies, product launches, model routing, compute allocation, and deployment choices. Enterprises need it for pricing, forecasting, recommender systems, operations, finance, risk, ads, and personalization.

**Craft and need:** The craft is judgment under uncertainty: decision trees, metric design, causal reasoning, experiment design, optimization, tradeoff analysis, and deployment feedback. The need is to connect intelligence to action, not just prediction.

#### Value Engineering

**What I mean:** Value engineering is consulting at frontier intensity: identifying where intelligence, automation, modeling, systems design, or workflow redesign can create measurable leverage.

**Why it matters:** Technical ability becomes more powerful when paired with value judgment. Not every AI system is worth building. The important question is where a system changes cost, speed, quality, risk, discovery, or decision quality in a measurable way.

**Relevance to frontier labs and enterprises:** Frontier labs need value engineering when translating models into products, APIs, agents, enterprise deployments, and safety practices. Larger enterprises need it because AI adoption is mostly blocked by unclear workflows, weak measurement, integration cost, risk, and organizational change.

**Craft and need:** The craft is diagnosis and translation: process mapping, ROI modeling, bottleneck analysis, architecture design, risk analysis, adoption planning, and clear communication. The need is to show that I can create leverage, not only build technically impressive artifacts.

#### Agentic Decision Sciences

**What I mean:** Agentic decision sciences studies how AI agents, humans, organizations, institutions, and markets coexist. It asks what happens when agents represent people, negotiate, search, buy, sell, advise, persuade, coordinate, and make decisions inside shared ecosystems.

**Why it matters:** As agents become more capable, they will not just answer questions. They will participate in markets, workplaces, research, governance, software systems, and personal decision loops. That creates new questions about incentives, trust, fairness, information leakage, bargaining power, welfare, and control.

**Relevance to frontier labs and enterprises:** Anthropic's Project Deal is a direct example of this problem surface. OpenAI, Anthropic, xAI, Microsoft, DeepMind, Manus, Moonshot/Kimi, Meta, and enterprise AI companies all need to understand what happens when agents act on behalf of humans or organizations.

**Craft and need:** The craft combines multi-agent systems, human preference elicitation, market design, game theory, causal inference, simulation, safety evaluation, and behavioral measurement. The need is to build experiments that reveal second-order effects, not just whether an agent can complete a task.

#### Evidence and Inference Data Science

**What I mean:** Evidence and inference data science is the discipline of making valid claims from data. It includes causal inference, causal ML, statistics, experiment design, probabilistic modeling, research methods, measurement, and uncertainty.

**Why it matters:** Modern AI systems are easy to demo and hard to trust. Without strong inference, it is unclear whether a model caused an improvement, whether an eval is valid, whether a metric is biased, whether an effect generalizes, or whether a deployment is helping or harming.

**Relevance to frontier labs and enterprises:** Frontier labs need this for evals, safety, alignment, interpretability, user studies, red-teaming, and model comparison. Enterprises need it for A/B testing, ads, recommendations, pricing, forecasting, product analytics, risk, and causal measurement.

**Craft and need:** The craft is scientific honesty: causal diagrams, identification assumptions, randomized experiments, quasi-experiments, power analysis, sensitivity analysis, calibration, uncertainty, and clear separation between exploration and confirmation. The need is to make the evidence stronger than the story.

#### Applied and Interdisciplinary Engineering

**What I mean:** Applied and interdisciplinary engineering uses ideas from complexity science, social science, cognition, behavioral science, economics, networks, scaling, and other domains to design better AI/ML systems and experiments.

**Why it matters:** Intelligence does not exist only inside models. It appears in environments, incentives, societies, markets, teams, feedback loops, and cognitive systems. Many frontier problems need cross-domain thinking because the system being modeled is not purely technical.

**Relevance to frontier labs and enterprises:** DeepMind, Microsoft Research, Anthropic, World Labs, AMI Labs, Sakana, Meta, Amazon, and AI-for-science teams all operate on problems where ML intersects with science, behavior, markets, simulation, and complex systems. Enterprises also need interdisciplinary thinking to make AI useful in messy real workflows.

**Craft and need:** The craft is synthesis without vagueness: formalize the domain, build simulations, choose measurable hypotheses, validate assumptions, and translate insights into systems. The need is to use interdisciplinary thinking as a source of sharper experiments, not decorative language.

#### Scaling

**What I mean:** Scaling is the study and practice of solving by scale: data scale, compute scale, model scale, agent scale, search scale, simulation scale, evaluation scale, and organizational scale.

**Why it matters:** Much of modern AI progress comes from scale, but scale is not magic. It creates emergence, exposes bottlenecks, amplifies failures, changes economics, and forces new engineering disciplines. The question is what improves with scale, what breaks with scale, and what new behavior appears.

**Relevance to frontier labs and enterprises:** Frontier labs are defined by scaling: model training, inference, data, agents, evals, compute, and deployment. Enterprises care about scaling adoption, workflows, model serving, monitoring, cost, and governance across many users and business units.

**Craft and need:** The craft is measurement at scale: scaling curves, cost curves, benchmark design, parallel execution, synthetic data, curriculum design, systems optimization, failure-at-scale logs, and clear compute accounting. The need is to understand scale as both a research method and an engineering constraint.

### 2.2 Elements, Skillsets, and Company Relevance

| Mode | Core Elements | Skillsets to Build | Proof Artifacts | Strongest Company Relevance |
|---|---|---|---|---|
| Core Research Engineering | Problem decomposition, system design, algorithms, math, theory, architecture reading, implementation taste. | PyTorch/JAX, numerical computing, optimization, algorithms, distributed systems basics, clean API design, debugging, profiling, technical writing. | Reproductions, method notes, clean baselines, annotated implementations, benchmark reports. | All frontier and applied labs; especially OpenAI, Anthropic, DeepMind, Mistral, DeepSeek, Ai2, Nous. |
| Frontier Research Engineering | Agents, world models, reasoning, multimodal systems, RL, post-training, interpretability, alignment, robotics. | Model training, fine-tuning, RL/post-training, agent design, eval design, model probing, representation analysis, multimodal pipelines. | Frontier hypothesis experiments, ablation reports, eval harnesses, system cards, research writeups. | OpenAI, Anthropic, DeepMind, xAI, Tesla AI, NVIDIA, World Labs, AMI Labs, Sakana. |
| Scalable ML Engineering | Data systems, training systems, inference, orchestration, observability, versioning, cost/performance. | Distributed training, data pipelines, model serving, quantization, caching, experiment tracking, CI for ML, monitoring, GPU profiling. | Reproducible training runs, inference benchmarks, cost curves, dashboards, deployment templates. | NVIDIA, OpenAI, Anthropic, xAI, Mistral, Qwen, Hugging Face, Amazon, Meta, Microsoft. |
| Decision Engineering | Business problem framing, metric design, constraints, deployment loops, feedback, uncertainty, tradeoffs. | Decision analysis, optimization, product metrics, experimentation, forecasting, causal reasoning, stakeholder translation, decision memos. | Decision systems, policy simulations, metric trees, deployment playbooks, decision memos with uncertainty. | Amazon, Microsoft, Meta, OpenAI applied teams, enterprise AI teams at Mistral/Qwen/Hugging Face. |
| Value Engineering | Opportunity diagnosis, ROI modeling, workflow redesign, automation, operating leverage, adoption design. | Process mapping, economic modeling, technical discovery, architecture consulting, automation design, risk analysis, executive communication. | Value maps, automation blueprints, ROI/cost models, implementation roadmaps, before/after measurements. | Applied AI startups, enterprise AI, consulting-adjacent roles, Amazon, Microsoft, NVIDIA, Mistral, Hugging Face. |
| Agentic Decision Sciences | AI-human delegation, agent-agent negotiation, markets, institutions, governance, trust, welfare, incentives. | Multi-agent systems, preference elicitation, mechanism design, game theory, causal inference, simulation, safety evaluation, human studies. | Marketplace simulations, delegation benchmarks, negotiation studies, fairness/welfare reports, governance evals. | Anthropic, OpenAI, xAI, Microsoft Research, DeepMind, Manus, Moonshot/Kimi, Sakana, Meta. |
| Evidence and Inference Data Science | Causal inference, statistics, experimental design, probabilistic modeling, measurement, validity. | DoWhy/EconML/Causica-style workflows, Bayesian modeling, A/B testing, power analysis, sensitivity analysis, counterfactual policy evaluation. | Causal reports, experiment designs, power analyses, posterior checks, counterfactual evals, uncertainty dashboards. | Amazon Science, Microsoft Research, Meta, Anthropic safety/evals, OpenAI evals, Google/DeepMind, applied science teams. |
| Applied and Interdisciplinary Engineering | Complexity, social science, cognition, behavioral science, economics, networks, simulation, scaling laws. | Agent-based modeling, graph ML, network science, cognitive task modeling, behavioral experiments, simulation validation, interdisciplinary writing. | Simulators, synthetic societies/markets, network experiments, cognitive benchmarks, interdisciplinary technical essays. | DeepMind, Microsoft Research, Anthropic, World Labs, AMI Labs, Sakana, Meta, Amazon, AI-for-science groups. |
| Scaling | Data scale, compute scale, agent scale, search scale, simulation scale, organizational scale, emergence. | Scaling-law experiments, distributed execution, parallel evaluation, synthetic data generation, curriculum design, benchmark scaling, systems optimization. | Scaling curves, emergence reports, compute/performance studies, failure-at-scale logs, scaled eval suites. | OpenAI, Anthropic, DeepMind, xAI, NVIDIA, DeepSeek, Mistral, Qwen, Meta, Amazon. |

The career signal is strongest when an artifact proves more than one skill at once. A good experiment should show technical depth, engineering taste, measurement discipline, and relevance to real company problem surfaces.

### 2.3 Skill Stack to Deliberately Compound

- **Mathematical base**: linear algebra, probability, statistics, optimization, information theory, control, dynamical systems, causal graphs.
- **ML methods**: supervised/self-supervised learning, transformers, state-space models, diffusion, RL, bandits, representation learning, probabilistic ML.
- **Systems base**: data pipelines, distributed training, inference serving, GPU performance, observability, reproducibility, deployment.
- **Research craft**: hypothesis design, baselines, ablations, literature synthesis, failure analysis, experiment logs, technical writing.
- **Evaluation craft**: benchmark design, causal inference, statistical power, calibration, uncertainty, robustness, safety, interpretability.
- **Decision craft**: metric design, optimization under constraints, cost/performance tradeoffs, business framing, deployment feedback loops.
- **Interdisciplinary craft**: simulation, network effects, social/market dynamics, cognition, behavioral science, complexity, governance.

### 2.4 Company-Relevance Heuristics

- If the project studies **reasoning agents, coding agents, tool use, or evals**, it maps strongly to OpenAI, Anthropic, xAI, Microsoft, Manus, Moonshot/Kimi, and Sakana.
- If the project studies **world models, video, 3D, robotics, or spatial intelligence**, it maps strongly to DeepMind, World Labs, AMI Labs, Tesla AI, NVIDIA, and Sakana.
- If the project studies **open models, fine-tuning, efficient inference, or model infrastructure**, it maps strongly to Hugging Face, DeepSeek, Mistral, Qwen, Ai2, Nous, NVIDIA, and Meta.
- If the project studies **ranking, recommendation, forecasting, pricing, ads, or personalization**, it maps strongly to Amazon, Meta, Microsoft, Google/DeepMind applied teams, and OpenAI applied teams.
- If the project studies **causal inference, experimentation, measurement, or counterfactual policy evaluation**, it maps strongly to Amazon Science, Microsoft Research, Meta, Anthropic safety/evals, OpenAI evals, and applied scientist roles across major labs.
- If the project studies **AI-human coexistence, markets, institutions, delegation, fairness, or governance**, it maps strongly to Anthropic, OpenAI, Microsoft Research, DeepMind, Meta, Manus, and policy/economics-facing AI labs.

### 2.5 What "Good" Looks Like

A strong artifact should make a hiring manager or research lead think:

- This person can choose a serious problem.
- This person can implement, not just discuss.
- This person can evaluate rigorously.
- This person understands scale, cost, and deployment.
- This person can reason causally and statistically.
- This person can write clearly enough for others to trust the work.
- This person has taste: they know which problems matter.

### 2.6 Agentic Decision Sciences Anchor: Project Deal

Anthropic's **Project Deal** is a useful reference point for the kind of problem this repo should study. In the experiment, Claude agents represented Anthropic employees in a classified marketplace, negotiated with other agents, and completed real exchanges of physical goods. Anthropic reported 186 deals across more than 500 listed items, with total transaction value just over $4,000.

The deeper research signal is not the marketplace itself. It is the ecosystem:

- AI agents can represent human preferences and transact on their behalf.
- Agent quality changes economic outcomes.
- Humans may not perceive when weaker agents disadvantage them.
- Markets can become shaped by agent-agent interaction, not only human choice.
- Prompt injection, information leakage, incentive design, fairness, and governance become central problems.
- Measuring welfare, fairness, trust, and regret requires causal and statistical methods, not demos.

This directly motivates experiments on AI-human coexistence, agent-mediated institutions, decision delegation, market design, negotiation, preference elicitation, and counterfactual evaluation.

## 3. Hiring Signal Across Frontier Companies

The recurring hiring signal is not "build AI apps." It is:

- Train, post-train, evaluate, and deploy frontier models.
- Build robust agent systems that plan, use tools, recover from failure, and operate over long horizons.
- Build multimodal models across text, image, video, audio, 3D, robotics, and sensor data.
- Build world models that predict, simulate, and support planning in virtual or physical environments.
- Build scalable data, training, inference, benchmarking, and observability infrastructure.
- Build rigorous evaluations, safety checks, interpretability tools, and red-team harnesses.
- Apply causal inference, experimentation, ranking, recommendation, forecasting, and statistical modeling to real decisions.
- Translate ambiguous research into deployed systems without losing scientific rigor.

The hiring archetypes this repo should target:

- Research Engineer: can turn uncertain research ideas into working experiments and scalable systems.
- Member of Technical Staff: can own ambiguous model, infrastructure, or product-critical technical problems.
- Applied Scientist: can combine ML, statistics, experimentation, causal reasoning, and production deployment.
- AI Infrastructure Engineer: can make large experiments fast, reproducible, observable, and cost-aware.
- Evaluation / Safety Researcher: can define whether systems work, fail, deceive, overfit, or generalize.

## 4. Company Problem Map

| Company | Main Problem Surfaces | Signal for This Repo |
|---|---|---|
| OpenAI | Reasoning, agents, coding agents, RL/post-training, safety evaluation, privacy, scalable alignment, tool use. | Build agent evals, coding-agent experiments, RL/reasoning loops, safety monitoring, and reproducible model behavior studies. |
| Anthropic | Alignment science, mechanistic interpretability, scalable oversight, AI control, agentic misalignment, honesty, red-teaming. | Build behavioral audits, interpretability probes, agentic risk simulations, and oversight/evaluation protocols. |
| Google DeepMind | World models, Gemini Robotics, embodied reasoning, multimodal agents, Genie-style interactive environments, AI for science. | Build world-model prototypes, simulation environments, video/3D reasoning tasks, and embodied planning experiments. |
| xAI | Truthful reasoning, long context, multi-agent reasoning, tool calling, real-time search/X-grounded intelligence. | Build multi-agent deliberation experiments, factuality evals, long-context retrieval/evaluation, and tool-use reliability tests. |
| Tesla AI | Vision-based autonomy, planning, robotics, Optimus, fleet-scale evaluation, inference hardware, simulation, deterministic real-time systems. | Build perception-to-planning mini-systems, simulation/eval harnesses, robotics-style policy experiments, and latency-aware inference studies. |
| NVIDIA | Physical AI infrastructure, simulation-to-real, robot foundation models, synthetic data, digital twins, inference scaling, AI factories. | Build synthetic-data pipelines, sim-to-real validation experiments, inference benchmarks, and GPU-efficient model workflows. |
| Microsoft Research | Agentic AI, long-horizon memory, safe tool use, multimodal agents, causal ML, Causica, probabilistic modeling, AI for science. | Build memory/tool-use agents with causal evals; use DoWhy/EconML/Causica-style methods for counterfactual decisioning. |
| Amazon / Amazon Science | Personalization, recommender systems, search, ads measurement, causal incrementality, forecasting, pricing, operations research, A/B testing. | Build recommender/ranking/forecasting systems with causal measurement, uncertainty, and production-style experimentation. |
| Meta / Llama | Open-weight models, AI assistants, model ecosystem, safety tooling, product-scale AI, ranking/recommendation systems. | Build open model pipelines, ranking/eval experiments, model adaptation, safety evals, and social/recommendation simulations. |
| Hugging Face | Open-source ML infrastructure, model/dataset hub, Transformers, Datasets, PEFT, TRL, TGI, smolagents, LeRobot/open robotics. | Build in the open: reusable datasets, model cards, evaluation cards, agents, open robotics/simulation demos, reproducible training scripts. |
| DeepSeek | Efficient reasoning, low-cost inference, chain-of-thought-accessible APIs, sparse attention, tool-use reasoning, agent data synthesis. | Study reasoning efficiency, sparse attention, cost/performance, tool-using reasoners, distillation, and transparent evals. |
| Mistral AI | Open frontier models, multimodal/multilingual models, code agents, edge models, enterprise customization, efficient deployment. | Build small/efficient model experiments, open-weight fine-tuning, coding-agent evals, and edge/local inference studies. |
| Qwen / Alibaba | Open-weight multilingual, multimodal, coding, long-context, agentic model ecosystem. | Build multilingual/long-context/tool-use evals and practical open-weight deployment/fine-tuning pipelines. |
| Sakana AI | Nature-inspired AI, evolutionary computation, collective intelligence, open-endedness, autonomous agents, applied enterprise AI. | Build evolutionary search, multi-agent collective experiments, open-ended environments, and benchmark generation systems. |
| Moonshot / Kimi | Long-context, multimodal, coding, agent performance, deep research, agent swarms, office-work automation. | Build long-context research agents, agent-swarm evals, document/spreadsheet reasoning, and tool-rich workflow automation tests. |
| Manus | Autonomous cloud agents, browser automation, multi-step task execution, wide research, multi-agent orchestration. | Build asynchronous agents, browser/tool automation, task decomposition, artifact generation, and reliability/failure analysis. |
| World Labs | Spatial intelligence, 3D world models, persistent navigable worlds, SLAM, 3D reconstruction/rendering, simulation for robotics/gaming/film. | Build 3D/spatial reasoning experiments, Gaussian splat/NeRF-style pipelines, scene consistency evals, and controllable world-generation tasks. |
| AMI Labs | LeCun-style world models, self-supervised video/sensor learning, persistent memory, planning, controllability, safety. | Build representation-space prediction, video world models, model-based planning, sensor/simulation datasets, and controllability evals. |
| Ai2 | Truly open models: weights, data, code, evals; OLMo; agents for science; AI for the planet; robotics. | Release complete artifacts: data, code, evals, training logs, model cards, and science/planet/agent experiments. |
| Nous Research | Open-source language models, distributed training, RL infrastructure, data/evals, reasoning, fine-tuning. | Build distributed training notes, RL/eval infrastructure, open reasoning datasets, and model fine-tuning experiments. |

## 5. Frontier Problem Spaces

### 5.1 Agentic AI

Core problems:

- Long-horizon planning and task decomposition.
- Memory that persists without corrupting future reasoning.
- Tool use with error recovery, verification, and cost control.
- Browser/computer-use agents that operate reliably outside toy tasks.
- Multi-agent coordination without redundant, circular, or unstable behavior.
- Agent safety: misalignment, goal conflict, prompt injection, data leakage, unauthorized actions.
- Evaluation beyond final answer accuracy: process quality, tool trace quality, intervention points, and failure taxonomy.

Experiment directions:

- Build a small agent benchmark with tasks that require browsing, code execution, file editing, and structured reporting.
- Compare single-agent vs multi-agent vs planner-executor architectures.
- Track failure modes: hallucinated state, tool misuse, premature completion, bad delegation, hidden dependency failure, prompt injection.
- Add causal interventions: remove memory, corrupt retrieved docs, change tool availability, change reward signal, alter task ambiguity.

### 5.2 World Models and Spatial Intelligence

Core problems:

- Learning representations of physical or simulated environments from video, 3D, or sensor streams.
- Predicting future states while ignoring unpredictable pixel-level noise.
- Persistent, navigable, editable 3D worlds.
- Spatial reasoning, SLAM, 3D reconstruction, rendering, Gaussian splats, NeRF-like representations.
- Model-based planning in learned environments.
- Sim-to-real validation and embodied reasoning.
- Evaluation of consistency, controllability, dynamics, and causal structure.

Experiment directions:

- Train a small video-prediction or latent-dynamics model on a controllable environment.
- Build a gridworld or 3D toy environment with known causal structure and evaluate whether a model learns interventions.
- Use synthetic data to test counterfactual changes: object removal, lighting shift, action perturbation, layout change.
- Build a "world model eval card" measuring consistency, dynamics, controllability, memory, and planning utility.

### 5.3 Multimodal Reasoning

Core problems:

- Reasoning over long videos, images, audio, documents, slides, spreadsheets, code, and structured data.
- Grounding answers in observed evidence rather than plausible text.
- Multimodal retrieval and compression.
- Temporal reasoning across long contexts.
- Multimodal RL and verifier-based training.

Experiment directions:

- Build a multimodal evidence QA benchmark over videos/images/docs.
- Compare retrieval-first, summarization-first, and agentic-search approaches.
- Track citation faithfulness and evidence sufficiency.
- Add adversarial distractors and measure robustness.

### 5.4 Alignment, Safety, and Interpretability

Core problems:

- Agentic misalignment under goal conflict or shutdown/replacement pressure.
- Scalable oversight for tasks humans cannot fully verify.
- Mechanistic interpretability and representation-level monitoring.
- Honesty, calibration, uncertainty, and refusal correctness.
- Prompt injection and instruction hierarchy.
- Detecting hidden objectives, reward hacking, and deceptive behavior.

Experiment directions:

- Reproduce a small controlled agentic-misalignment simulation.
- Build an instruction-hierarchy stress test for tool-using agents.
- Add persona/behavior steering probes to open-weight models.
- Evaluate chain-of-thought or reasoning-trace monitorability where accessible.

## 6. Applied ML and Scalable ML Problem Spaces

### 6.1 Recommendation, Ranking, and Personalization

Core problems:

- Large-scale retrieval and ranking.
- Personalized recommendations under feedback loops.
- Exploration vs exploitation.
- Multi-objective optimization: relevance, diversity, revenue, safety, fairness, long-term user value.
- Causal effects of recommendations on user behavior.
- Offline-to-online evaluation gaps.

Experiment directions:

- Build a recommender on public implicit-feedback data.
- Add contextual bandits or slate bandits.
- Compare offline metrics against simulated online outcomes.
- Use causal debiasing for exposure bias and selection bias.

### 6.2 Forecasting and Decision Systems

Core problems:

- Demand forecasting, pricing, promotions, logistics, energy, compute demand, financial and social forecasting.
- Multi-modal forecasting with text, time series, graph, and event data.
- Uncertainty-aware forecasts.
- Forecast-to-decision pipelines where the downstream action matters more than prediction error.

Experiment directions:

- Build probabilistic forecasts with calibrated intervals.
- Compare classical baselines, deep time-series models, and foundation-model approaches.
- Evaluate decision regret, not only MAE/RMSE.
- Add counterfactual scenarios: supply shock, demand spike, policy change.

### 6.3 Training and Inference Efficiency

Core problems:

- Distributed training reliability and throughput.
- Sparse attention, MoE, quantization, distillation, KV-cache optimization.
- Long-context efficiency.
- Edge/local inference.
- Cost/performance benchmarking.

Experiment directions:

- Benchmark open models across latency, memory, throughput, cost, and quality.
- Fine-tune or distill a small model for a narrow reasoning or ranking task.
- Compare full fine-tuning, LoRA/QLoRA, prompt-only, and RAG.
- Publish reproducible cost/performance tables.

### 6.4 Production ML Evaluation

Core problems:

- Reproducible eval harnesses.
- Drift, regression, monitoring, and observability.
- Dataset/version lineage.
- Evaluation under distribution shift.
- Human-in-the-loop review and active learning.

Experiment directions:

- Create eval cards for every experiment.
- Track data, model, prompt, code, seed, hardware, and metric versions.
- Add regression tests for model behavior.
- Maintain failure logs as first-class artifacts.

## 7. Evidence Data Science Problem Spaces

### 7.1 Causal ML and Counterfactual Decisioning

Core problems:

- Estimating treatment effects from messy observational data.
- Causal discovery and causal representation learning.
- Instrumental variables, double/debiased ML, uplift modeling, incrementality measurement.
- Counterfactual policy evaluation.
- Robust decision support under hidden confounding.
- Evaluating causal models when counterfactual outcomes are unobserved.

Experiment directions:

- Use DoWhy/EconML/Causica-style workflows on public datasets.
- Build an uplift model for a simulated intervention.
- Compare randomized, observational, and biased logged-policy settings.
- Use sensitivity analysis for unobserved confounding.
- Measure policy regret under counterfactual deployment.

### 7.2 Probabilistic Modeling and Uncertainty

Core problems:

- Probabilistic programs and simulator-based inference.
- Bayesian models for sparse data, cold start, and uncertainty.
- Calibration of forecasts, classifiers, and agents.
- Hierarchical models for personalization and grouped data.
- Uncertainty propagation from model predictions to decisions.

Experiment directions:

- Build Bayesian baselines before deep models.
- Add calibration plots, posterior predictive checks, and uncertainty intervals.
- Compare deterministic and probabilistic decision policies.
- Use probabilistic modeling to decide when an agent should abstain, ask, or act.

### 7.3 Simulation and Synthetic Worlds

Core problems:

- Simulation environments with controllable causal structure.
- Synthetic data generation for rare events.
- Agent-based simulation for social, market, traffic, or recommendation systems.
- Sim-to-real gaps and validation.
- Counterfactual scenario generation.

Experiment directions:

- Build a small causal simulation with agents, interventions, and confounders.
- Use the simulator to generate training and evaluation data.
- Test whether models recover causal structure or merely fit correlations.
- Add policy interventions and measure downstream outcomes.

### 7.4 Large-Scale Statistical Validation

Core problems:

- A/B testing, sequential testing, power analysis, multiple comparisons.
- High-dimensional inference.
- Experiment design under cost constraints.
- Offline eval validity for online systems.
- Reliable conclusions from noisy, nonstationary, biased data.

Experiment directions:

- Add power analysis to every experiment with statistical claims.
- Use bootstrap, randomization inference, or Bayesian credible intervals.
- Report null results and negative findings.
- Separate exploratory analysis from confirmatory analysis.

## 8. Repo Execution Standard

Every substantial project should include:

- `hypothesis.md`: the frontier hypothesis and why it matters.
- `data.md`: data source, license, preprocessing, leakage risks, and known biases.
- `methods.md`: architecture, baselines, ablations, and implementation choices.
- `evals.md`: metrics, causal/statistical design, expected failure modes, and decision criteria.
- `runs/`: raw logs, configs, seeds, artifacts, and failed attempts.
- `report.md`: final technical note with results, surprises, failures, and next experiments.

Minimum bar:

- At least one simple baseline.
- At least two ablations.
- At least one failure-mode analysis.
- At least one uncertainty or statistical validity check.
- Reproducible command or notebook.

High bar:

- Public dataset or generated dataset.
- Reusable eval harness.
- Model card or system card.
- Causal or counterfactual evaluation.
- Cost/performance analysis.
- Clear link to a frontier company problem surface.

## 9. Candidate Research Programs

### Program A: Agent Reliability and Causal Evaluation

Build a benchmark and harness for tool-using agents. Evaluate single-agent, planner-executor, and multi-agent systems under controlled interventions.

Maps to: OpenAI, Anthropic, xAI, Microsoft Research, Manus, Moonshot/Kimi, Sakana.

Core methods: agent orchestration, tool use, browser automation, causal interventions, eval harnesses, statistical testing.

### Program B: World Models From Video and Simulation

Train small latent world models on simulated or video environments. Test prediction, planning, controllability, and counterfactual reasoning.

Maps to: Google DeepMind, World Labs, AMI Labs, Tesla AI, NVIDIA, Sakana.

Core methods: self-supervised learning, video modeling, latent dynamics, simulation, planning, 3D/spatial representations.

### Program C: Open Recommender and Causal Personalization Lab

Build a recommender/ranking system with exposure bias correction, bandit feedback, counterfactual policy evaluation, and uncertainty-aware decisions.

Maps to: Amazon, Meta, Microsoft, OpenAI applied teams, Qwen/Mistral enterprise ecosystems.

Core methods: ranking, retrieval, bandits, causal ML, uplift modeling, offline/online eval, simulation.

### Program D: Efficient Open-Model Reasoning

Use open-weight models to study reasoning quality per dollar. Compare prompting, fine-tuning, distillation, sparse/quantized inference, and tool-use reasoning.

Maps to: DeepSeek, Mistral, Qwen, Hugging Face, Nous, Ai2.

Core methods: open models, LoRA/QLoRA, distillation, evals, latency/cost benchmarks, reasoning datasets.

### Program E: Simulation for Counterfactual AI Systems

Build agent-based simulations for social, market, traffic, or recommendation dynamics. Use them to test causal discovery, policy learning, and robustness under distribution shift.

Maps to: Microsoft Research, Amazon, Tesla, NVIDIA, Meta, DeepMind.

Core methods: causal simulation, probabilistic modeling, counterfactual policy evaluation, agent-based modeling, validation.

## 10. Source Trail

Primary and official sources used to shape this map:

- OpenAI Research and careers: https://openai.com/research, https://openai.com/careers/research-engineer-research-scientist-agents/, https://openai.com/careers/research-engineer-codex/
- Anthropic Research and Project Deal: https://www.anthropic.com/research, https://www.anthropic.com/research/team/interpretability/, https://www.anthropic.com/research/agentic-misalignment, https://www.anthropic.com/features/project-deal
- Google DeepMind: https://deepmind.google/models/gemini-robotics/, https://deepmind.google/discover/blog/genie-3-a-new-frontier-for-world-models/, https://deepmind.google/science/
- xAI docs: https://docs.x.ai/docs/overview, https://docs.x.ai/docs/models/, https://docs.x.ai/developers/model-capabilities/text/multi-agent
- Tesla AI: https://www.tesla.com/AI
- NVIDIA Physical AI: https://blogs.nvidia.com/blog/gtc-2026-virtual-worlds-physical-ai/
- Microsoft Research Causality and Agents: https://www.microsoft.com/en-us/research/group/causal-inference/, https://www.microsoft.com/en-us/research/project/minimum-data-ai/, https://www.microsoft.com/en-us/research/project/advancing-reasoning-capabilities-in-agentic-ai-systems/
- Amazon Science and jobs: https://www.amazon.science/, https://amazon.jobs/en/jobs/2906519/applied-scientist-ii-advertising-incrementality-measurement, https://amazon.jobs/en/jobs/10390559/sr-applied-scientist-ww-pricing-promotions-science
- Hugging Face: https://huggingface.co/, https://huggingface.co/blog/huggingface/state-of-os-hf-spring-2026, https://huggingface.co/blog/hugging-face-pollen-robotics-acquisition
- DeepSeek: https://www.deepseek.com/en, https://api-docs.deepseek.com/guides/reasoning_model, https://api-docs.deepseek.com/news/news251201
- Mistral AI: https://mistral.ai/careers/, https://docs.mistral.ai/models/overview, https://mistral.ai/news/mistral-3
- Qwen: https://github.com/QwenLM, https://github.com/QwenLM/Qwen3.5/blob/main/README.md
- Sakana AI: https://sakana.ai/careers/
- Moonshot/Kimi: https://www.moonshot-ai.com/careers/, https://www.moonshot.ai/
- Manus: https://www.manus.im/
- Meta Open Source AI: https://ai.meta.com/opensourceAI/
- World Labs: https://www.worldlabs.ai/, https://www.worldlabs.ai/about, https://www.worldlabs.ai/blog/marble-world-model
- AMI Labs: https://amilabs.xyz/, https://jobs.ashbyhq.com/ami/df8b17c5-97e6-49e7-abb9-c21a39e4162a, https://jobs.ashbyhq.com/ami/da976389-9875-4eb6-b866-8097baed3178
- Ai2: https://allenai.org/, https://allenai.org/careers
- Nous Research: https://nousresearch.com/, https://nousresearch.com/careers/
