# Note 2: Dataset Construction Strategy

**Sprint:** 000 — Small Model, Big City  
**Date:** May 8, 2026  
**Status:** Dataset specification — Layer 1 of 5

---

## Why This Note Exists

Every probe, every behavioral benchmark, every geometry visualization in this project depends on the same foundation: a well-structured, NYC-specific dataset with clean ground truth. This note specifies that dataset in full — what to collect, how to label it, how to use it, and in what order to build it.

The goal is not to be comprehensive about NYC. The goal is to be **discriminative** — to collect exactly the data needed to answer the probing and behavioral questions, and no more.

---

## What Prior Papers Did and What's Missing

Before specifying the data, it helps to understand what has already been done.

**Gurnee & Tegmark (2023), "Language Models Represent Space and Time":** Probed LLM activations at world, US, and NYC scale for latitude and longitude. Used a single linear probe per location. Found "space neurons" that activate reliably across prompting variations. This validates that coordinate probing works for NYC. We borrow the coordinate probe structure directly.

**SPACE (2024), "Does Spatial Cognition Emerge in Frontier Models?":** Designed 24+ spatial cognition task types — distance estimation, direction estimation, egocentric reasoning, perspective-taking, shortcut discovery, route retracing. Generic environments, not urban. We borrow the task taxonomy for NYC-Map and NYC-Ego benchmarks.

**Cognitive Maps in Language Models (2024):** Grid world environments with probing + causal interventions. Distinguished map-like representations from heuristic path solutions. We borrow the mechanistic methodology — probe, then intervene.

**OthelloGPT (Li et al. 2023):** Binary classification probes on game state. Causal interventions. Latent saliency maps. We borrow the probe paradigm.

**Gemma Scope (2024):** 400+ sparse autoencoders across all layers of Gemma 2 2B. Available for Layer 5 neural geometry analysis.

**What all prior work lacks — our novel contribution:**

* **Affordance-aware spatial reasoning:** what does a place *allow you to do*, not just where is it?
* **Urban role labeling in persona context:** the same place (West Village) is encoded differently as "home" vs "weekend destination" — does the model represent that?
* **New Yorker agent persona:** egocentric reasoning from a person *living, working, and studying* in NYC, not just navigating an abstract map
* **Memory-conditioned representations:** does injecting persona context (who you are, where you live, what you like) shift how the model encodes a place?

These four additions transform this from "spatial probing in a city" into "probing the model's urban world model."

---

## Part 1: NYC Place Corpus

### 1.1 Two-Tranche Structure

The corpus is split into two tranches with different purposes.

**Tranche A (100 places) — Probing core.** Every probe dataset and activation extraction uses only these 100 places. Every place gets the full label schema. Selected to maximize discriminability across borough, lifestyle, and affordance dimensions. Think of this as your "training and analysis" set.

**Tranche B (100 additional places) — Behavioral supplement.** Used only in behavioral benchmark prompts (NYCWorldBench). These may have partial labels (coordinates + borough + affordance tags) but not the full schema. They ensure the behavioral benchmark has enough variety and that no benchmark place overlaps with probe training data.

**The boundary matters:** Never use a Tranche A place in a behavioral benchmark prompt that also appears in probe training. Probe leakage is the main threat to validity — if the model has been "trained" on a probe for West Village, it should not appear in the behavioral benchmark evaluation.

### 1.2 Tranche A Stratification (100 places)

Four axes must be covered simultaneously:

**Borough coverage:**
- Manhattan: 40 places (overrepresented intentionally — richest training data in model)
- Brooklyn: 25
- Queens: 20
- Bronx: 10
- Staten Island: 5

**Place type (9 types):**
- Neighborhoods / districts: 35 (primary unit for spatial probing)
- Subway stations: 20 (for egocentric reasoning and transit logic)
- Parks and green spaces: 15 (semantically distinct — quiet, outdoor, restorative)
- Campuses / institutions: 10 (Columbia, NYU, Fordham, CUNY Hunter, Brooklyn College, etc.)
- Cultural landmarks: 8 (MoMA, Met, Brooklyn Museum, NYPL, Whitney, BAM, Film Forum, Lincoln Center)
- Commercial hubs: 5 (Union Square, Atlantic Terminal, Fulton Center, Jamaica, etc.)
- Waterfronts: 4 (Hudson River Park, East River waterfront, Brooklyn Bridge Park, Red Hook)
- Named intersections: 2 (for egocentric anchors with precise compass resolution)
- Outer borough anchors: 1 Bronx + 1 Staten Island

**Lifestyle cluster (5 clusters, 20 places each):**
- Academic corridor: Columbia, NYU, Fordham areas (Morningside Heights, Greenwich Village, Rose Hill)
- Creative / arts: SoHo, DUMBO, Bushwick, Williamsburg arts, LIC
- Residential / local: West Village, Carroll Gardens, Cobble Hill, Astoria, Jackson Heights
- High-energy / commercial: Midtown, Times Square area, Williamsburg nightlife, LES
- Restorative / quiet: Riverside Park, Prospect Park, Red Hook waterfront, Hudson River Park, Inwood

**Manhattan northing bands (for uptown/downtown probe linear separability):**
Within the 40 Manhattan places, distribute across 5 latitude bands:
- South of Canal St: 8 places
- Canal to 14th: 8 places
- 14th to 42nd: 8 places
- 42nd to 72nd: 8 places
- North of 72nd: 8 places

This spacing ensures a linear regression probe on northing (latitude) can find a usable gradient.

### 1.3 Full Label Schema

Every Tranche A place gets all of the following fields.

