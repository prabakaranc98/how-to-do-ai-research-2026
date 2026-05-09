# Note 3: Research Quality Bar

**Sprint:** 000 — Small Model, Big City  
**Date:** May 8, 2026  
**Status:** Standards and methodology reference

---

## Why This Note Exists

This project aims to produce work that could stand next to ICLR-quality mechanistic interpretability papers. That requires deliberate decisions about methodology — not just "run a probe and report accuracy," but designing experiments with controls, ablations, and falsifiable claims.

This note sets the quality bar by examining what the best recent work does, and translating that into concrete standards for this project.

---

## Goodfire: Manifold Steering (2025)

**Paper:** Manifold Steering (Goodfire, arxiv 2605.05115)

**Core finding:** Linear steering often fails because concepts live on *non-linear manifolds* inside the model. Fitting a steering vector along the mean direction of a concept is insufficient when the concept occupies a curved subspace. Goodfire introduced manifold-aware steering that fits to the activation manifold, producing more coherent steered outputs.

**Why this matters for this project:**

The probe layer (Layer 3) and geometry layer (Layer 5) in this project implicitly assume that concepts like "uptown," "quiet," or "academic" are linearly decodable. That assumption should be tested, not assumed.

**Standard to adopt:**

For every probe, train both a linear probe and an MLP probe (2-layer, small). Compare accuracy. If the MLP probe significantly outperforms the linear one for a given concept (say, by >5% AUC), report it as evidence that the concept is non-linearly encoded. This turns a potential failure mode into an explicit finding.

For the geometry layer, when visualizing place representations with PCA/UMAP, also check whether the *residuals from a linear fit* are structured (e.g., do they correlate with lifestyle cluster or borough?). If linear geometry leaves structured residuals, the manifold is curved.

**What Goodfire gets right methodologically:**
- Behavioral validation of steering results: a steered activation should produce coherent, predictable output change, not just a shift in activation space
- Sparse edits: minimize the number of features changed, which makes interventions interpretable
- Off-manifold detection: show explicitly when linear steering produces incoherent outputs

For the causal steering experiments (Layer 5), adopt Goodfire's behavioral validation standard: a successful steering intervention must produce output changes that are (a) in the predicted direction, (b) coherent as a NYC plan, and (c) minimal in unwanted side effects.

---

## ICLR Quality Bar from Top Mechanistic Interpretability Papers

### 1. Gurnee & Tegmark (2023) — "Language Models Represent Space and Time"

*ICLR 2024. The most directly relevant prior work.*

**What they did right:**
- Multi-scale probing (world → US → NYC) — tests whether representations are hierarchical
- Control probes: trained probes on *shuffled* labels to confirm that probe accuracy comes from the signal, not from dataset structure
- Probe comparison across layers: not just "does the probe work" but "where does it peak?"
- Template sensitivity: tested multiple prompt variants to check invariance

**Standard to adopt for Layer 3:**

Every probe in this project must include:

1. **Control probe:** Train the same probe on shuffled labels. If the shuffled probe achieves >random chance, there is dataset leakage or the model dimension is accidentally correlated. The real probe should substantially outperform the shuffled one.

2. **Layerwise accuracy plot:** Report probe accuracy at every layer (or at strategic layers), not just the best layer. The peak layer is a finding — is spatial structure early (token identity), middle (semantic), or late (task)?

3. **Template invariance check:** For Probes 1–6, compare accuracy under Template A vs Template B. If both templates give similar accuracy, the representation is template-invariant. If not, it reveals context-sensitivity.

4. **Split discipline:** Place-level train/val split, stratified by borough. Never mix templates from the same place across the split.

### 2. OthelloGPT (Li et al., 2023) — "Emergent World Representations"

**What they did right:**
- Causal intervention, not just correlation: after finding that board state is linearly decodable, they patched activations to change the board state and verified that the model's output changed correspondingly
- Binary probes are powerful: a simple yes/no question per position is more interpretable than a complex multi-class probe
- The saliency map (which board positions the model "attends to" when predicting next move) connects probe evidence to behavior

**Standard to adopt for Layer 5 (causal steering):**

Probes alone are not sufficient for causal claims. The standard is:

> "Find it → Ablate it → See if behavior changes."

For this project, the causal experiment structure should be:
1. Identify a feature direction (e.g., "quiet place" direction from Probe 5)
2. Steer activations along or against that direction during generation
3. Measure whether the output plan shifts toward quieter or louder venues (validated against corpus affordance tags)
4. Report the causal effect size: how much did the generated plan's mean quiet_score change per unit of steering?

Without the causal validation step, all probe results are correlational only.

### 3. Cognitive Maps in Language Models (2024)

**What they did right:**
- Compared two hypotheses: does the model form a map-like coordinate system, or does it use path-dependent heuristics?
- Designed stimuli specifically to distinguish these: map-like representations should generalize to novel routes, heuristic representations should fail on shortcuts
- Used phase transition analysis: when does the model shift from map-like to heuristic behavior?

**Standard to adopt for the map vs. heuristic question:**

This project should explicitly test the two hypotheses:

