# Note 1: Setup Layer

**Sprint:** 000 — Small Model, Big City  
**Date:** May 8, 2026  
**Status:** Infrastructure note — Layer 1 of 5

---

## What This Note Covers

Before any probing, geometry analysis, or interpretability work can begin, three things must be in place:

1. A loaded model you can run inference on **and** extract activations from
2. A dataset of NYC places and prompts with clean ground-truth labels
3. An activation extraction pipeline that saves the right tensors from the right layers

This note builds that foundation. Everything in Layers 2–5 depends on it.

---

## The Five-Layer Stack (Reminder)

```
Layer 1: Setup         ← you are here
Layer 2: Behavioral    → does the model do the right thing?
Layer 3: Probing       → is the right information encoded in activations?
Layer 4: Representation Dynamics → how do representations evolve?
Layer 5: Geometry      → what is the shape of NYC inside the model?
```

Layers 2–5 all call into the infrastructure built here.

---

## Part 1: Model

### Which model

The primary model for this sprint is **Gemma 2B** (the 2-billion parameter instruction-tuned variant).

Reasons:
- Small enough to run locally on a single GPU or even CPU with quantization
- Part of an open family with multiple sizes (2B, 7B, 9B) so you can later compare
- Has Gemma Scope sparse autoencoders released for it, which Layer 5 will use
- Instruction-tuned variant (gemma-2b-it) gives better behavioral outputs; base variant gives cleaner activations for probing — you will use both

Model IDs on HuggingFace:

```
google/gemma-2-2b       ← base model, better for probing
google/gemma-2-2b-it    ← instruction-tuned, better for behavioral evaluation
```

Later comparison models if needed:
- `google/gemma-2-9b` and `google/gemma-2-9b-it` — larger Gemma
- `Qwen/Qwen2.5-1.5B` or `Qwen/Qwen2.5-3B` — compact alternatives
- A large model (GPT-4o or Claude) as an upper-bound behavioral reference only

### Loading the model in PyTorch + HuggingFace

The key point for research use is: **you need `output_hidden_states=True`** to get activations from all layers, not just the final logits.

```python
from transformers import AutoTokenizer, AutoModelForCausalLM
import torch

model_name = "google/gemma-2-2b"

tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype=torch.bfloat16,    # memory-efficient
    device_map="auto",             # places layers on available device
)
model.eval()
```

Running a forward pass and getting hidden states:

```python
inputs = tokenizer("West Village is a neighborhood in", return_tensors="pt").to(model.device)

with torch.no_grad():
    outputs = model(**inputs, output_hidden_states=True)

# outputs.hidden_states is a tuple of tensors
# one tensor per layer (including the embedding layer)
# shape of each: [batch_size, sequence_length, hidden_dim]
print(len(outputs.hidden_states))   # should be num_layers + 1 (embedding + each transformer block)
print(outputs.hidden_states[0].shape)  # e.g. [1, 8, 2304] for Gemma 2B
```

### What is a "hidden state" / "activation"?

Every transformer layer transforms a sequence of vectors. After each layer, the model has an updated representation for every token in the input. These are called **hidden states** or **activations**.

For Gemma 2B:
- Hidden dimension: **2304**
- Number of transformer layers: **26**
- So after a forward pass, you have 27 tensors (1 embedding + 26 layers), each of shape `[batch, seq_len, 2304]`

For probing and geometry, you will usually want the activation at **the last token** of the prompt for single-token probing, or the activations at **place-name tokens** for place-specific probing.

```python
# Get the last-token activation from layer 12
layer_idx = 12
last_token_activation = outputs.hidden_states[layer_idx][0, -1, :]  # shape: [2304]
```

### An intuition for layers

Think of the layers as a pipeline of processing:

```
Layer 0  (embedding)   → raw token identity, no context yet
Layers 1–8  (early)    → syntax, local context, basic word meaning
Layers 9–18 (middle)   → richer semantics, relational structure, spatial facts
Layers 19–26 (late)    → task-specific processing, output preparation
```

This is a hypothesis, not a law — the whole point of probing is to find out what is actually where. But early layers tend to encode surface features and late layers tend to encode task-relevant decisions.

---

## Part 2: NYC Data

### Why you need structured data before experiments

You cannot run a probe without knowing the answer. You cannot evaluate geometry without knowing which borough a neighborhood belongs to. The data layer is where you define ground truth.

### 2.1 Place list

Start with roughly **80–120 NYC places** across boroughs, types, and lifestyle profiles.

