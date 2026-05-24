---
title: Mobile & WebGL Optimization
layout: default
parent: Workflows
nav_order: 1
---

# Mobile & WebGL Optimization Guide

**Optimize scatter densities, enforce strict spacing rules, and structure LOD groups for mobile environments**

Mobile devices and WebGL builds operate under tight hardware restrictions. High vertex counts, excessive draw calls, and uncompressed textures will cause memory crashes or overheat target devices. To achieve smooth $60\text{ FPS}$ on typical mobile hardware, environment assets must be carefully scattered and optimized.

---

## 1. Enforcing Spacing Rules & Clutter Clamping

Random clustering looks beautiful on high-end PCs, but on mobile, overlapping objects waste vertices and fill the screen with unnecessary overdraw.

* **Increase Spacing Limits**: Open your `ScatterProfile` and increase the **Minimum Spacing** slider. For instance, grass should have a minimum spacing of $0.8\text{m} - 1.5\text{m}$ rather than $0.2\text{m}$.
* **Clamp Clutter Counts**: Limit total instances inside your scatter area. In a mobile scene, keep the overall density under 1,500 objects per chunk.
* **Collision Avoidance**: Keep **Collision Avoidance** checked on your profile so that shrubs, trees, and rocks never intersect each other or clip through static obstacles.

---

## 2. Prefab and LOD Group Setup

The scattering engine fully supports Unity's **LOD Group** component:

1. Ensure your environmental prefabs have an `LODGroup` component attached to the root.
2. Configure three levels of detail:
   - **LOD 0**: High-detail mesh (only visible up close).
   - **LOD 1**: Mid-detail mesh with reduced vertex counts.
   - **LOD 2 (or LOD 3)**: Low-detail proxy mesh or a cross-billboard card (visible from far away).
3. The placement service (`ScatterPlacementService.cs`) automatically instantiates these LOD group prefabs, and Unity's renderer handles culling and distance transitions.

---

## 3. Draw Call Optimization (Mobile Pipeline)

Even with GPU instancing enabled, mobile devices can bottleneck if they process too many separate draw calls. 

1. **GPU Instancing**: Ensure your environmental materials have the **Enable GPU Instancing** checkbox ticked in the inspector. This allows Unity to batch identical meshes.
2. **Combine Meshes**: For non-instanced assets (or static background clusters), run the **Static Mesh Combiner** via **Tools → Decnet → Static Mesh Combiner**.
   - Group grass tufts and rocks into single combined meshes.
   - Disable/deactivate the original individual transforms to free up scene hierarchies.
   - This reduces CPU overhead by merging multiple draw calls into a single batch.

---

## Mobile Workflow Checklist

When compiling environments for mobile, verify the following:

- [ ] Target hardware in the **Performance Budget System** is set to **Mobile**.
- [ ] No red budget alerts are active in your scatter windows.
- [ ] Materials use mobile-optimized shaders (e.g. `URP/Simple Lit` or custom mobile shaders).
- [ ] Overlap spheres are clear, with no clipping geometries.
- [ ] Static Mesh Combiner has been run on all non-moving background objects.

> [!WARNING]
> WebGL builds do not support dynamic multi-threaded physics raycasts natively in some browsers. If you are baking terrain candidates using **Mesh Surface Sampler**, perform the bake inside the Unity Editor *before* compiling the WebGL build. Do not run dynamic bakes at runtime.

> [!TIP]
> Use flat unlit or vertex-lit shaders for grass and ground clutter. Lighting calculations on thousands of individual transparent leaf blades will bottleneck mobile fragment shaders, regardless of how optimized your geometry is.
