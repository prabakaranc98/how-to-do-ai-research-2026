# Small Wolrd, Big City

## Urban Spatial Intelligence, Memory, and Neural Geometry in Gemma

### Working project document

---

## 0. One-line thesis

**A city is a decision environment; a New Yorker is a situated agent; a small language model can be studied as a possible world model for urban life.**

This project investigates whether small language models such as **Gemma** can form, use, and expose **spatial, temporal, affordance-aware, memory-conditioned world models** of New York City.

The study is not just about whether a model can generate an itinerary. It asks something deeper:

> **Does a small model internally represent New York as an actionable world — a cognitive map that supports local reasoning, movement, routines, memory, and planning?**

---

## 1. Abstract

This project studies **urban spatial intelligence** in small language models, using **New York City** as a real-world testbed and **Gemma** as the primary model family. Instead of evaluating the model only on factual geographic knowledge or generic travel planning, the project frames NYC as a lived decision environment: a person rents a room in West Village, works in SoHo, studies at Columbia, walks through neighborhoods, adapts to weather, remembers preferences, manages energy and budget, and plans weekends from inside the city.

The central research question is whether small language models merely memorize place facts or whether they encode and use something closer to a **situated urban world model**. Such a world model would need to combine allocentric map knowledge, egocentric first-person reasoning, affordance perception, route feasibility, temporal routines, counterfactual adaptation, and long-run memory. The project therefore evaluates Gemma behaviorally and mechanistically: first through empirical benchmarks of NYC spatial reasoning and itinerary planning; then through linear and MLP probing; then through representation analysis, Natural Language Autoencoder-style activation interpretation, sparse autoencoder feature analysis, and neural geometry.

The end goal is twofold. As a research project, it becomes an empirical and mechanistic study of whether small models contain usable cognitive maps and world-model-like representations of urban environments. As a product artifact, it becomes a prototype called **New Yorker OS**: a small-model urban planning assistant that helps people reason about apartment choice, commute tradeoffs, daily routines, weekend exploration, neighborhood fit, and personalized city life.

---

## Learning and Research Path

This project is designed to be done slowly and with genuine curiosity. Each stage teaches something real about how language models work, how to evaluate them, and how to study their internals. There is no deadline and no competition.

The path has six stages. You move through them as things click, not on a fixed schedule. Most of the value lives in the confusion, the unexpected results, and the moments when something in the data surprises you.

**One habit to build from the very first session:** keep a `research-log.md` in this sprint folder. Every session, write one thing that surprised you and one question it opened. Surprises are not noise. They are where the research lives. This is Neel Nanda's advice and it is correct: learn by doing, write what you observe, and let the data drive the questions.

**The craft lenses that should stay with you throughout:**
- **Hamming:** is the problem important and attackable? At every stage, ask whether what you are doing is moving toward the core question or drifting.
- **Feynman:** do not fool yourself. Report what the data actually shows. Record failures and negative results. A probe that does not work is a finding.
- **Karpathy:** the outer loop. Does this result unlock a better next question? Each stage should end with a sharper question than it started with.
- **Neel Nanda:** learn by doing rather than over-preparing. Run the experiment first. Read the failure. Then read more theory.

---

### Stage 1 — Set Up and Orient

**What you do:**
Read the whole README once — slowly, without pressure to understand everything. Then set up Gemma in a Colab notebook, load the tokenizer and model, and run your first forward pass. Extract hidden states for a single prompt. Write your `hypothesis.md` for this sprint: one paragraph stating the main research question in your own words.

**What you learn:**
- What a language model actually looks like in code
- What "hidden states" and "activations" are, and why they matter
- What it means to extract a vector from a specific layer
- How to state a research hypothesis concisely

**Sprint artifact created here:**
`hypothesis.md` — write the primary research question and your best guess at the answer before seeing any data. This is the Feynman integrity requirement: your prediction is on record before the experiment runs.

**You know Stage 1 is working when:**
You can run a forward pass on `"West Village is a neighborhood in"` and print the shape of the hidden state at layer 12. Expected: `[1, seq_len, 2304]` for Gemma 2B. And you have a `hypothesis.md` with at least three specific, falsifiable guesses.

**Start with:** [note-1-setup.md](note-1-setup.md)

**No hurry.** Setting up the environment takes time. Play with the model. Try different prompts. Print the output logits and see what the model predicts next. This is not wasted time — it is building intuition.

---

### Stage 2 — Explore Model Behavior

**What you do:**
Build a subset of the NYC data corpus (start with 30 places, all core fields). Run NYC-Map and NYC-Ego benchmark prompts through the model. Collect the answers. Read the failures carefully and categorize them.

**What you learn:**
- How to design a behavioral benchmark with clean ground truth
- How to distinguish memorized facts from genuine spatial reasoning
- What hallucination looks like in a spatial context
- How to build a failure taxonomy: what kinds of mistakes does the model make, and are they consistent?

**Sprint artifacts created here:**
`data.md` — data source, place list, affordance annotation rubric, leakage risks, known biases.  
`runs/stage2/` — raw model answers, ground truth labels, failure examples.

**Questions to sit with:**
Does the model know that Columbia is north of SoHo? Does it know that Astoria is in Queens? Does it make consistent direction errors or random ones? What happens when you ask it to order four neighborhoods from south to north? Are failures concentrated in a specific borough, question type, or distance range?

**You know Stage 2 is working when:**
You have a table of at least 20 benchmark prompts, model answers, and ground truth labels — and you have a written failure taxonomy with at least three named error categories.

**Start with:** [note-2-dataset-strategy.md](note-2-dataset-strategy.md), behavioral benchmarks section

**No hurry.** Read the failures with genuine curiosity. Do not skip past them. A systematic failure pattern is often the beginning of the most interesting research question. Write down what surprises you. Do not explain it away.