For each place, record:

```python
{
    "name": "West Village",
    "lat": 40.7338,
    "lon": -74.0059,
    "borough": "Manhattan",
    "neighborhood_cluster": "Lower Manhattan / Downtown",
    "place_type": "neighborhood",
    "lifestyle_tags": ["quiet", "walkable", "cafes", "local", "residential"],
    "crowd_level": "low-medium",
    "price_level": "medium-high",
    "indoor_outdoor": "outdoor-primary",
    "transit_accessibility": "high",
    "columbia_distance_km": 11.2,
    "columbia_transit_min": 35,
    "notes": "Reference home base for the project persona"
}
```

Coordinate sources: Google Maps, NYC OpenData, OSM (OpenStreetMap).

A suggested minimum place list covering the key clusters:

**Lower Manhattan / Downtown**
- West Village, SoHo, Lower East Side, Financial District, Tribeca, Chinatown

**Midtown**
- Chelsea, Hell's Kitchen, Times Square, Bryant Park, Murray Hill, Gramercy

**Upper Manhattan**
- Upper West Side, Columbia University, Morningside Heights, Harlem, Washington Heights

**Parks and landmarks**
- Central Park, Riverside Park, The High Line, Hudson River Park, Prospect Park

**Brooklyn**
- Williamsburg, DUMBO, Brooklyn Heights, Cobble Hill, Park Slope, Bushwick, Coney Island

**Queens**
- Astoria, Long Island City, Flushing, Jackson Heights

**Bronx + Staten Island**
- One or two anchors each, for completeness

**Non-place anchors (for egocentric tests)**
- Named subway stations, intersections, waterfront points

### 2.2 Probe datasets

A probe dataset is: a set of (prompt → activation) pairs with a numeric or categorical label.

You need separate probe datasets for each target you want to decode from activations.

**Coordinate probe dataset**

Goal: decode latitude and longitude from activations.

```
prompt: "The neighborhood known as West Village is located in Manhattan."
label: [lat=40.7338, lon=-74.0059]

prompt: "The neighborhood known as Astoria is located in Queens."
label: [lat=40.7721, lon=-73.9302]
```

One prompt per place. Collect the activation at the place-name token (or the last token). Label is a 2D float [lat, lon]. Train a linear regression probe.

**Borough probe dataset**

Goal: decode borough from activations (4-class classification).

```
prompt: "The neighborhood known as West Village is located in ..."
label: "Manhattan"   → 0

prompt: "The neighborhood known as Astoria is located in ..."
label: "Queens"      → 1
```

**Direction probe dataset**

Goal: decode relative direction (north/south of a reference point) from activations.

```
prompt: "The neighborhood known as Columbia University is _____ of West Village."
label: "north"   → 1

prompt: "The neighborhood known as Financial District is _____ of West Village."
label: "south"   → 0
```

**Affordance probe dataset**

Goal: decode affordance category from place activations.

```
prompt: "A person who wants to find a quiet café would go to West Village."
label: "quiet"

prompt: "A person who wants to experience nightlife would go to Williamsburg."
label: "social/nightlife"
```

**Role probe dataset**

Goal: decode whether a place is mentioned as home / work / study / weekend in context.

```
prompt: "I live in West Village and work in SoHo."
label at 'West Village' token: "home"
label at 'SoHo' token: "work"
```

### 2.3 Prompt templates

For consistency, use fixed templates across experiments. This controls what varies (the place) and keeps everything else constant.

```
FACTUAL:      "The neighborhood known as {place} is located in New York City."
RELATION:     "The neighborhood known as {place_a} is {direction} of {place_b}."
EGOCENTRIC:   "You are standing in {place} facing {direction}."
ROUTINE:      "I live in {home} and work in {work}."
AFFORDANCE:   "Someone who wants {affordance} should visit {place}."
PLANNING:     "Plan a Saturday starting from {home} with {budget} budget and {energy} energy."
```

Always tokenize the same template, vary only the place name. This makes cross-place comparison meaningful.

### 2.4 Data files to create

```
data/
    places.json            ← full place list with coordinates, labels, tags
    probes/
        coordinate.jsonl   ← (prompt, lat, lon) triples
        borough.jsonl      ← (prompt, borough_label) pairs
        direction.jsonl    ← (prompt, direction_label) pairs
        affordance.jsonl   ← (prompt, affordance_label) pairs
        role.jsonl         ← (prompt, role_label) pairs
    benchmarks/
        nyc_map.jsonl      ← behavioral prompts + expected answers
        nyc_ego.jsonl
        nyc_route.jsonl
        nyc_affordance.jsonl
        nyc_life.jsonl
        nyc_counterfactual.jsonl
```