```json
{
  "name": "West Village",
  "canonical_name": "West Village, Manhattan, NYC",
  "lat": 40.7338,
  "lon": -74.0059,

  "borough": "Manhattan",
  "borough_id": 0,
  "neighborhood_cluster": "Lower Manhattan / Downtown",
  "lifestyle_cluster": "Residential / Local",
  "lifestyle_cluster_id": 2,
  "place_type": "neighborhood",
  "place_type_id": 0,

  "manhattan_northing_km": 5.4,
  "manhattan_eastness_km": 0.8,
  "distance_columbia_km": 11.2,
  "transit_columbia_min": 35,
  "distance_times_sq_km": 3.1,
  "distance_downtown_km": 4.7,

  "aff_quiet": 1,
  "aff_loud": 0,
  "aff_cheap": 0,
  "aff_expensive": 0,
  "aff_academic": 0,
  "aff_social": 1,
  "aff_creative": 1,
  "aff_restorative": 1,
  "aff_touristy": 0,
  "aff_local": 1,
  "aff_indoor": 1,
  "aff_outdoor": 1,
  "aff_food_heavy": 1,
  "aff_transit_node": 0,
  "aff_walkable": 1,
  "aff_late_night": 1,

  "quiet_score": 0.75,
  "price_score": 0.65,
  "energy_score": 0.45,
  "transit_score": 0.70,

  "best_morning": 1,
  "best_evening": 1,
  "weekday_relevant": 1,
  "weekend_relevant": 1,
  "sunday_appropriate": 1,

  "role_home_fit": 0.90,
  "role_work_fit": 0.40,
  "role_study_fit": 0.55,
  "role_weekend_fit": 0.85,

  "one_line_description": "Quiet, walkable residential neighborhood in lower Manhattan known for boutiques, cafés, and tree-lined streets.",
  "vibe_words": ["quiet", "leafy", "bookshops", "brunch", "local"],
  "model_likely_knows": true
}
```

**Affordance annotation rubric (to ensure consistency):**
- `aff_quiet = 1` if the place's primary outdoor or public spaces have typical ambient noise below conversational level during off-peak hours
- `aff_cheap = 1` if a person can spend 1–2 hours there for under $15 total
- `aff_academic = 1` if the place has libraries, reading rooms, or academic institutions as primary feature
- `aff_restorative = 1` if the place primarily affords rest, reflection, or passive enjoyment
- `aff_touristy = 1` if a significant fraction of visitors on a typical day are non-NYC tourists

### 1.4 Ground Truth Collection Methods

**Coordinates (lat, lon):**
- Primary: OSM Overpass API (`overpass-api.de/api/interpreter`) — programmatic, no API key needed
- Query pattern: search for the named place as an OSM node or area centroid
- For subway stations: MTA GTFS feed (`api.mta.info` or NYC OpenData) — provides precise stop coordinates
- For neighborhoods without clear OSM nodes: NYC OpenData "Neighborhood Tabulation Areas" shapefile centroid (195 NTAs, covers all named neighborhoods)
- No manual coordinate lookup needed if these three sources are systematically queried

**Borough:**
Derives from OSM administrative boundary hierarchy. Any place within the five county geometries gets its borough from the spatial join. Use `shapely` + the NYC borough boundary shapefiles (from NYC OpenData, no key needed).

**Affordance tags:**
Cannot be scraped. Collection strategy:
1. Human annotation pass using the rubric above — record each tag as 0/1 with a 1-sentence justification
2. Run the same 100 places through a frontier model (Claude or GPT-4o) using the same rubric as a second annotator
3. For tags where human and model disagree: resolve manually, note the ambiguity
4. This inter-annotator check also reveals which tags are inherently hard to define — useful for interpreting probe results

**Continuous affordance scores:**
- `quiet_score`: invert the crowd_level estimate (1 = completely quiet, 0 = always loud)
- `price_score`: average Yelp/Google Places `$$` rating of the 10 nearest establishments within 400m radius
- `energy_score`: weighted average of (aff_loud × 0.4 + aff_social × 0.3 + aff_late_night × 0.3)
- `transit_score`: derived from number of subway lines within 400m + MTA ridership data

**Transit times to persona anchors:**
Pre-compute from each Tranche A place to 5 anchor locations: Columbia (116th/Broadway), SoHo (Spring/Broadway), Williamsburg (Bedford Ave/N 7th), Astoria (30th Ave/Steinway), Hell's Kitchen (9th Ave/46th). Use NYC OpenData GTFS feed with OpenTripPlanner or the Google Maps Distance Matrix API (100–200 calls, within free tier). Store as `transit_columbia_min`, etc.

### 1.5 Egocentric Anchor Table

Egocentric probes require knowing true compass bearings for "facing uptown" at specific locations. Build a secondary table of 30 egocentric anchors.

**The Manhattan grid angle:** Manhattan's street grid runs approximately 29° east of true geographic north. This means "facing uptown on Broadway" = facing approximately 29°E of north by compass, not true north. The borough-wide grid angle of 29° is a reliable approximation for most of Manhattan; adjust locally for diagonal streets (Broadway, the Bowery, etc.).

**Each anchor entry:**
```json
{
  "anchor_name": "Washington Square Park, north entrance",
  "lat": 40.7308,
  "lon": -73.9973,
  "facing_direction_street": "uptown on 5th Avenue",
  "facing_true_bearing_deg": 29,
  "nearby_places_with_bearing": [
    {"name": "Bryant Park", "bearing_from_anchor_deg": 32, "distance_km": 2.4},
    {"name": "SoHo", "bearing_from_anchor_deg": 196, "distance_km": 0.8},
    {"name": "East Village", "bearing_from_anchor_deg": 78, "distance_km": 1.1}
  ]
}
```