- **H-Map:** Gemma has an internal coordinate-like representation. Evidence: coordinate probe accuracy is high, and probe activations correlate with geographic distance between places.
- **H-Heuristic:** Gemma uses statistical co-occurrence patterns rather than a map. Evidence: coordinate probes fail when tested on place pairs that rarely co-occur in text, probe accuracy degrades for outer borough places (lower training data density).

Design at least one discriminating test: take 10 well-known places (high H-Map probe accuracy expected) and 10 obscure places (e.g., specific outer Queens neighborhoods). Compare probe accuracy. If the gap is large, the model has a heuristic, not a map.

### 4. Recent Manifold Work (TopoLM, ICLR 2025)

TopoLM introduces 2D spatial representations of model units with spatial smoothness loss, revealing semantically interpretable clusters in the model's internal topography.

**Standard to adopt for Layer 5 geometry:**

When running UMAP/PCA on place activations, do not just color by borough. Systematically compare multiple colorings:
- By borough
- By lifestyle cluster
- By quiet_score / price_score / energy_score
- By geographic distance from a reference (Columbia, Times Square)
- By place type (neighborhood vs subway station vs park)

The question is: which of these dimensions explains the most variance in activation geometry? The answer is a finding. If lifestyle cluster explains more variance than geographic distance, the model represents NYC by *vibe*, not by map. If geographic distance dominates, the model has a spatial structure. These are different and interesting results.

---

## The Three-Level Evidence Standard

For any claim in this project, evidence comes in three levels. Claims should be supported at the appropriate level:

**Level 1 — Behavioral (weakest):** The model outputs the right thing. Necessary but not sufficient. A model can output correct places without having an internal spatial representation — it might just have memorized the answer.

**Level 2 — Representational (medium):** A probe trained on activations can decode the relevant feature. Stronger than behavioral — it says something is encoded, not just outputted. But it is still correlational.

**Level 3 — Causal (strongest):** Intervening on the relevant activation changes model behavior in the predicted direction. This is the OthelloGPT standard and the Goodfire standard. For this project, achieving Level 3 evidence for at least one claim (e.g., "the quiet-place representation causally affects itinerary planning") would make the work significantly more compelling.

The expected evidence distribution for this project:
- NYC-Map / NYC-Ego / NYC-Affordance behavioral results: Level 1
- Probe 1–6 accuracy plots: Level 2
- Probe 7 (role in persona context) and Probe 8 (memory delta): Level 2, interpreted toward Level 3
- Causal steering experiments: Level 3 (at least for 2–3 concepts)

A paper with solid Level 2 evidence across all probes + Level 3 evidence for 2 concepts is ICLR-competitive.

---

## Statistical Practices

**Never report raw accuracy without a baseline.** For every probe, compute:
- Majority class baseline (most frequent class in test set)
- Random probe baseline (shuffled labels)
- Best layer accuracy vs. baseline gap

**Use confidence intervals for probe accuracy.** With 20 test places, the 95% confidence interval on a binary probe accuracy is roughly ±20%. Report CIs, especially for probes with small N.

**Report effect sizes, not just p-values.** For the memory delta probe (Probe 8), report cosine distance with a 95% CI across the 100 test pairs, not just "the shift is statistically significant."

**For layerwise plots:** Smooth the plot across layers (e.g., 3-layer moving average) if the raw curve is noisy. Show the smoothed curve with the raw values as dots.

---

## What "ICLR Quality" Means for This Project

Concretely, a submission at ICLR would need:

1. A clear, testable primary claim — something that could be false
2. A dataset that makes it testable — clean, reproducible, with documented ground truth
3. A control condition for every main finding
4. At least one causal experiment (not just probing)
5. An honest failure analysis — where does the model fail, and does that failure tell us something?
6. Comparison against a reasonable baseline (e.g., a larger model, a model with no fine-tuning)
7. Reproducible code and data

This project has all the ingredients. The key is discipline:
- Run controls (shuffled label probes)
- Test the alternative hypothesis (heuristic vs map)
- Attempt at least one causal intervention
- Report failures as findings, not as oversights

---

## Specific Additions to note-2 Dataset Strategy

Based on the quality bar above, the dataset construction in note-2 should be augmented with:

**Addition 1: Obscure vs famous place split**
In Tranche A, explicitly tag each place with `model_likely_knows: true/false` (already in schema). At probe evaluation time, report accuracy separately for famous vs obscure places. If the gap is large, the representation is heuristic, not geometric.

**Addition 2: Out-of-template test set**
For 20% of Tranche A places (held out from probe training), construct a fourth prompt template (Template D) that was never seen during training. Test probe accuracy on Template D. Near-training accuracy = template-invariant encoding. Large accuracy drop = template-specific encoding.

**Addition 3: Causal steering probes**
For 5 places per affordance category (quiet, academic, social, touristy), save the activations and explicitly plan causal steering experiments in Layer 5. These places should be in Tranche A and their affordance labels verified with high confidence.

**Addition 4: Inter-borough distance validation set**
Construct 50 distance questions spanning different borough pairs (Manhattan–Brooklyn, Manhattan–Queens, Brooklyn–Queens). The model's probe accuracy on these cross-borough pairs (vs within-Manhattan pairs from Probe 3) will distinguish map-like from heuristic spatial representations.