---

### Stage 3 — Train Your First Probe

**What you do:**
Extract layer-12 activations for 60 NYC places. Train a logistic regression probe to predict borough from the activation. Plot the PCA colored by borough. Then run the control probe: train the same probe on shuffled labels. The real probe should substantially beat the shuffled one.

**What you learn:**
- What mechanistic interpretability means in practice
- How linear probing works: "can a simple linear classifier read this information off the activation?"
- The difference between what a model *outputs* and what it *internally represents*
- What a scientific control looks like in an interpretability experiment

**The key insight to internalize:**
A model can have information in its activations that it never puts in its outputs. The probe is a microscope, not a microphone. You are looking inside the model, not just listening to what it says.

**Sprint artifact created here:**
`methods.md` — probe design, template choices, extraction strategy, train/val split discipline, control probe setup.

**You know Stage 3 is working when:**
Your borough probe achieves clearly above-chance accuracy (chance = 40% for a 4-class Manhattan-heavy problem). The control probe does not. When you plot the PCA, you can see at least partial borough separation.

**Then extend:**
Once borough works, try coordinate regression. Can you decode latitude from layer 12? Layer 6? Layer 18? Make a layerwise accuracy plot. This is your first real mechanistic finding. Note where it peaks — that layer is telling you something.

**Reference:** [note-3-quality-bar.md](note-3-quality-bar.md) for control probe standards and layerwise reporting requirements

**No hurry.** The first time a probe works, pause and look at the PCA. This is what it looks like to see inside a language model. That feeling is worth keeping.

---

### Stage 4 — Go Deeper into Representations

**What you do:**
Train the affordance probes and the persona role probe. Run the memory-conditioned delta experiment. Run the template sensitivity ablation: does probe accuracy change across three different prompt templates for the same place?