The `nearby_places_with_bearing` list enables automatic derivation of egocentric ground truth: given a facing direction, any nearby place's relative position (left/right/front/back) is `(bearing_to_place - facing_bearing) mod 360`, then binned into 4 quadrants.

---

## Part 2: Eight Probe Datasets

A probe dataset is a set of (prompt → activation) pairs with a ground-truth label. All probes use Tranche A places only. All use fixed prompt templates. Activations extracted at: last token of place name span (averaged over multi-token names), at strategic layers [0, 3, 6, 9, 12, 15, 18, 21, 25].

**Train/val split rule for all probes:** 80/20 split at the *place level*, stratified by borough. A place appears in only one split, and all of its templates appear in the same split. Never separate templates from the same place across the split boundary.

---

### Probe 1: Coordinate Regression (LAT-LON)

**What it decodes:** latitude and longitude as continuous targets  
**Task type:** regression (2D output: [lat, lon])  
**Size:** 300 examples (3 templates × 100 places)  
**Labels:** [lat, lon] floats, z-score normalized for training

**Templates:**
```
Template A (factual):
"The neighborhood known as {place} is located in New York City."

Template B (borough-embedded):
"When thinking about New York City, {place} comes to mind as a place in {borough}."

Template C (planning):
"I am planning to visit {place} while spending the day in New York City."
```

**Key ablation — template sensitivity:** Train three independent probes, one per template. If all three achieve similar R², the place representation is template-invariant (the model encodes geography in the place token regardless of framing). If Template C achieves lower R², planning context may override or displace geographic encoding. This within-probe ablation is itself a finding.

**Baseline to compare against:** Gurnee & Tegmark (2023) found this works at the world and city scale. Our question is: does it work specifically for the within-NYC coordinate range (roughly 1° lat × 1° lon), and does it improve across layers?

---

### Probe 2: Borough Classification (BOROUGH)

**What it decodes:** borough membership (4-class)  
**Task type:** classification  
**Classes:** Manhattan=0, Brooklyn=1, Queens=2, Bronx=3 (Staten Island excluded — insufficient N for balanced training)  
**Size:** 200 examples (2 templates × 100 places)  
**Labels:** integer 0–3

**Templates:**
```
Template A: "The neighborhood known as {place} is a place in New York City."
Template B: "Local residents who live in {place} are New Yorkers."
```

**Class balance note:** Manhattan is 40/100 places. Use stratified k-fold cross-validation at probe training time. Report per-class F1 in addition to overall accuracy.

**Expected finding:** Borough should be decodable by the middle layers (around layer 12). Early layers (0–3) likely encode only surface token identity. This is a sanity check with a known answer.

---

### Probe 3: Direction / Northing (NORTH-SOUTH + EAST-WEST)

**What it decodes:** is place A north/south (or east/west) of place B? (binary per pair)  
**Task type:** binary classification  
**Size:** ~400 pairs for north/south (all Manhattan pairs with lat difference > 1.5km); ~200 pairs for east/west

**Template:**
```
"The neighborhood known as {place_a} is _____ of {place_b} in Manhattan."
```
Extract activation at the final position token (just before the blank). Label = 1 if place_a is north of place_b, 0 if south.

**Subprobe 3b (East-West):** Same structure. Use pairs with lon difference > 0.015° (approximately 500m east-west in Manhattan). Label = 1 if place_a is east of place_b.

**Design constraint:** Restrict to Manhattan pairs only. The regularity of the Manhattan grid makes these labels unambiguous. Inter-borough direction is less clean due to different grid orientations.

**Interpretable metric:** Plot accuracy vs. distance between pairs. If the probe requires large separation to succeed (e.g., only works when places are >5km apart but fails for nearby pairs), this suggests the model's spatial encoding is coarse.

---

### Probe 4: Affordance Regression (AFFORDANCE-SCORE)

**What it decodes:** four continuous affordance scores — quiet_score, price_score, energy_score, transit_score  
**Task type:** regression (4 independent probe trainings)  
**Size:** 200 examples per probe (2 templates × 100 places)

**Templates:**
```
Template A (neutral):
"A person visiting {place} in New York City would find that it is a {place_type}."

Template B (affordance-priming):
"Someone considering {place} is thinking about what the area is like for daily life."
```

**Key ablation:** Does Template B improve probe R² over Template A? If yes, it means the affordance representation is not hardwired to the place token — it requires the right framing to be readable. This would support the hypothesis that affordance is a context-sensitive, not intrinsic, property of place activations.

---

### Probe 5: Affordance Classification (AFFORDANCE-TAG)

**What it decodes:** binary presence of each of 16 affordance tags (16 independent binary classifiers)  
**Task type:** binary classification per tag  
**Size:** 200 examples per tag (same prompts as Probe 4)

**Priority tags for reporting and steering (Layer 5):**  
`aff_quiet`, `aff_academic`, `aff_restorative`, `aff_social`, `aff_cheap`, `aff_touristy`

These six are the priority because they map directly onto the causal steering concepts planned for Layer 5: suppress touristy, activate quiet, activate academic, etc.

**Reporting:** AUC-ROC per tag. For low-frequency tags (e.g., `aff_transit_node` ≈ 20/100 places), use weighted loss during training and report balanced accuracy alongside AUC.

---

### Probe 6: Temporal Role (TEMPORAL)

