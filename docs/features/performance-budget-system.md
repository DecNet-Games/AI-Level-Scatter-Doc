---
title: Performance Budget System
layout: default
parent: Features
nav_order: 11
---

# Performance Budget System

**Define and enforce placement budget rules scaled for Mobile, VR, Console, and PC targets**

Adding detail to game scenes is addictive. Developers often keep adding grass, rocks, and props until the editor feels rich, only to discover later that the game runs at 15 FPS on target mobile hardware or causes thermal throttling on standalone VR headsets. Undoing and re-configuring layouts to fit target frame rates is an expensive development setback.

The **Performance Budget System** prevents performance regressions before they happen. It allows you to select your target platform and automatically restricts scatter limits, warns you of budget overflows, and scales density ranges dynamically.

---

## How the Budgeting Engine Works

The budget engine calculates maximum instance thresholds based on target device capabilities:

| Target Platform | Max Instances (per $100\text{m} \times 100\text{m}$) | Memory Budget (VRAM) | Vertex Count Limit | Recommend Optimization |
| :--- | :--- | :--- | :--- | :--- |
| **Mobile / WebGL** | 1,500 | 64 MB | 250,000 | Static Mesh Combining + LOD3 |
| **Standalone VR** | 2,500 | 128 MB | 500,000 | Strict Spacing + LOD2 |
| **Console (Mid-Tier)**| 10,000 | 512 MB | 2,000,000 | GPU Instancing |
| **High-End PC** | Unlimited (50,000+) | 2 GB+ | 10,000,000+ | Native Streaming Chunks |

### Live Budget Calculations

The system calculates total vertex and draw call metrics before placing objects:
$$\text{Projected Vertices} = \sum (\text{Prefab Mesh Vertices} \times \text{Candidate Count})$$
$$\text{Projected Draw Calls} = \sum (\text{Prefab Unique Materials} \times \text{Candidate Count}) \quad [\text{Without Instancing}]$$

If these projected values exceed the selected platform's threshold, the system displays budget warnings in the inspector and flags the placement profile as `Over Budget`.

---

## Editor Window Interface

Access the budget settings at **Tools → Decnet → Performance Budget System**. The window features a premium dark header with a distinct crimson accent.

* **Target Profile**: Select the scatter profile to audit.
* **Target Hardware**: Dropdown menu containing Mobile, Standalone VR, Console (Base), Console (Next-Gen), and High-End PC.
* **Asset Allocation Bar**: A clean, color-coded horizontal progress bar displaying:
  - **Green (Safe)**: Under $70\%$ allocation.
  - **Orange (Warning)**: $70\% - 99\%$ allocation.
  - **Red (Overbudget)**: $100\%+$ allocation.
* **Auto-Scale Density**: Clicking this button automatically scales down the selected profile's density parameters to bring the projected counts back within target hardware limits.
* **Validation Console**: Lists warning logs detailing which specific prefabs contribute the most to the vertex/draw call overflow.

```
[Target Platform: Standalone VR]
Instance Budget: 2,150 / 2,500 [██████████████░░░] 86% - Safe
Vertex Budget:   580k  / 500k  [█████████████████] 116% - OVER BUDGET!
```

> [!IMPORTANT]
> The vertex calculation scanner depends on the presence of a `MeshFilter` or `SkinnedMeshRenderer` on the target prefab. If your prefabs use custom nested geometries, ensure the parent object contains the main mesh components, or check **Scan Deep Hierarchies** in the budget window settings.

> [!TIP]
> If your profile is flagged as overbudget, you don't necessarily have to decrease density. You can check the **Bake Static Mesh Combiner** rule, which will merge the spawned prefabs and reduce draw calls by up to $95\%$, resolving the performance bottleneck.
