---
title: Workflows
layout: default
nav_order: 7
has_children: true
---

# Advanced Project Workflows

**Tailor your environment scatter settings for specific target platforms and large world architectures**

A scattering configuration that works perfectly for a small PC tech demo will fail when applied to a mobile game targeting old Android devices or a massive multi-kilometer open-world RPG. Different game types require different optimization strategies, physics checks, and hierarchy organization.

This section covers production-proven workflows for configuring and optimizing AI Level Scatter.

---

## Workflow Categories

Select one of the workflows below to optimize your environment pipelines:

1. **[Mobile & WebGL Optimization Guide](mobile-projects.md)**: Strategies for reducing draw calls, managing vertex limits, utilizing Level of Detail (LOD) groups, and enforcing strict spacing rules.
2. **[Large-Scale & Open-World Environments](large-projects.md)**: Handling grid partitioning, streaming chunks, boundary padding, and high-performance terrain raycast bakes.

---

## The Environment Optimization Pipeline

To guarantee visual fidelity and target frame rates, follow this recommended generation cycle:

```
[Define Scatter Profile] ──> [Apply Target Platform Budget]
                                      │
[Bake Mesh Combiner]   <─── [Execute Context Rules Scan]
        │
[Verify Scene Heatmap] ──> [Deploy / Build Project]
```

> [!NOTE]
> All workflows are fully supported across all major Unity Render Pipelines (Built-in, URP, and HDRP). The layout generators and rules engines are render-pipeline agnostic.