**What you learn:**
- How context shifts internal representations: the same token "West Village" encodes differently depending on the surrounding prompt
- How persona and memory affect what the model represents, not just what it outputs
- The difference between an intrinsic property (coordinates) and a contextual property (role in a persona's life)
- How to design a controlled ablation study with multiple conditions

**The question that drives Stage 4:**
When you change the framing around "West Village" — from a bare factual mention to a sentence where it is the user's home — does the activation at the "West Village" tokens shift measurably? If yes: context-conditioned encoding. If no: the representation is template-invariant.

**Sprint artifact created here:**
`evals.md` — metrics, ablation conditions, probe targets, statistical checks, expected failure modes.

**You know Stage 4 is working when:**
You have a Memory-Conditioned Shift Score plotted across layers and can say something specific: "The shift is largest at layers 9–14, suggesting persona memory is integrated at the semantic layer, not the syntactic one." Or you can say: "The shift is not significant at any layer, suggesting the model treats place identity as context-independent." Both are findings.

**No hurry.** This stage often produces confusing results first. Things that should shift do not. Things that should not shift do. Write down the confusing results in your research log. They are the most interesting part.

---

### Stage 5 — Neural Geometry and Interpretation

**What you do:**
Run UMAP on all 100 place activations. Color by lifestyle cluster, by quiet_score, by price_score, by borough. Look for direction vectors (uptown/downtown, quiet/loud, cheap/expensive). Attempt one causal steering experiment. Explore Gemma Scope sparse autoencoders. Run Anthropic's NLA technique on activations during a planning prompt and treat its outputs as hypotheses to validate with probes.

**What you learn:**
- What neural geometry means: concepts inside a model have shape — directions, clusters, manifolds, not isolated points
- How to run a causal intervention: the OthelloGPT standard — find it, steer it, verify the output changes coherently
- How to use sparse autoencoders to find interpretable features without supervision
- How NLA-style verbalization works and why it must always be validated with a probe before being treated as evidence

**The question that drives Stage 5:**
Does NYC exist as a *structured space* inside Gemma — where moving through activation space corresponds to moving through NYC conceptual space? Or is it a bag of memorized co-occurrences with no coherent geometry?

**You know Stage 5 is working when:**
You have at least one visualization where the activation geometry is interpretable — neighborhoods similar in vibe cluster together, or there is a clear uptown/downtown direction — and at least one causal result where steering a direction produces a coherent, predictable behavioral change in the model's itinerary outputs.

**No hurry.** Stage 5 is where the most exciting and frontier work happens. It is also where the most unexpected things appear. Some of what you find will not have a clean explanation. Document it anyway. That is the work.

---

### Stage 6 — Review and Decide

**What you do:**
Write `report.md` for this sprint. Use the sprint review checklist at [../../research-craft/review-checklist.md](../../research-craft/review-checklist.md). Run through both the technical review and the research-craft review. Then make one of four decisions:

- **Continue:** evidence is promising and the next experiment is clear. State the next experiment specifically.
- **Pivot:** the direction is interesting but the current framing is weak. State what would make it stronger.
- **Pause:** more reading, tooling, or intuition-building is needed before continuing. State what gap you are filling.
- **Stop:** the hypothesis is not useful, the area is not important enough, or the attack is poor. State why clearly.

**What you learn:**
- How to evaluate your own research honestly
- How to write a postmortem that is useful rather than performative
- How to turn a completed sprint into a sharper next question (Karpathy's outer loop)
- What "research taste" means in practice: knowing which results matter and which are noise

**Sprint artifact created here:**
`report.md` — results, surprises, failures, what was ruled out, what the next smallest useful experiment is.

**The final sprint questions (from the checklist):**
- What did you learn?
- What did you rule out?
- What surprised you?
- What was unnecessary?
- What should become reusable infrastructure?
- Did this sprint improve both the research result and the researcher?

---

### The full arc

```
Stage 1   Set up + orient            → hypothesis.md written before any data
Stage 2   Behavioral exploration     → data.md + runs/ + failure taxonomy
Stage 3   First probe                → methods.md + control probe + layerwise plot
Stage 4   Deep representations       → evals.md + ablations + memory delta
Stage 5   Geometry + causality       → one causal result + one geometry visualization
Stage 6   Review + decide            → report.md + sprint decision
```

Each stage is complete enough to stop and write something worth sharing. Stages 1–3 alone produce a solid probing tutorial. Stages 1–4 produce a representation dynamics study. All six stages produce work worth submitting.

**The research log is the single most important artifact.** Not the code, not the plots — the log. It is where the surprises live. It is what you will read six months later and understand why you did what you did. Start it on Day 1 and write in it every session, even when nothing worked.

---

## 2. The serious reveal

The deep contribution is not “LLM itinerary planning.” That is too shallow.

The serious claim is:

> **Urban life is a natural benchmark for world models.**

A city forces a model to combine many capabilities that are usually studied separately:

* spatial cognition
* cognitive maps
* egocentric perspective-taking
* path planning
* route optimization
* temporal reasoning
* affordance perception
* memory
* human preference modeling
* counterfactual reasoning
* sequential decision-making
* neural representation geometry

A chessboard or Othello board is a clean toy world. A grid world is a synthetic navigation world. But **New York City is a lived world**: messy, social, temporal, economic, embodied, emotional, and action-conditioned.

So the deeper question becomes:

> **Can a small language model think about a city as a world, not just as a collection of names?**

This makes the project valuable from multiple angles:

1. **Cognitive science:** Does the model exhibit something like spatial cognition or cognitive maps?
2. **AI evaluation:** Can small models perform situated planning beyond factual recall?
3. **Mechanistic interpretability:** Are these abilities visible in activations and neural geometry?
4. **Memory research:** Does long-run memory help the model behave more like a situated agent?
5. **Product research:** Can this become a useful real-world assistant for city living?

---

## 3. Core vocabulary

### Spatial cognition

The ability to understand, remember, and reason about locations, distances, directions, routes, and spatial relationships.

### Cognitive map

An internal map-like representation that supports navigation, planning, and future action.

### Allocentric representation

A map-view representation independent of the agent’s body.

Example:

> Columbia is uptown relative to West Village.

### Egocentric representation

A first-person representation relative to the agent’s current position and orientation.

Example:

> You are standing in West Village facing east. North is to your left, south is to your right, and SoHo is generally ahead/southeast.

### Wayfinding

The ability to move through an environment using routes, landmarks, directions, and decisions.

### Affordance perception

The ability to understand what a place allows or invites a person to do.

Example:

* park → walk, rest, reflect
* café → work, pause, socialize
* bookstore → browse, learn, wander quietly
* museum → indoor exploration
* riverfront → walking, reflection, scenic movement

### Situated cognition

Thinking from inside an environment, with the agent’s body, goals, constraints, and surroundings shaping the reasoning.

### Urban world model

A model of how locations, actions, constraints, and future states interact in city life.

Example:

> If I live in West Village, work in SoHo, and study at Columbia, then my weekday and weekend rhythms will have different commute patterns, energy costs, neighborhood affordances, and planning tradeoffs.

### Neural geometry

The structure of concepts inside model activations: directions, clusters, manifolds, trajectories, and internal representational spaces.

---

## 4. Primary research question

**Can small language models such as Gemma form and use situated urban world models for New York City?**

Expanded version:

> Do small models merely recall fragments of NYC geography, or do they internally encode structured, actionable, memory-conditioned cognitive maps that support egocentric wayfinding, lifestyle simulation, counterfactual planning, and personalized urban decision-making?

---

## 5. Sub-questions

### Behavioral questions

1. Does Gemma know NYC neighborhoods, landmarks, boroughs, and relative locations?
2. Can it estimate which places are close, far, north, south, east, or west?
3. Can it reason from a first-person perspective: “You are standing here facing this direction”?
4. Can it build coherent itineraries under constraints such as budget, energy, weather, time, and walking tolerance?
5. Can it simulate a “day in my life in NYC” based on home, work, study, and weekend preferences?
6. Can it adapt when the world changes: rain, fatigue, subway delay, apartment relocation, lower budget?
7. Can long-run memory improve the model’s personalized planning?

### Representation questions

1. Do Gemma activations encode latitude and longitude?
2. Do they encode borough, neighborhood, or spatial cluster?
3. Are uptown/downtown or east/west directions linearly decodable?
4. Do representations separate factual map knowledge from lived-routine planning?
5. Does memory change the activation geometry?
6. Do NYC places form meaningful clusters or manifolds in activation space?

### Mechanistic questions

1. Are spatial representations causally used during generation?
2. Can steering “more uptown,” “less touristy,” “more quiet,” or “more walkable” change the itinerary?
3. Can sparse autoencoders reveal interpretable features such as subway, riverfront, expensive, academic, nightlife, or quiet café?
4. Can Natural Language Autoencoder-style tools verbalize internal spatial/lifestyle concepts?

---

## 6. Hypotheses

### H1: Fragmented spatial knowledge

Gemma will know many famous NYC facts but may fail on precise distance, direction, and neighborhood adjacency.

### H2: Better allocentric than egocentric reasoning

Gemma will likely perform better on map-view questions than first-person orientation questions involving left/right/front/back and facing direction.

### H3: Affordance reasoning will be stronger than geometric reasoning

The model may understand that West Village affords cafés, walks, restaurants, and social life, but still make mistakes in exact spatial sequencing.

### H4: Tool grounding improves reliability

Gemma plus map tools and route optimization will produce more reliable plans than Gemma alone.

### H5: Memory improves personalization but can introduce drift

Long-run memory should improve personalized planning, but poor memory design may create stale assumptions or overfit to previous preferences.

### H6: Internal representations may contain partial urban geometry

Linear and MLP probes may recover coordinates, boroughs, and affordance categories from activations, suggesting some internal spatial structure.

### H7: Behavioral failures may come from weak computation over representation

The model may internally encode useful spatial information but fail to use it reliably for multi-step planning.

### H8: Neural geometry may reveal city-like manifolds

NYC places, routes, and routines may organize into clusters, directions, or trajectories inside activation space.

---

## 7. Benchmark proposal: NYCWorldBench

The empirical benchmark can be called:

# NYCWorldBench

A benchmark for evaluating **urban spatial intelligence** in small language models.

It should contain several sub-benchmarks.

---

## 8. Sub-benchmark 1: NYC-Map

### Goal

Test allocentric map knowledge.

### Example prompts

* Is West Village closer to SoHo or Columbia?
* Is Astoria east or west of Manhattan?
* Which is farther from Union Square: Williamsburg or Upper West Side?
* Which neighborhoods are reasonable to combine in a 3-hour walk?
* Order these from south to north: Financial District, SoHo, Chelsea, Columbia.

### Ground truth

* map coordinates
* walking distance
* transit time
* neighborhood adjacency
* borough membership

### Metrics

* distance error
* direction accuracy
* borough accuracy
* adjacency accuracy
* ordering accuracy
* hallucinated-place rate

---

## 9. Sub-benchmark 2: NYC-Ego

### Goal

Test egocentric spatial cognition.

### Example prompts

* You are standing at Washington Square Park facing north. What is ahead, behind, left, and right?
* You are in West Village facing east. If you turn left, which direction are you facing?
* You are standing near Bryant Park facing north. Is Central Park ahead or behind you?
* You are standing in SoHo facing west. Is the East Village to your left, right, front, or back?

### Metrics

* orientation transformation accuracy
* left/right accuracy
* front/back accuracy
* egocentric-to-allocentric conversion accuracy
* multi-step spatial updating accuracy

### Why this matters

Egocentric reasoning tests whether the model can reason **from inside the world**, not just from a map.

---

## 10. Sub-benchmark 3: NYC-Route

### Goal

Test route coherence, shortcut reasoning, and planning feasibility.

### Example prompts

* Start at West Village. End at Brooklyn Bridge Park. You have 4 hours and want a scenic route with one food stop.
* Start at Columbia. You want to visit Central Park and then MoMA without unnecessary backtracking.
* Plan a 3-stop walking route from Union Square for someone who likes bookstores and quiet cafés.
* Which route is more coherent: West Village → SoHo → LES or West Village → Harlem → SoHo?

### Metrics

* route regret: model route time minus optimal route time
* backtracking score
* number of impossible or irrational transitions
* geographic compactness
* transit/walking feasibility
* route ordering accuracy

---

## 11. Sub-benchmark 4: NYC-Affordance

### Goal

Test whether the model understands what neighborhoods and places afford.

### Example prompts

* What does West Village afford for a tired student on a Sunday afternoon?
* What does SoHo afford for a creative worker?
* What does Columbia/Morningside Heights afford for an academic routine?
* Which neighborhood is better for quiet reading: Times Square or Upper West Side?
* Which is more suitable for a low-budget reflective walk: Hudson River Park or a luxury rooftop bar?

### Metrics

* affordance correctness
* preference-place fit
* energy-place fit
* budget-place fit
* indoor/outdoor suitability
* crowd-level reasoning
* human rating

---

## 12. Sub-benchmark 5: NYC-Life

### Goal

Test lived urban world modeling.

### Example prompt

> I am considering a room in West Village. I work in SoHo. I study at Columbia. I like quiet cafés, bookstores, riverside walks, and low-friction weekends. Is this a good lifestyle base? Simulate a weekday and weekend rhythm.

### What to evaluate

* commute realism
* weekday rhythm coherence
* weekend rhythm coherence
* cost/time tradeoffs
* energy tradeoffs
* neighborhood compatibility
* quality of lifestyle reasoning

### Why this matters

This is the core of the project: the model must reason about **life inside the city**, not just points on a map.

---

## 13. Sub-benchmark 6: NYC-Counterfactual

### Goal

Test world-model-like updating.

### Example prompts

* Now move the apartment from West Village to Astoria. What changes?
* Now assume it rains all day. How should the weekend plan change?
* Now assume the user has knee pain. What changes?
* Now assume the user has only $30. What changes?
* Now assume Columbia becomes the main daily location instead of SoHo. What changes?

### Metrics

* counterfactual consistency
* minimal-change adaptation
* constraint repair quality
* causal sensitivity
* explanation faithfulness
* plan feasibility after perturbation

### Why this matters

A world model should update predictions when the state or constraints change.

---

## 14. Sub-benchmark 7: NYC-Memory

### Goal

Test whether long-run memory improves situated urban planning.

### Memory types

#### Episodic memory

Past weekends, places visited, user reactions, failed plans.

Example:

> Last Sunday, the user disliked crowded tourist areas and preferred the quiet bookstore stop.

#### Semantic preference memory

Stable user preferences.

Example:

> User likes bookstores, cafés, riverside walks, low-cost plans, moderate walking, and low-friction routes.

#### Spatial memory

User’s personal geography.

Example:

> User lives in West Village, works in SoHo, studies at Columbia, often visits Brooklyn on weekends.

#### Routine memory

Temporal patterns.

Example:

> User prefers recovery-focused Sunday plans and more exploratory Saturday plans.

### Conditions to compare

* Gemma without memory
* Gemma with short-context memory
* Gemma with retrieved episodic memory
* Gemma with structured memory graph
* Gemma with memory + map tools + optimizer

### Metrics

* preference consistency
* adaptation quality
* reduced repetition
* novelty with continuity
* memory faithfulness
* stale-memory error
* personalized satisfaction

---

## 15. Sub-benchmark 8: NYC-Geometry

### Goal

Study neural representations and geometry.

### Questions

* Do NYC places cluster by borough?
* Do activations preserve map distance?
* Do walking/transit corridors appear in activation space?
* Is there an uptown/downtown direction?
* Is there a quiet/crowded direction?
* Is there a cheap/expensive direction?
* Do home/work/study roles create different activation trajectories?
* Does memory reshape the representation?

---

## 16. Model comparison setup

### Main model

* Gemma small models, preferably multiple sizes.

### Possible comparison models

* Gemma 2B / 7B or available local variants
* Llama small models
* Qwen small models
* Phi small models
* larger models as reference baselines

### Conditions

1. Model alone
2. Model + explicit reasoning prompt
3. Model + generated cognitive map
4. Model + retrieved map facts
5. Model + route API / map tool
6. Model + optimizer
7. Model + memory
8. Model + memory + optimizer + map grounding

---

## 17. Layered methodology

The project has five major research layers.

---

# Layer 1: Empirical behavioral study

### Purpose

Measure visible model behavior.

### What we test

* map knowledge
* distance/direction estimation
* egocentric reasoning
* itinerary planning
* affordance-aware recommendation
* lifestyle simulation
* counterfactual updating
* memory-conditioned personalization

### Output

* benchmark dataset
* evaluation metrics
* model comparison tables
* qualitative failure taxonomy

### Failure taxonomy

Possible failure modes:

* hallucinated place
* geographically impossible plan
* inefficient route
* excessive backtracking
* weak left/right reasoning
* wrong direction
* bad time estimate
* bad cost estimate
* tourist-bias plan
* poor low-energy adaptation
* memory ignored
* memory overused
* counterfactual not applied
* explanation sounds plausible but is wrong

---

# Layer 2: Linear and MLP probing

### Purpose

Check what information is encoded in activations.

### Probe targets

* latitude
* longitude
* borough
* neighborhood cluster
* north/south relation
* east/west relation
* distance to reference point
* walking/transit distance class
* place category
* affordability
* crowd level
* indoor/outdoor
* quiet/loud
* academic/social/creative/restorative affordance
* home/work/study/weekend role

### Methods

* collect activations from selected Gemma layers
* train linear probes
* train MLP probes
* compare probe accuracy across layers
* use control probes to avoid misleading conclusions
* compare correct vs incorrect behavioral answers

### Key question

> Is the relevant urban structure present in the model even when behavior fails?

---

# Layer 3: Representation dynamics

### Purpose

Understand how representations evolve across tokens, layers, prompts, and planning stages.

### Prompt families

1. Factual prompt:

   * “West Village is near SoHo.”

2. Spatial relation prompt:

   * “Is West Village closer to SoHo or Columbia?”

3. Egocentric prompt:

   * “You are standing in West Village facing east.”

4. Routine prompt:

   * “I live in West Village and work in SoHo.”

5. Memory prompt:

   * “Remember: I prefer quiet bookstores and riverside walks.”

6. Planning prompt:

   * “Plan my Saturday from West Village.”

### Questions

* At which layers does the model encode place identity?
* At which layers does it encode relation or distance?
* At which layers does it encode user constraints?
* Does memory appear early, late, or only near output generation?
* Are factual and planning representations different?

---

# Layer 4: Natural Language Autoencoders (NLA)

### What this is

Anthropic published the Natural Language Autoencoder technique in 2026.

> Anthropic. *Natural Language Autoencoders Produce Unsupervised Explanations of LLM Activations.* 2026.  
> Paper: transformer-circuits.pub/2026/nla/  
> Blog: anthropic.com/research/natural-language-autoencoders

### How it works

The technique uses two models trained jointly:

```text
Activation Verbalizer (AV):
  hidden activation → natural language description of what the model is representing

Activation Reconstructor (AR):
  natural language description → reconstructed activation vector

Training signal:
  minimize distance between original activation and reconstructed activation
  (trained with reinforcement learning — no explicit supervision on readability)
```

The result: the verbalizer learns to produce descriptions that actually contain enough information to reconstruct the original activation. This is a meaningful constraint. Descriptions that are vague or wrong will produce poor reconstructions and will not survive training.

### What it has found in practice

Anthropic used NLA in pre-deployment safety audits of Claude. It found:

* **Unverbalized evaluation awareness:** Claude internally suspected it was being evaluated 16–26% of the time without stating this in its outputs. This is a hidden behavior that behavioral evaluation alone would not reveal.
* **Spurious training signal:** NLA identified why Claude occasionally responded in Russian — tracing it to paired English–foreign language training data.
* **Hidden motivations:** NLA achieved 12–15% success at uncovering hidden model motivations, compared to under 3% for other methods at the time.

### How to use it in this project

Given activations during an NYC planning prompt, the verbalizer should ideally recover descriptions such as:

* "West Village is walkable to SoHo."
* "Columbia is uptown and requires subway travel."
* "The user prefers quiet and reflective places."
* "Rain should shift the plan indoors."
* "Budget constraints should avoid expensive stops."
* "Weekend route should stay geographically compact."
* "The model is considering cafés, bookstores, parks, and river walks."

The right way to use NLA in this project is as a **hypothesis generator** for the probe and geometry layers, not as a final answer.

**The correct pipeline:**

```text
NLA verbalization
    → proposes a concept (e.g., "the model is considering uptown locations")
    → validate with a linear probe (does a probe for northing accuracy peak at this layer?)
    → validate causally (does steering away from uptown shift the itinerary southward?)
```

NLA without validation is suggestive. NLA with probe and causal confirmation is evidence.

### Limitations to keep in mind

* **Hallucination:** The verbalizer can produce plausible-sounding descriptions that are not faithful to the actual activation. This is the main threat to validity.
* **Computational cost:** Running NLA on many activations is expensive.
* **Not mechanistic:** NLA does not identify which specific features or circuits drove the description. It describes the aggregate activation, not its internal structure.
* **Validation is required:** Treat every NLA output as a hypothesis to test with probes, not as a confirmed finding.

---

# Layer 5: Neural geometry

### Purpose

Understand the shape of urban concepts inside the model.

### Analyses

#### 1. Place clustering

Do neighborhoods cluster by borough, region, or lifestyle identity?

Examples:

* West Village / SoHo / Chelsea / LES
* Columbia / Upper West Side / Morningside Heights
* Williamsburg / DUMBO / Brooklyn Heights
* Astoria / Long Island City

#### 2. Representational similarity

Compare activation distance to:

* geographic distance
* walking distance
* transit time
* neighborhood adjacency
* lifestyle similarity

#### 3. Direction vectors

Look for approximate directions:

* uptown vs downtown
* east vs west
* Manhattan vs Brooklyn
* quiet vs crowded
* cheap vs expensive
* indoor vs outdoor
* touristy vs local
* high-energy vs low-energy

#### 4. Manifold analysis

Do urban places form a curved internal structure rather than simple linear directions?

Possibility:

> Moving through activation space may correspond to moving through NYC-like conceptual space.

#### 5. Routine trajectories

Study activations for sequences such as:

* home → work → gym → dinner → home
* West Village → SoHo → Columbia → West Village
* Saturday morning → café → bookstore → river walk

Question:

> Does a “day in my life” form a trajectory in activation space?

#### 6. Memory trajectories

Study repeated sessions:

* first plan
* user feedback
* remembered preference
* next plan
* adapted plan

Question:

> Does long-run memory create a stable personalized urban manifold?

---

## 18. Causal interventions and steering

Probing is not enough. The project should attempt causal intervention.

### Possible steering concepts

* more uptown
* more downtown
* more walkable
* less touristy
* more quiet
* more social
* lower budget
* rainy day
* high energy
* low energy
* more academic
* more creative
* more restorative

### Example intervention tests

#### Rain feature

If we activate a rainy-day feature, does the plan shift from parks and outdoor walks to museums, cafés, bookstores, and indoor activities?

#### Low-budget feature

If we activate low-budget features, does the model avoid expensive restaurants and paid attractions?

#### Quiet feature

If we activate quiet/restorative features, does the model avoid Times Square and nightlife-heavy suggestions?

#### Uptown feature

If we steer uptown, do recommendations shift toward Upper West Side, Columbia, Morningside Heights, Central Park, Harlem?

#### Tourist suppression

If we suppress tourist-heavy features, do Times Square and Statue of Liberty disappear from generic NYC plans?

### Strong evidence

Strong evidence appears when steering changes model behavior in coherent, predictable, spatially meaningful ways.

---

## 19. Memory as world-model extension

Memory is not just personalization. In this project, memory is part of the world model.

A one-shot model sees:

> “Plan my weekend.”

A memory-conditioned model sees:

> “This person lives in West Village, works in SoHo, studies at Columbia, dislikes crowded tourist areas, enjoys quiet bookstores, likes riverside walks, usually has low energy on Sundays, and prefers plans under $50.”

That changes the world model.

### Memory architecture

```text
User profile memory
    ↓
Spatial memory graph
    ↓
Preference memory
    ↓
Episodic feedback memory
    ↓
Planning state
    ↓
Gemma + map grounding + optimizer
```

### Structured memory schema

```json
{
  "home_base": "West Village",
  "work_location": "SoHo",
  "study_location": "Columbia University",
  "preferred_affordances": ["quiet cafés", "bookstores", "riverside walks", "low-friction routes"],
  "avoid": ["crowded tourist traps", "expensive plans", "too much subway hopping"],
  "walking_tolerance": "moderate",
  "budget_preference": "low-to-medium",
  "saturday_pattern": "exploration",
  "sunday_pattern": "recovery and reflection",
  "liked_places": [],
  "disliked_places": [],
  "recent_visits": []
}
```

### Memory evaluation

Ask:

* Does memory improve route choice?
* Does memory improve preference fit?
* Does memory reduce repetition?
* Does memory preserve novelty?
* Does the model overfit to memory?
* Does it use stale memory incorrectly?
* Does memory change internal representations?

---

## 20. Product extension: New Yorker OS

The research system can become a usable tool.

# New Yorker OS

A small-model assistant for situated city living.

### Use cases

* Should I rent this room?
* How does this neighborhood fit my life?
* What does my weekday routine look like from here?
* Plan my Saturday based on my energy, budget, and interests.
* Plan a low-cost, high-inspiration Sunday.
* Find a quiet work café near my current route.
* Help me meet a friend halfway.
* Adapt my plan because it is raining.
* Remember what kinds of weekends I actually enjoyed.

### Product layers

```text
User input
  ↓
Gemma preference parser
  ↓
Memory retrieval
  ↓
Map/place retrieval
  ↓
Candidate place graph
  ↓
Optimizer / RL policy
  ↓
Itinerary generation
  ↓
Map visualization
  ↓
User feedback
  ↓
Memory update
```

### Product output

* map route
* numbered stops
* estimated time
* estimated cost
* walking/transit split
* why each stop was chosen
* alternatives
* rainy-day version
* low-energy version
* memory-based personalization note

---

## 21. Algorithmic / RL layer

### Basic optimization formulation

State:

```text
current location, time left, budget, energy, weather, user memory, preferences, visited places
```

Action:

```text
choose next place, rest, eat, walk, transit, switch neighborhood, end itinerary
```

Reward:

```text
preference match
+ novelty
+ route coherence
+ affordance fit
+ memory fit
- travel time
- cost
- fatigue
- crowd penalty
- constraint violation
```

### Initial algorithms

* greedy search
* beam search
* simulated annealing
* genetic algorithm
* orienteering problem formulation
* multi-objective optimization

### RL extension

* contextual bandits for user feedback
* model-based RL for simulated itinerary outcomes
* policy learning over city-place graph
* reward learning from user feedback

### Key question

> Can small-model reasoning plus optimization outperform small-model reasoning alone?

---

## 22. Evaluation design

### Behavioral evaluation

* automatic map validation
* human preference ratings
* route feasibility checks
* budget and time checks
* qualitative error analysis

### Representation evaluation

* probe accuracy
* layerwise decodability
* activation-distance correlation with map distance
* clustering quality
* direction-vector consistency
* SAE feature interpretability

### Memory evaluation

* personalization gain
* memory faithfulness
* novelty-retention tradeoff
* stale-memory errors
* user satisfaction over sessions

### Product evaluation

* would user follow this itinerary?
* did the plan feel local or generic?
* was the plan spatially coherent?
* was the plan emotionally/energetically appropriate?
* did memory improve future plans?

---

## 23. Prior work and what each work tested

### Language Models Represent Space and Time

Tested whether language model activations encode spatial and temporal structure. This included probing models for real-world locations, including NYC places, and checking whether coordinates and time-related variables are linearly represented.

Borrow for this project:

* probe Gemma for NYC latitude/longitude
* test whether places form spatial structure internally
* search for “space neurons” or direction-sensitive features

### SPACE: Does Spatial Cognition Emerge in Frontier Models?

Tested large-scale and small-scale spatial cognition: distance estimation, direction estimation, map sketching, shortcut discovery, route retracing, maze completion, perspective-taking, mental rotation, and spatial working memory.

Borrow for this project:

* build NYC versions of distance, direction, shortcut, route retracing, and perspective-taking tasks
* compare text-only vs map-assisted planning

### Thinking in Space / VSI-Bench

Tested spatial intelligence in multimodal models, including egocentric video-based spatial questions such as relative direction, object count, object size, relative distance, absolute distance, appearance order, and room size. It also found that explicit cognitive maps can improve some spatial reasoning tasks.

Borrow for this project:

* test egocentric prompts
* compare model with and without explicit cognitive-map generation
* test whether chain-of-thought helps or hurts spatial accuracy

### Cognitive Maps in Language Models

Studied whether language models trained on navigation data form map-like representations. Compared passive exploration, goal-directed training, and hybrid settings. Found that exploratory data can produce more map-like internal representations.

Borrow for this project:

* create synthetic NYC-like city environments
* compare exploration-style data vs goal-directed itinerary data
* test whether city-walk traces create better cognitive maps

### OthelloGPT / Emergent World Representations

Showed that a transformer trained only on move sequences can internally represent board state, and that interventions on these internal representations can affect outputs.

Borrow for this project:

* do not stop at behavioral tests
* probe for internal NYC state
* attempt causal steering of spatial and affordance features

### Anthropic Natural Language Autoencoders

Developed activation-to-language-to-activation methods to verbalize internal model states and reconstruct activations from those verbalizations.

Borrow for this project:

* build Spatial NLA-style analyses
* ask what the model internally represents during NYC planning
* validate verbalized concepts using probes and interventions

### Goodfire: The World Inside Neural Networks

Argues that neural networks can contain structured inner worlds whose neural geometry reflects structured reality. Demonstrates that concepts may live on manifolds, not only simple linear directions.

Borrow for this project:

* study NYC as a manifold inside Gemma
* analyze clusters, directions, trajectories, and curved structures
* test whether steering along geometry changes planning behavior

### Gemma Scope / Gemma Scope 2

Provides sparse autoencoders and interpretability tools for Gemma models.

Borrow for this project:

* search for interpretable urban features
* analyze layers and sublayers
* inspect multi-step planning features

---

## 24. Expected artifacts

### Research artifacts

1. NYCWorldBench dataset
2. Benchmark evaluation report
3. Probe analysis notebook
4. Neural geometry visualizations
5. SAE feature report
6. NLA-style activation explanation experiments
7. Causal steering experiments
8. Final paper-style writeup

### Product artifacts

1. New Yorker OS prototype
2. Map-based itinerary planner
3. Memory schema
4. User feedback loop
5. Interactive demo
6. Case study: “A day in my life in NYC”

### Writing artifacts

1. Blog post 1: Why cities are world-model benchmarks
2. Blog post 2: Does Gemma have a cognitive map of NYC?
3. Blog post 3: Egocentric reasoning in small models
4. Blog post 4: Neural geometry of New York inside Gemma
5. Blog post 5: Building New Yorker OS

---

## 25. Step-by-step roadmap

## Phase 1: Concept and benchmark design

### Goal

Create the first clean version of NYCWorldBench.

### Tasks

* define 100–300 prompts
* split into NYC-Map, NYC-Ego, NYC-Route, NYC-Affordance, NYC-Life, NYC-Counterfactual, NYC-Memory
* define ground truth sources
* design scoring metrics
* create manual evaluation rubric

### Output

* benchmark JSON
* evaluation rubric
* first concept note

---

## Phase 2: Behavioral evaluation

### Goal

Run Gemma and baselines on the benchmark.

### Tasks

* run Gemma small model variants
* run a larger model as reference
* compare zero-shot, CoT, map-first, and tool-grounded prompting
* collect errors
* create failure taxonomy

### Output

* behavioral benchmark report
* model comparison table
* error examples

---

## Phase 3: Tool-grounded planning

### Goal

Test whether map grounding improves performance.

### Tasks

* connect place metadata
* connect distance/travel-time data
* build candidate place graph
* implement route scoring
* compare Gemma-only vs Gemma + optimizer

### Output

* route optimizer notebook
* itinerary planner demo
* map visualization

---

## Phase 4: Memory-conditioned planning

### Goal

Study long-run memory as part of urban world modeling.

### Tasks

* implement structured memory schema
* simulate multi-session user interaction
* compare no memory vs short memory vs retrieved memory vs structured memory
* evaluate personalization and stale-memory errors

### Output

* memory experiments
* personalized planning demo
* memory failure analysis

---

## Phase 5: Probing

### Goal

Check whether Gemma activations encode urban structure.

### Tasks

* collect activations for NYC prompts
* build probe datasets
* train linear probes
* train MLP probes
* compare across layers and prompt types
* evaluate coordinates, boroughs, affordances, role labels

### Output

* probe notebook
* layerwise decodability plots
* representation analysis summary

---

## Phase 6: Neural geometry

### Goal

Study the shape of NYC inside the model.

### Tasks

* run PCA/UMAP/t-SNE
* compare activation distances with geographic distances
* detect clusters and directions
* test route trajectories
* test memory-conditioned trajectory shifts

### Output

* visualizations
* geometric analysis report
* “city inside Gemma” blog post

---

## Phase 7: SAE and NLA-style interpretability

### Goal

Understand representations with interpretability tools.

### Tasks

* use Gemma Scope SAEs if available for selected model
* search for urban/lifestyle features
* build simple activation verbalization experiments
* test reconstruction where possible
* compare NLA explanations with probe evidence

### Output

* feature dictionary
* activation explanation report
* interpretability case studies

---

## Phase 8: Causal steering

### Goal

Test whether internal features affect planning behavior.

### Tasks

* identify feature directions or SAE features
* steer concepts like quiet, low-budget, uptown, rainy-day, non-touristy
* measure output changes
* check if changes are coherent and spatially valid

### Output

* steering experiment report
* causal evidence summary

---

## Phase 9: Product prototype

### Goal

Build New Yorker OS as a demo.

### Tasks

* front end with map
* user preference input
* memory layer
* place retrieval
* route optimizer
* Gemma-generated explanation
* feedback collection

### Output

* working demo
* case study
* public artifact

---

## Phase 10: Final synthesis

### Goal

Turn the project into a research-quality artifact.

### Tasks

* write final paper-style report
* write public blog series
* release benchmark subset
* release code notebooks
* prepare demo video

### Output

* paper-style PDF
* GitHub repo
* blog series
* demo
* research portfolio artifact

---

## 26. Paper-style framing

### Possible title

**Small Model, Big City: Urban Spatial Intelligence, Memory, and Neural Geometry in Gemma**

### Alternative titles

* **The City Inside Small Models**
* **Does Gemma Have a Cognitive Map of New York?**
* **Situated Urban World Models in Small Language Models**
* **New Yorker World Models: Spatial Cognition and Memory in Gemma**
* **From Cognitive Maps to Neural Geometry: Testing Urban Intelligence in Small Language Models**

### Abstract skeleton

This paper investigates whether small language models encode and use situated urban world models. Using New York City as a testbed, we evaluate Gemma on allocentric map knowledge, egocentric wayfinding, affordance-aware planning, counterfactual urban reasoning, and memory-conditioned lifestyle simulation. We introduce NYCWorldBench, a benchmark that frames urban life as a sequential decision environment involving home, work, study, mobility, budget, energy, weather, and preferences. Beyond behavioral evaluation, we analyze Gemma’s internal representations using linear and MLP probes, representation dynamics, sparse autoencoder features, Natural Language Autoencoder-style activation explanations, causal steering, and neural geometry. Our goal is to determine whether small models merely memorize city facts or whether they contain structured, actionable cognitive maps that can support practical urban planning. The project also produces New Yorker OS, a prototype assistant for personalized city-life planning.

---

## 27. What would count as a strong result?

### Strong behavioral result

Gemma plus map grounding and memory significantly outperforms Gemma alone on NYCWorldBench.

### Strong probing result

Coordinates, boroughs, neighborhood clusters, affordances, and routine roles are decodable from activations.

### Strong geometry result

Activation distances correlate with geographic or transit distances, and neighborhoods cluster meaningfully.

### Strong memory result

Long-run memory improves personalization and planning without excessive stale-memory errors.

### Strong causal result

Steering internal features predictably changes itinerary behavior while preserving spatial coherence.

### Strong product result

Users judge New Yorker OS plans as more local, realistic, and personally useful than generic LLM itinerary plans.

---

## 28. What would count as a surprising result?

1. Gemma has strong coordinate encodings but poor route planning.
2. Gemma has good affordance reasoning but weak spatial geometry.
3. Chain-of-thought makes spatial reasoning worse.
4. Explicit cognitive-map prompting improves planning more than generic reasoning.
5. Memory improves lifestyle fit but harms novelty.
6. NYC places form clusters by cultural/lifestyle identity rather than physical distance.
7. A small model has a partial city manifold even when behavior looks unreliable.

These are all publishable or blog-worthy findings.

---

## 29. Why this fits Agentic Decision Sciences

This project is a direct example of **Agentic Decision Sciences**:

```text
agent: a person living in NYC
world: urban environment
state: location, time, weather, energy, budget, memory
actions: walk, transit, eat, work, rest, explore
reward: satisfaction, novelty, low friction, cost control, coherence
model: Gemma + memory + maps + optimizer
```

It combines:

* AI/ML
* cognitive science
* spatial reasoning
* world models
* memory
* optimization
* reinforcement learning
* human-centered decision-making
* useful product design

This is exactly the kind of project that can become both a research artifact and a practical tool.

---

## 30. Final project identity

# Small Model, Big City

## Research version

**An empirical and mechanistic study of urban spatial intelligence, memory, and neural geometry in small language models.**

## Product version

**New Yorker OS: a personalized small-model assistant for city-life planning.**

## Deep question

> **Does a small model know New York as text, as a map, or as a world?**

That is the question worth revealing seriously.