---

## Part 3: Activation Extraction

### The extraction pipeline

For each probe dataset, you need to:
1. Tokenize each prompt
2. Run a forward pass with `output_hidden_states=True`
3. Select which token position to extract from
4. Select which layers to extract from
5. Store the result

```python
import torch
import json
from pathlib import Path

def extract_activations(model, tokenizer, prompts, layers_to_extract, token_strategy="last"):
    """
    prompts: list of strings
    layers_to_extract: list of ints, e.g. [0, 6, 12, 18, 25]
    token_strategy: "last" | "first" | specific int index
    
    returns: dict of {layer_idx: tensor of shape [num_prompts, hidden_dim]}
    """
    results = {l: [] for l in layers_to_extract}
    
    for prompt in prompts:
        inputs = tokenizer(prompt, return_tensors="pt").to(model.device)
        
        with torch.no_grad():
            outputs = model(**inputs, output_hidden_states=True)
        
        seq_len = inputs["input_ids"].shape[1]
        
        if token_strategy == "last":
            token_idx = -1
        elif token_strategy == "first":
            token_idx = 0
        else:
            token_idx = token_strategy
        
        for layer_idx in layers_to_extract:
            # shape: [hidden_dim]
            act = outputs.hidden_states[layer_idx][0, token_idx, :].cpu().float()
            results[layer_idx].append(act)
    
    # stack into tensors
    return {l: torch.stack(results[l]) for l in layers_to_extract}
```

### Which token to extract from

This is a research decision that matters.

**Last token** (`token_strategy="last"`): In causal (decoder-only) transformers like Gemma, the last token has attended to all previous tokens. It carries a compressed "summary" of the context. Use this for prompts where you want the model's overall representation of the situation.

**Place-name token**: If your prompt is "West Village is north of SoHo" and you want the representation of "West Village" specifically, extract at the token(s) corresponding to that span. This is more targeted and appropriate for place-specific probes.

```python
# Finding the token index of a place name in the prompt
prompt = "The neighborhood known as West Village is in Manhattan."
tokens = tokenizer(prompt, return_tensors="pt")["input_ids"][0]
token_strings = [tokenizer.decode([t]) for t in tokens]
print(list(enumerate(token_strings)))
# Find the index where "West" or "Village" appears
```

**Average over place-name tokens**: If the place name spans multiple tokens (e.g. "West" and "Village" are two tokens), you can average their activations. More stable than picking just one.

### Which layers to extract from

You do not need all 26 layers for every experiment. Start with a strategic subset:

```python
LAYERS_STRATEGIC = [0, 3, 6, 9, 12, 15, 18, 21, 25]
# → every 3rd layer, covers the full depth at reasonable cost

LAYERS_DENSE = list(range(26))
# → all layers, use only when comparing layerwise decodability precisely
```

For initial probing: use `LAYERS_STRATEGIC`. For layerwise plots: use `LAYERS_DENSE`.

### Storage format

Save activations as `.pt` (PyTorch tensors) alongside a metadata `.json`.

```
activations/
    coordinate_probe/
        layer_00.pt      ← tensor [N, 2304]
        layer_06.pt
        layer_12.pt
        layer_18.pt
        layer_25.pt
        metadata.json    ← {prompts, labels, layer_indices, model_name, token_strategy}
    borough_probe/
        layer_00.pt
        ...
```

Metadata format:

```json
{
    "model": "google/gemma-2-2b",
    "date": "2026-05-08",
    "token_strategy": "last",
    "layers": [0, 6, 12, 18, 25],
    "n_samples": 85,
    "prompts": ["The neighborhood known as West Village ...", ...],
    "labels": [{"lat": 40.7338, "lon": -74.0059}, ...]
}
```

Loading back:

```python
acts = torch.load("activations/coordinate_probe/layer_12.pt")
meta = json.load(open("activations/coordinate_probe/metadata.json"))
labels = meta["labels"]
```

---

## Part 4: Sanity Checks

Before moving to probing, run these checks to confirm the pipeline is working.

### Check 1: Activation shapes are correct

