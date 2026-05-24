---
title: Scatter Variation Generator
layout: default
parent: Features
nav_order: 9
---

# Scatter Variation Generator

**Generate distinct environment variations using seed mutation and delta perturbations**

Designing vast game worlds requires visual diversity. Reusing the exact same scatter configurations across multiple levels or forest zones leads to repetitive, recognizable patterns. Level designers often waste time duplicate-saving scatter profiles and manually altering offsets to introduce subtle variations.

The **Scatter Variation Generator** automates this by creating profile iterations. By applying mathematical perturbations to seed values, density thresholds, scale ranges, and rotation offsets, the generator compiles multiple unique layout variations from a single master profile.

---

## Variation Algorithm

The generator works on the active `ScatterProfile` and applies a series of user-configurable seed mutations and floating-point delta offsets:

### 1. Seed Mutation
Every scatter session is governed by a pseudo-random number generator (PRNG) initialized with a specific seed value. The Variation Generator mutates this seed using a high-entropy hash function:
$$\text{New Seed} = (\text{Base Seed} \times 1103515245 + 12345) \pmod{2^{31}}$$
This ensures that every generated variation features a completely unique layout distribution, even if all other parameters remain constant.

### 2. Delta Perturbations
To prevent layouts from looking like simple random shuffles, the engine applies delta offsets to existing parameters:
* **Scale Delta**: Alters the min and max bounds by a user-defined percentage (e.g. $\pm 5\%$), simulating different growth stages or flora ages across environments.
* **Rotation Delta**: Shifts random yaw angles or introduces subtle tilt variations to simulate uneven ground forces.
* **Density Delta**: Slightly increases or decreases target counts (e.g. $\pm 10\%$) to mimic dense forest thickets and sparse clearings.

```
[Master Profile: Seed 1024] ──> [Mutator Engine] ──┬─> [Variation A: Seed 3409, Scale +5%]
                                                 ├─> [Variation B: Seed 8912, Scale -3%]
                                                 └─> [Variation C: Seed 1105, Density +10%]
```

---

## How to Use the Variation Generator

Open the variation manager via **Tools → Decnet → Scatter Variation Generator**. The window features a premium dark header with a distinct amber accent.

1. **Source Profile**: Assign the master `ScatterProfile` you wish to vary.
2. **Variation Count**: Select how many distinct profile variants you want to compile (ranges from 1 to 10).
3. **Perturbation Limits**:
   - **Density Jitter**: Limit the percentage variance in candidate density ($0\% - 30\%$).
   - **Scale Drift**: Set the maximum scale deviation from the base prefab limits ($0.05 - 0.5$).
   - **Rotation Deviation**: Limit the maximum additional yaw or pitch offset applied to prefabs.
4. **Compile Variants**:
   - The engine automatically duplicates the master profile, appends suffix tags (e.g. `_Variant_A`, `_Variant_B`), applies the delta perturbations, and saves the new assets in the same folder as the parent.
   - Alternatively, use the **Apply Perturbation Live** button to temporarily preview the variation in your active scene view without creating permanent assets.

> [!NOTE]
> Since all generated variations are saved as native ScriptableObjects, they retain full compatibility with your standard scene placement pipelines, including undo steps and static mesh combining.

> [!TIP]
> When setting up biome grids or chunked loading systems, generate 3 to 4 variations of your main ground-clutter profile. Distribute these variants randomly across adjacent terrain chunks to break up structural tiling and render a seamless natural landscape.
