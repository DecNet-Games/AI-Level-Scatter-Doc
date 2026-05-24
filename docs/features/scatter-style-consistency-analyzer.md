---
title: Style Consistency Analyzer
layout: default
parent: Features
nav_order: 12
---

# Style Consistency Analyzer

**Scan active scattered environments to audit material, shader, and asset consistency**

When working with assets from different packages (e.g. mix-and-matching Synty models with customized low-poly meshes), it is easy to introduce visual inconsistencies. A stone prefab using a high-fidelity PBR shader placed next to a stylized flat-shaded ruins archway immediately breaks game immersion. Similarly, mismatched texture sizes or anomalous object scales can ruin environment art quality.

The **Style Consistency Analyzer** audits your scenes, scanning every scattered object to find shader mismatches, texture resolution jumps, and scale anomalies.

---

## Analysis Rules & Auditing Metrics

The scanner runs a multi-pass check on all active GameObjects parented under a scatter profile container.

### 1. Shader Compatibility Audit
The engine checks the shader assigned to each active material. It categories materials based on shader model types (e.g. Standard PBR, Stylized/Toon, Flat Unlit, Custom URP/HDRP) and flags warning flags if a single scatter layout mixes incompatible shading models (e.g. placing a Toon-shaded plant directly under a PBR-shaded tree).

### 2. Texel Density & Resolution Consistency
The analyzer inspects the texture dimensions referenced in material properties (e.g. `_MainTex`, `_BaseMap`). It highlights:
- **Texel Discrepancy**: Textures that are too large (e.g. a 4K rock texture next to a 256px leaf texture).
- **GPU Bloat**: Small props using uncompressed or oversized textures that waste memory.

### 3. Scale Drift Verification
Objects placed in the scene are audited against their original prefab settings. If an instance has been scaled to an extreme degree (e.g. a shrub scaled up by $10\times$), it flags a scale drift warning since the polygon density and texture mapping will look stretched and pixelated.

---

## Editor Window Interface

Open the analyzer at **Tools → Decnet → Style Consistency Analyzer**. The interface features a premium dark header with a distinct violet/cyan accent.

* **Audit Selection**: Select whether to scan the **Active Selection**, the **Whole Scene**, or a specific **Scatter Profile Parent**.
* **Analyzer Settings**:
  - **Check Shaders**: Toggles shader compatibility audits.
  - **Check Texture Sizes**: Compares texel layouts.
  - **Check Scale Thresholds**: Flags instances scaled outside of the profile's min/max bounds.
* **Scan environment**: Executes the audit pass and lists items in the **Audit Results Tree View**:
  - **Red Alert (Error)**: Broken material references, missing shaders, or unassigned prefabs.
  - **Yellow Alert (Warning)**: Incompatible shaders (PBR mixed with Toon) or texel density mismatches.
  - **Blue Alert (Info)**: Non-batching materials that could be combined.
* **Auto-Fix Button**: For scale drift warnings, clicking Auto-Fix clamps the scale back within the prefab's defined profile boundaries. For non-batching materials, it recommends combining textures into a shared atlas.

```
[Audit Results: 3 Warnings Found]
⚠ WARNING: Material 'M_PineTree' uses Standard PBR shader, while 'M_CartoonGrass' uses Stylized Toon shader. (Visual Inconsistency)
⚠ WARNING: Texture 'T_Rock_4K' (4096px) exceeds average texel density by 4x. Recommend downscaling to 1024px.
⚠ WARNING: GameObject 'PineTree_Variant_A (2)' has scale (4.5, 4.5, 4.5) which exceeds profile limits.
```

> [!NOTE]
> The style analyzer does not modify your textures or materials directly. It serves as an advisory audit tool, giving you the warnings and details needed to keep your environmental style consistent.

> [!TIP]
> Run a **Style Consistency Audit** right before baking your environment with the **Static Mesh Combiner** to ensure all combined sub-meshes share compatible materials and shaders.