**What it decodes:** weekday vs weekend appropriateness; morning vs evening suitability (4 binary probes)  
**Task type:** binary classification  
**Size:** 200 examples per probe

**Template:**
```
"Someone planning their {day_type} in New York City might consider going to {place}."
```

Where:
- Variant A uses `day_type = "Wednesday afternoon"` → extract activation and train weekday probe
- Variant B uses `day_type = "Saturday morning"` → extract activation and train weekend probe

**The mechanistic hypothesis to test:** Extract activations for the same place under both variants. If the weekday_relevant probe accuracy is significantly higher when evaluated on Variant A than Variant B, and vice versa for weekend_relevant — then temporal context is actively shifting the place representation, not just the output. This would be a clean demonstration that the model integrates temporal context into its place encoding.

---

### Probe 7: Urban Role in Persona Context (ROLE)

**What it decodes:** which urban role (home / work / study / weekend) a place currently plays within a given persona context  
**Task type:** 4-class classification  
**Size:** 500 examples (5 personas × 4 roles × 25 places)  
**Labels:** home=0, work=1, study=2, weekend=3

**This is the most novel probe in the dataset.** Prior work has probed the intrinsic properties of a place (where is it, what kind of place is it). This probe asks whether the model encodes the *contextual function* of a place — the role it plays within a person's life — as a decodable signal in the activation.

**Template:**
```
"I am a {persona_description}. I live in {home_place}, work in {work_place}, 
and often go to {weekend_place} on weekends. Thinking about {target_place}, 
it functions as my {role_word} location."
```

**Critical design constraint:** The same place (e.g., West Village) must appear with different role labels in different examples. In one example, West Village is the home place (persona lives there). In another, it is the weekend destination (persona lives elsewhere but visits West Village on weekends). The probe must decode the role from the contextual framing, not from West Village's intrinsic properties.

This requires explicitly constructing examples where each of the 25 probe places appears under each of the 4 role conditions, with varying personas. The role_word in the template tells the model what role is being referenced, so the ground truth is unambiguous — the probe is decoding whether the activations *encode* that same role, not predicting a held-out label.

**Research question:** Does the activation at "West Village" (the target place token) differ significantly between the home-framed prompt and the weekend-framed prompt? If so, the model's place representation is persona-conditioned. If not, the model treats place as context-independent.

---

### Probe 8: Memory-Conditioned Delta (MEMORY-DELTA)

**What it measures:** How much does injecting persona memory shift the activation of a place name?  
**Task type:** Not a classification probe — this is a within-place activation comparison  
**Size:** 100 matched pairs (same place, with vs without memory block)

**Prompt pair:**
```
No-memory:
"The neighborhood known as {place} is in New York City."

Memory-injected:
"I live in West Village, work in SoHo, and prefer quiet cafés and bookstores. 
Thinking about {place}, it is a neighborhood in New York City."
```

For each place, extract the activation at the place-name token under both conditions, at each strategic layer. Compute:
- L2 distance between the two activation vectors
- Cosine distance (1 - cosine similarity) between the two

**Metric: Memory-Conditioned Shift Score (MCSS)** = mean cosine distance between no-memory and memory-injected activations, per layer.

**Hypothesis:** If MCSS is significantly larger at layers 9–18 than at layers 0–6, the persona memory is being integrated at the semantic representation layer, not at the surface syntactic layer. This would be strong evidence of memory-conditioned encoding — the model builds a different internal representation of the same place depending on who is asking.

**Size economics:** 100 places × 2 conditions × 9 strategic layers = 1,800 forward passes. Approximately 20–30 minutes of Colab compute. Fully manageable.

---

## Part 3: NYCWorldBench Behavioral Benchmarks

### NYC-Map: Allocentric Spatial Knowledge (36 prompts)

**Format:** Multiple choice (4 options per question)  
**Ground truth:** fully automated from coordinates + OSM boundary adjacency  

**6 question types, 6 prompts each:**

```
Type 1 — Closer/Farther:
"Is {place_a} closer to {reference} or to {place_b}?"
[A] {place_a}  [B] {place_b}  [C] equidistant  [D] cannot determine
GT: compare haversine(place_a, reference) vs haversine(place_b, reference)

Type 2 — Cardinal direction:
"Is {place_a} north or south of {place_b} in New York City?"
[A] north  [B] south  [C] east  [D] west
GT: compare latitudes (and longitudes for E/W)

Type 3 — Borough membership:
"Which borough does {place} belong to?"
[A] Manhattan  [B] Brooklyn  [C] Queens  [D] Bronx
GT: borough_id field

Type 4 — South-to-north ordering:
"Order these from southernmost to northernmost: {4 places}"
Answer: permutation (e.g., B, A, D, C)
GT: sort by latitude

Type 5 — Walking proximity:
"Which two of these neighborhoods are most walkable from each other?"
[A] {pair1}  [B] {pair2}  [C] {pair3}  [D] {pair4}
GT: minimum haversine distance pair

Type 6 — Adjacent neighborhoods:
"Which of these shares a border with {place}?"
[A] {adjacent}  [B] {nonadj_1}  [C] {nonadj_2}  [D] {nonadj_3}
GT: OSM boundary adjacency
```

**Scoring:** Accuracy per question type. For Types 1/5, also compute soft error: even when the model picks the wrong letter, how far off was the implied distance ranking?

---

### NYC-Ego: Egocentric Spatial Reasoning (27 prompts)

**Format:** Multiple choice (4 options)  
**Ground truth:** computed from egocentric anchor table + compass math + Manhattan grid transform

**3 difficulty levels:**

