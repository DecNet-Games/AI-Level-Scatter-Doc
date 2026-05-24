---
title: AI Storytelling & Handcrafted Feel
layout: default
parent: Features
nav_order: 8
---

# AI Storytelling & Handcrafted Feel

**Infuse algorithmic placements with asymmetry, focal gaps, and clustering bias**

Procedural scattering algorithms often suffer from "perfect uniform distribution" syndrome. They distribute grass, rocks, and trees in perfectly even spacings that feel sterile and artificial. Real-world environments are asymmetrical, chaotic, and driven by narrative context—a campsite has gathered logs around a fire, rocks pile up in natural clusters, and pathways naturally form clear focal openings.

The **AI Storytelling & Handcrafted Feel** window allows you to apply narrative clutter presets and adjust placement statistics to replicate the organic brush strokes of human level designers.

---

## Spatial Composition Principles

This system modifies candidate positions, scales, and densities using three naturalistic layout algorithms:

### 1. Clustering Bias (Natural Grouping)
In nature, flora and debris rarely grow in isolation; they cluster together where seeds fall or wind deposits them. The clustering algorithm selects random "attractor seeds" inside your placement bounds. Instead of a flat random distribution, candidate weights are amplified near attractors and dampened in-between, producing clustered groups separated by sparse clearings.

### 2. Focal Gaps (Negative Space)
A key rule of professional composition is the preservation of negative space. High-density noise blocks sightlines and creates visual exhaustion. The **Focal Gap** generator defines specific coordinates (or automatically identifies paths/camera sights) where scattering density is aggressively restricted to $0 - 5\%$. This guarantees clear sightlines toward landmarks, shrines, or doorways.

### 3. Asymmetry & Perturbation
Grid patterns are immediately broken using scale and rotation jitter. However, standard random numbers produce uniform ranges. The handcrafted engine applies **Perlin-noise driven delta perturbations**, generating smooth waves of scaling (e.g. smaller plants on the periphery, larger plants in the center) and directional leans that mimic prevailing winds.

---

## Editor Window Interface

Access this system from the Unity menu: **Tools → Decnet → Storytelling & Feel**. The window features a premium dark header with a glowing gold accent.

* **Feel Presets**: Select from pre-configured storytelling layouts:
  - **Fallen Campsite**: Generates dense clutter clusters with clear circular openings.
  - **Overgrown ruins**: Spawns high-density vines and debris climbing vertical surfaces while maintaining open floors.
  - **Worn Pathway**: Clears central corridors while banking heavy boulders and shrubs on the edges.
  - **Prevailing Wind**: Orients and bends all grass and tree models in a unified, naturally perturbed wave direction.
* **Clustering Strength**: Slider to adjust how tightly grouped assets are. Higher values pull candidates closer to localized seed clusters.
* **Focal Gap Size**: Configures the diameter of negative space zones, keeping critical scene viewpoints clear.
* **Perturbation Intensity**: Controls the degree of non-uniform translation offsets applied to break up geometric grids.

```
[Uniform Procedural Scatter]          [AI Storytelling Scatter]
  *   *   *   *   *   *   *             * * *         *       * *
  *   *   *   *   *   *   *      vs     * * * *               * * *
  *   *   *   *   *   *   *                *        [Gap]       *
  *   *   *   *   *   *   *                                   *
```

> [!TIP]
> Combine **Clustering Bias** with **Mesh Surface Sampling** when decorating ruins. By placing clutter seeds near broken pillars or wall corners, you can make moss and stone debris look like they crumbled naturally over time.