```python
acts = extract_activations(model, tokenizer, ["West Village is in Manhattan."], layers_to_extract=[0, 12, 25])
for layer, tensor in acts.items():
    print(f"Layer {layer}: {tensor.shape}")  # expect [1, 2304]
```

### Check 2: Different places give different activations

```python
acts_wv = extract_activations(model, tokenizer, ["West Village is a neighborhood."], layers_to_extract=[12])
acts_ast = extract_activations(model, tokenizer, ["Astoria is a neighborhood."], layers_to_extract=[12])

cos_sim = torch.nn.functional.cosine_similarity(acts_wv[12], acts_ast[12])
print(f"Cosine similarity: {cos_sim.item():.4f}")  # should be < 1.0 (ideally somewhat < 1.0)
```

### Check 3: Same prompt gives the same activation (determinism)

```python
a1 = extract_activations(model, tokenizer, ["West Village is in Manhattan."], layers_to_extract=[12])
a2 = extract_activations(model, tokenizer, ["West Village is in Manhattan."], layers_to_extract=[12])
print(torch.allclose(a1[12], a2[12]))  # should be True
```

### Check 4: Visualize a few activations

Run PCA on 10–20 place activations from layer 12 and plot. You should see that Manhattan vs Queens vs Brooklyn places are vaguely separated even before any probing. If they are all on top of each other, something may be wrong with your prompts or token extraction.

```python
from sklearn.decomposition import PCA
import matplotlib.pyplot as plt

# Collect layer-12 activations for 20 places
# ... run extract_activations for each place prompt ...

pca = PCA(n_components=2)
coords_2d = pca.fit_transform(activation_matrix)  # [N, 2]

for i, place in enumerate(places):
    color = borough_color_map[place["borough"]]
    plt.scatter(coords_2d[i, 0], coords_2d[i, 1], color=color, s=60)
    plt.annotate(place["name"], coords_2d[i], fontsize=7)

plt.title("PCA of layer-12 activations (colored by borough)")
plt.show()
```

If Manhattan places cluster on one side and Brooklyn on the other, you have early evidence that the model's geometry is spatially meaningful. If everything is random, that is also a finding.

---

## Part 5: What Layer 1 Produces

By the end of this setup layer, you should have:

| Artifact | Description |
|---|---|
| `data/places.json` | 80–120 NYC places with coordinates, borough, tags |
| `data/probes/*.jsonl` | Probe datasets for coordinate, borough, direction, affordance, role |
| `activations/*/layer_XX.pt` | Saved activations for each probe dataset across strategic layers |
| `activations/*/metadata.json` | Metadata linking each activation to its prompt and label |
| `notebooks/00-sanity-checks.ipynb` | Sanity check notebook confirming shapes, differences, PCA visualization |

These become the inputs to:
- **Layer 2 (Behavioral):** run benchmark prompts through the IT model, collect outputs
- **Layer 3 (Probing):** train linear and MLP probes on these activations with these labels
- **Layer 4 (Dynamics):** compare activations across layers for the same prompt families
- **Layer 5 (Geometry):** run PCA/UMAP/RSA on place activations, look for manifolds

---

## Part 6: Key Conceptual Questions to Hold in Mind

As you build this infrastructure, keep these questions open. They will become the probing hypotheses:

1. **Does the template matter?** Does "West Village is a neighborhood" give the same activation as "The model thinks of West Village as home"? If they differ significantly, the representation is context-sensitive, not just a lookup.

2. **Does the layer matter?** Is borough better decoded from layer 6 or layer 18? That tells you something about where geographic knowledge lives in the model.

3. **Does the instruction-tuned model encode differently than the base model?** If IT activations cluster less by borough and more by affordance/vibe, the fine-tuning may have shifted what the model primarily represents.

4. **Is the representation continuous or categorical?** If you project places by their activations and plot them on a map overlay, does the activation distance roughly match the geographic distance? That would be striking.

5. **Does prompt length or context change the activation?** A bare "West Village" token vs a full planning prompt — does the place representation shift when the user's persona (home, work, preferences) is in context?

---

## Next Steps

Layer 1 is complete when:

- [ ] Model loads and produces correct hidden state shapes
- [ ] Place list is written to `data/places.json` (minimum 60 places, 4 boroughs)
- [ ] At least 3 probe datasets exist in `data/probes/`
- [ ] Activations are extracted and saved for strategic layers
- [ ] Sanity checks pass (shapes, differences, PCA visible)

The next note (`note-2-behavioral.md`) will cover Layer 2: running the behavioral benchmark and collecting the first model outputs.