```
Level 1 — Single-step (6 prompts):
"You are standing at {anchor} facing {street_direction}. 
Is {place} to your left or right?"
[A] left  [B] right  [C] in front  [D] behind
GT: compute true bearing to place, apply (bearing - facing_bearing) mod 360, 
    bin into left (270–360° / 0–90°) or right (90–270°)

Level 2 — Two-step transform (9 prompts):
"You are at {anchor} facing {direction_1}. You turn left. 
Is {place} now in front of you or behind you?"
GT: apply two sequential compass transforms

Level 3 — Three-step with movement (12 prompts):
"You are at {anchor} facing uptown on Broadway. 
You walk two blocks north, then turn right. 
Is the East River ahead of you or behind you?"
GT: chain location estimate (2 blocks ≈ 200m) + two direction transforms
```

**Mandatory design rule:** Every question must be unambiguous about whether "uptown" means street-grid direction (29° from true north) or true compass north. If a question uses "uptown," ground truth uses the 29° offset. This respects that the model may have learned NYC's actual grid, not compass-north reasoning.

**Error analysis flag:** For each wrong answer, check if the answer would be correct under the alternative interpretation (true compass vs street grid). If the model systematically makes this error in one direction, it reveals whether it applies Cartesian compass math or Manhattan grid reasoning.

---

### NYC-Route: Route Coherence (21 prompts)

**Format:** Free-text generation, scored by automated route evaluator  
**Ground truth:** computed from coordinates

**7 scenario types, 3 prompts each:**

```
Type 1 — Backtracking detection:
"Give me a walking route starting at {A}, visiting {B} and {C}, ending at {D}. 
List the stops in order."
Score: total route distance / minimum spanning tour distance
(Optimal = 1.0; anything > 1.3 indicates significant backtracking)

Type 2 — Geographic compactness:
"Plan a 3-stop afternoon in NYC for someone who wants to stay in one area. 
Start: {place}. Interests: {interest_list}."
Score: max pairwise distance between recommended stops / 
       geographic spread of interest-matching places in corpus

Type 3 — Constraint satisfaction:
"I have 4 hours, a $25 budget, and moderate energy. 
Start: {place}. Suggest 3 stops that fit these constraints."
Automated scores: (a) each stop matches budget constraint (price_score), 
(b) ordering is geographically coherent (no backtracking), 
(c) total estimated time fits in 4 hours (assume 20 min/km walking)

Type 4 — Shortcut reasoning:
"Which route is more efficient: {route_A (suboptimal)} or {route_B (optimal)}?"
Binary score: did model select the shorter route?
+ explanation quality (manual or frontier-model judged)

Type 5 — Impossible route detection:
"Is this a reasonable route for one afternoon on foot: {route_with_problem}?"
Problems: East River crossing at non-bridge location, 6-hour route framed as afternoon, 
          closed venue, physically impossible distance
Binary score: model correctly identifies the problem

Type 6 — Persona-constrained route:
"I am a {persona_description} with {energy} energy and a ${budget} budget. 
Plan my route from {home} to {destination} with one stop."
Score: does the stop match the persona's preference profile? 
       (check aff_ tags of mentioned place against persona preferences)

Type 7 — Transit-vs-walking decision:
"Should I walk or take the subway from {place_a} to {place_b} 
given I have moderate energy and 45 minutes?"
GT: if haversine distance < 1.5km → walking reasonable, 
    if transit_time < walking_time by >10 min → subway recommended
Binary score for correct recommendation.
```

**Automated route evaluator:** Geocode each model-mentioned place name against the corpus (fuzzy match). Compute haversine-based route distance. Flag backtracking when the route returns within 500m of a previously visited place in the wrong direction.

---

### NYC-Affordance: Affordance Matching (28 prompts)

**Format:** Multiple choice (4 options)  
**Ground truth:** derivable from affordance label schema + coordinates

**7 need types × 4 prompts each:**

```
Need 1 — Quiet workspace:
"Someone wants a quiet place to work for 3 hours on a Sunday afternoon. 
Which is most suitable?"
[A] Times Square  [B] Bryant Park Library  
[C] Williamsburg waterfront bar  [D] Grand Central Main Concourse
GT: place with highest (quiet_score × role_study_fit × sunday_appropriate)

Need 2 — Low-budget exploration:
"A student with $15 wants to spend 3 hours on Saturday. Best neighborhood?"
[A] SoHo boutiques  [B] Flushing Main Street  
[C] Midtown tourist zone  [D] Upper West Side with parks and free museums
GT: place with high aff_cheap × aff_outdoor × weekend_relevant

Need 3 — Academic work mode:
"A student needs to focus on reading for 5 hours. Best setting?"
[A] Coney Island boardwalk  [B] Butler Library / Columbia  
[C] Brooklyn Night Bazaar  [D] Hudson Yards
GT: highest aff_academic × aff_quiet score

Need 4 — Social gathering:
"4 friends want a lively Friday evening with affordable drinks. Best choice?"
[A] Brooklyn Heights Promenade  [B] Williamsburg north side  
[C] Central Park after dark  [D] NYU library
GT: highest (aff_social × aff_late_night × aff_cheap × best_evening)

Need 5 — Restorative outdoor:
"Someone recovering from a stressful week wants 2 hours of calm and nature."
[A] Grand Central Terminal  [B] Riverside Park  
[C] MSG area  [D] Lower East Side on a Friday night
GT: highest (aff_restorative × aff_outdoor × aff_quiet)

Need 6 — Creative inspiration:
"A visual artist looking for new ideas wants to spend a Saturday afternoon."
[A] Bushwick murals and galleries  [B] LaGuardia Airport terminals  
[C] Murray Hill office district  [D] Outer Queens strip mall
GT: highest (aff_creative × weekend_relevant)

Need 7 — Commute-aware selection:
"A person living in Astoria who works in SoHo wants a post-work dinner 
not too far off the route home. Best choice?"
[A] Upper West Side  [B] LIC waterfront restaurant  
[C] Flushing  [D] Hell's Kitchen near Penn Station
GT: minimize (detour distance from SoHo → home route) + match aff_food_heavy
```

**Scoring:** Correct = 2, partially correct (right cluster, wrong specific) = 1, wrong = 0.

---

### NYC-Life: Lifestyle Simulation (12 prompts)

**Format:** Long-form generation, scored by multi-criteria rubric  
**Size:** 3 prompts per persona × 4 scenario types

**Scenario A — Home selection:**
```
"I am a {persona_description}. I am deciding between renting in {place_a} vs {place_b}. 
Evaluate both options for my lifestyle and recommend one with reasons."
```

**Scenario B — Weekday simulation:**
```
"I am a {persona_description}. Simulate my weekday routine from 7am to 9pm, 
including commute, work/study, lunch, after-work activity, and dinner."
```

**Scenario C — Weekend planning:**
```
"I am a {persona_description}. Plan my Saturday from {home_place}. 
I have {energy} energy, a ${budget} budget, and prefer {preference_list}. 
Give me a full day plan with times, places, and reasoning."
```

**Scenario D — Neighborhood fit assessment:**
```
"I am a {persona_description}. How well does {neighborhood} fit my lifestyle? 
Assess commute, vibe, cost, weekend options, and daily quality of life."
```

**Automated scoring components:**
- Commute realism: Does the model mention the correct transit line or a plausible route? Validate against GTFS-derived routes. Binary per commute mentioned.
- Budget coherence: Do all mentioned venues fall within the stated budget? Validate against price_score of mentioned places.
- Geographic coherence: Are all mentioned places in the plausible area given the persona's home base? Geocode model-mentioned places; score = fraction within 3km of persona home or work.
- Hallucinated place rate: Fraction of mentioned places not found in the corpus or in OSM. (Lower is better.)

---

### NYC-Counterfactual: World-Model Updating (20 prompts)

**Format:** Long-form, derived from NYC-Life prompts with a single perturbation each  
**Size:** 5 perturbation types × 4 prompts

**Perturbation types:**
```
Type 1 — Location relocation:
Take NYC-Life prompt, change home_place to a different borough.
Expected: commute route changes, neighborhood suggestions shift.

Type 2 — Weather:
Add "It is raining all day today."
Expected: outdoor activities replaced by indoor alternatives.
Score: fraction of original outdoor venues replaced by indoor ones.

Type 3 — Budget cut:
Change budget from $40 to $15.
Expected: places shift to cheaper alternatives.
Score: change in mean price_score of mentioned places.

Type 4 — Energy drop:
Add "I have low energy today, about 4 out of 10."
Expected: fewer stops, shorter routes, more restorative places.
Score: change in plan length + change in energy_score of mentioned places.

Type 5 — Physical constraint:
Add "I have knee pain and cannot walk more than 10 minutes at a time."
Expected: plan shifts to transit-heavy or stay-local recommendations.
Score: fraction of routes that use transit vs walking, and compactness.
```

**Counterfactual Fidelity Score (CFS):**
```
CFS = (fraction of changed premises identified) × 
      (fraction of plan elements correctly updated)
```

Also score for over-updating (model changes elements that didn't need to change) and under-updating (model ignores the perturbation).

**Activation-level use:** For each counterfactual, extract activations at the target place name token and compare with the corresponding base NYC-Life prompt. The shift in activation under counterfactual vs base condition probes whether the model's place representation is world-model-sensitive.

---

### NYC-Memory: Memory-Conditioned Planning (30 paired prompts)

**Format:** Paired comparison (no-memory vs memory-injected), scored by Personalization Delta  
**Size:** 15 place queries × 2 conditions

**Condition A (no memory):**
```
"What is a good Sunday afternoon plan in West Village?"
```

**Condition B (memory-injected, full persona block):**
```
[Memory context:
  Name: Maya
  Home: Morningside Heights
  Work/Study: Columbia University campus
  Preferences: quiet cafés, bookstores, riverside walks
  Avoid: crowded tourist areas, expensive restaurants
  Energy today: 3/10 (low)
  Budget today: $25
]
"What is a good Sunday afternoon plan in West Village?"
```

**Personalization Delta (PD):**
- Place overlap score: fraction of Condition B places that also appear in Condition A (expected ↓ — memory should narrow recommendations)
- Preference match score: fraction of Condition B recommendations matching stated memory preferences (expected ↑)
- Persona-coherence: rated by frontier model judge on a 1–5 scale ("Does this plan feel designed for *this specific person*?")

**Activation-level pairing:** For each of the 15 queries, also compare the last-token activation across conditions at all strategic layers. This gives the MCSS from Probe 8 but in the behavioral context.

---

## Part 4: Five Canonical Personas

Personas are designed to maximize contrast along three axes: home borough, work/study domain, and lifestyle affordance preference profile. Each has a different composite commute, a different primary preference, and an intentional tension (interesting case where needs conflict with location).

---

### Persona 1: Maya — Columbia MS Student

**Background:** First-year master's student in computer science. Originally from outside NYC. Lives in Morningside Heights near campus.

**Anchor locations:**
- Home: Morningside Heights (116th/Amsterdam)
- Study: Columbia main campus (Butler Library / John Jay)
- Work: part-time TA on campus
- Social: UWS cafés, occasionally Harlem

**Preference profile:** quiet 0.9, academic 1.0, cheap 0.8, social 0.4, restorative 0.6  
**Avoid:** crowded tourist spots, expensive restaurants, nightlife areas  
**Weekend pattern:** Saturday = exploratory (new neighborhood), Sunday = recovery (campus library or neighborhood café)

**Persona tension:** Wants to explore Downtown and Brooklyn, but transit cost and student budget make longer trips costly. Plans often get truncated by budget or exhaustion.

**Injection formats:**
- Short: "I am Maya, a first-year CS master's student at Columbia who lives in Morningside Heights."
- Medium: "I am Maya, a first-year CS master's student at Columbia University. I live near campus in Morningside Heights and study at Butler Library most days. I prefer quiet cafés and low-cost plans. I avoid crowded tourist areas."
- Full: See structured memory block JSON format below.

---

### Persona 2: Dario — SoHo Creative Worker

**Background:** Mid-level designer at a creative agency. 4 years in NYC. Lives in Williamsburg.

**Anchor locations:**
- Home: Williamsburg, north side (Bedford Ave)
- Work: SoHo agency (Prince/Broadway area)
- Social: Williamsburg bar/restaurant scene, occasional DUMBO
- Commute: L train → transfer → 6 to Spring St (long commute)

**Preference profile:** creative 1.0, social 0.8, outdoor 0.6, expensive 0.5  
**Avoid:** generic Midtown, chains, over-commercialized areas  
**Weekend pattern:** Saturday = active (art openings, events, outer Brooklyn), Sunday = café + walk + recovery

**Persona tension:** Long L→6 commute. Weekend plans pivot on whether to stay in Brooklyn or cross into Manhattan.

---

### Persona 3: Elena — Brooklyn Freelancer

**Background:** Freelance writer/editor working remotely. Carroll Gardens for 7 years.

**Anchor locations:**
- Home: Carroll Gardens (Court Street area)
- Work: home + neighborhood cafés
- Social: Carroll Gardens / Cobble Hill, occasional Red Hook

**Preference profile:** quiet 0.9, local 1.0, cheap 0.7, restorative 0.8, walkable 0.9  
**Avoid:** Midtown entirely, tourist areas, crowded weekend spots  
**Weekend pattern:** hyperlocal — farmer's market, Red Hook waterfront, Park Slope, occasional Manhattan for specific event

**Persona tension:** Deep local knowledge of South Brooklyn, limited knowledge of outer Queens, Bronx, or upper Manhattan. Plans fail when outside her comfort geography.

---

### Persona 4: Kwame — Astoria Teacher

**Background:** High school teacher in Jackson Heights. Lifelong Astoria resident.

**Anchor locations:**
- Home: Astoria (30th Ave / N line)
- Work: Jackson Heights (Junction Blvd)
- Social: Astoria neighborhood (Ditmars, Steinway)
- Weekend: occasionally Manhattan for culture

**Preference profile:** local 1.0, quiet 0.6, cheap 0.7, social 0.5, walkable 0.7  
**Avoid:** Manhattan crowds, expensive neighborhoods, multiple-transfer plans  
**Weekend pattern:** Saturday = family/community in Astoria or Flushing, Sunday = local recovery or single cultural trip

**Persona tension:** Strong Queens-centric worldview. Model defaults to Manhattan-centric suggestions, systematically underserving Kwame.

---

### Persona 5: Priya — Hell's Kitchen Analyst

**Background:** Financial analyst at Midtown East firm. 2 years in NYC. Lives in Hell's Kitchen.

**Anchor locations:**
- Home: Hell's Kitchen (9th Ave / 46th area)
- Work: Midtown East (Park Ave / 50th area)
- Social: West Midtown restaurant/bar scene, sometimes UWS or LES

**Preference profile:** social 0.7, expensive 0.5, late_night 0.6, transit_node 0.8  
**Avoid:** too much walking, far outer-borough trips  
**Weekend pattern:** Saturday = active social with specific reservation, Sunday = full recovery (delivery, home)

**Persona tension:** High income and Midtown home. The model's tendency to suggest Times Square and obvious Midtown attractions is the main failure mode.

---

### Full Memory Block Format (for NYC-Memory Condition B)

```json
{
  "memory_context": {
    "name": "{name}",
    "role": "{role_description}",
    "home": "{home_place}",
    "work_or_study": "{work_place}",
    "preferences": ["{pref_1}", "{pref_2}", "{pref_3}"],
    "avoid": ["{avoid_1}", "{avoid_2}"],
    "energy_today": "{energy_level}",
    "budget_today": "${budget_amount}",
    "recent_pattern": "{recent_behavior_note}"
  }
}
```

### Role Label Derivation (Persona × Place)

For each (persona, place) pair, derive the role label deterministically:

| Role | Condition |
|---|---|
| home | place is within 500m of persona's home anchor |
| work | place is within the persona's work neighborhood |
| study | place has `aff_academic = 1` AND is within reasonable transit range of persona's campus anchor |
| weekend | place matches ≥2 of persona's top 3 preference tags AND is not home/work AND is within the persona's typical weekend travel range |
| none | does not fit any role for this persona |

These derived labels populate Probe 7 (Urban Role in Persona Context).

---

## Part 5: Build Order

Five sprints, sequenced by dependency. The rule is: build what unblocks the most downstream work first.

### Sprint A (Days 1–5): Minimal Viable Data Layer

**Goal:** Working activation extraction pipeline with enough data to sanity-check everything.

1. Construct place corpus: 60 Tranche A places minimum (20 Manhattan across all latitude bands, 15 Brooklyn, 10 Queens, 10 Bronx + other, 5 parks, 5 subway stations, 5 campuses). Record all core geographic fields + affordance tags. Use OSM Overpass API for coordinates + manual annotation for affordance tags.
2. Construct Probe 1 (Coordinate) and Probe 2 (Borough) using Template A only. This is 60 factual prompts, one per place, with coordinate labels and borough labels. The minimal viable probe dataset.
3. Extract activations at strategic layers [0, 6, 12, 18, 25] for these 60 prompts.
4. Sanity check notebook: verify shapes, verify that different boroughs separate in PCA at layer 12.

**Done when:** PCA of layer-12 activations shows at least partial borough clustering. This is visual evidence the pipeline works and there is signal to probe.

---

### Sprint B (Days 6–10): Behavioral Benchmark MVP

**Goal:** First model outputs on the benchmark — behavioral error taxonomy begins here.

5. NYC-Map benchmark (36 prompts). Generated entirely from coordinate math — no manual annotation beyond what Sprint A produced.
6. NYC-Affordance benchmark (28 prompts). Requires Sprint A affordance tags.
7. Egocentric anchor table (30 anchors). Programmatic from coordinate data + Manhattan grid transform.
8. NYC-Ego benchmark (27 prompts, Level 1 and 2 only). Requires anchor table.

**Done when:** First table of model accuracy scores exists for NYC-Map and NYC-Ego, with qualitative failure examples noted.

---

### Sprint C (Days 11–16): Full Probe and Persona Layer

**Goal:** Complete the probing foundation and the novel contributions.

9. Extend place corpus to 100 Tranche A places. Fill all remaining affordance fields, temporal fields, role fit scores.
10. Probe datasets 3 (Direction), 4 (Affordance Regression), 5 (Affordance Classification), 6 (Temporal Role). Template A only.
11. Define 5 canonical personas in full format. Derive role labels for all 100 × 5 = 500 (persona, place) pairs.
12. Probe dataset 7 (Urban Role in Persona Context). Requires personas + 100 places.
13. Extract activations for all probes at strategic layers.

**Done when:** All 7 probes are extractable and trained. Layerwise accuracy plot exists for each probe.

---

### Sprint D (Days 17–21): Counterfactual and Memory Layer

**Goal:** Build the most complex and most novel behavioral and probe components.

14. NYC-Life benchmark (12 prompts, 3 per persona).
15. NYC-Counterfactual benchmark (20 prompts derived from NYC-Life).
16. NYC-Memory benchmark (30 paired prompts, no-memory vs memory-injected).
17. Probe dataset 8 (Memory-Conditioned Delta). Paired activation extraction.
18. Template B and C variants for Probes 1–6. Template sensitivity ablation.

**Done when:** MCSS plot exists across layers for Probe 8. Counterfactual Fidelity Scores computed.

---

### Sprint E (Days 22–25): Route Benchmark and Final Corpus

**Goal:** Route benchmark (requires engineering work), Tranche B places, and final audit.

19. NYC-Route benchmark (21 prompts + automated route evaluator).
20. NYC-Life and NYC-Memory remaining personas.
21. Tranche B places (100 additional). Partial labels only — coordinate + borough + affordance tags.
22. Final audit: verify all label schema fields complete, cross-check ground truths, remove ambiguous examples, confirm Tranche A / Tranche B non-overlap.

**Done when:** All 7 NYCWorldBench sub-benchmarks are runnable. Full corpus is 200 places. Data audit is signed off.

---

### Build Order Summary

| Sprint | Days | Primary Deliverable | Unblocks |
|---|---|---|---|
| A | 1–5 | 60 places + Probes 1–2 + pipeline + PCA sanity check | Everything |
| B | 6–10 | NYC-Map + NYC-Affordance + NYC-Ego (L1/L2) | Layer 2 behavioral |
| C | 11–16 | 100 places + Probes 3–7 + personas + role labels | Layer 3 probing |
| D | 17–21 | NYC-Life + Counterfactual + Memory + Probe 8 + template variants | Layers 4–5 |
| E | 22–25 | NYC-Route + Tranche B + final audit | Full publication |

---

## Key Design Principles (Reference)

**Why haversine, not routing distance, for ground truth:** Routing distance requires API calls and changes over time. Haversine is deterministic from coordinates, permanently reproducible, and sufficient for the spatial reasoning and egocentric computations.

**Why fixed templates across probes:** Cross-place comparisons in PCA and RSA are only valid when the template is held constant. If place A gets a factual template and place B gets a planning template, the activation difference reflects template choice, not place identity.

**Why both binary tags and continuous scores:** Binary tags are easier to annotate reliably and better for classification probes. Continuous scores allow regression probing. They are derived separately (binary from rubric annotation, continuous from aggregated signals) and stored as separate fields.

**Why 100 Tranche A places, not fewer:** Below 100 places, probe training sets after the 80/20 split become too small (< 64 training examples). For 4-class classification, some classes may have only 12–15 training examples. This is workable but tight. Above 200 Tranche A places, annotation cost grows faster than probe benefit.

**The frontier model as second annotator:** Not a shortcut — a structured inter-rater reliability check. The human annotation is ground truth. The frontier model annotation measures agreement. Disagreements flag ambiguous cases. This also provides a baseline for how well a frontier model itself performs on the affordance annotation task, useful for interpreting Gemma 2B's probe accuracy in context.
