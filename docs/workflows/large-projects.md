---
title: Large-Scale Environments
layout: default
parent: Workflows
nav_order: 2
---

# Large-Scale & Open-World Environments

**Manage massive environments using grid partitioning, boundary padding, and chunk optimization**

Scattering props across a $4\text{km} \times 4\text{km}$ terrain using a single placement profile will crash Unity or freeze the Editor. The memory overhead of storing hundreds of thousands of candidate points, running long physics raycasts, and managing massive hierarchies must be structured using modular partitioning strategies.

---

## 1. Grid Partitioning & Chunks

Rather than scattering over the entire world at once, split your environment into grid chunks (e.g. $100\text{m} \times 100\text{m}$ or $250\text{m} \times 250\text{m}$).

1. **Create Chunk Anchors**: Place empty GameObjects in your scene to act as grid centers (e.g. `Chunk_0_0`, `Chunk_0_1`).
2. **Scatter by Bounds**: Use the **Target Bounds** option in the main scatter window and set the boundary size to match your chunk dimensions.
3. **Generate & Save**: Generate each chunk independently. The placement service will group GameObjects under the respective chunk parent:
   `[Scatter] Grass_Chunk_0_0`
4. **Scene Streaming**: Grouping objects by chunk allows you to integrate with Unity’s **Scene Loading** APIs or custom streaming tools to load and unload environment details as the player travels.

---

## 2. Boundary Padding & Seamless Edges

When scattering chunks independently, you might notice artificial seams or gaps along the borders where chunks meet.

* **Boundary Padding**: Set a small boundary overlap (e.g. $1\text{m} - 2\text{m}$) beyond the grid bounds during the sampling phase.
* **Seam Stitching**: Ensure candidate points on the borders check the distance to neighbor chunk objects. This prevents plants from double-spawning at the seams or leaving a completely empty corridor between two chunks.

```
  [ Chunk A ]      [ Boundary Seam ]      [ Chunk B ]
  *   *   *   *  │  *   (Overlap)   *  │  *   *   *   *
  *   *   *   *  │    *   *   *   *    │  *   *   *   *
```

---

## 3. Chunk Mesh Combining Workflow

Baking meshes on a massive scale requires balancing draw call reduction against occlusion culling efficiency. Combining a whole $1\text{km}$ map into one mesh means Unity can never cull any part of it, forcing the GPU to process the entire environment even when the player looks at the floor.

* **Optimal Bake Size**: Combine meshes per chunk ($100\text{m}$ bounds). This achieves a healthy balance: draw calls drop from thousands to one per material, but Unity can still cull chunks behind the camera.
* **Exclude LOD0 Trees**: Combine only small ground clutter (grass, rocks, twigs). Keep large trees with complex LOD structures as individual GameObjects or use Unity's native Terrain Tree system so they transition and fade smoothly.

---

## Open-World Workflow Checklist

When scattering details across large maps, follow these practices:

- [ ] Segment the scene into a grid coordinate system.
- [ ] Set **Target Bounds** to match chunk sizes.
- [ ] Save baked meshes as asset files in the project folder (do not keep them inside the scene file to prevent scene size bloat).
- [ ] Combine only low-to-mid detail assets per chunk.
- [ ] Verify that culling works correctly by checking the statistics in the Game view when moving the camera.

> [!WARNING]
> Baking large colliders into a single combined mesh collider can degrade physics performance. Keep colliders on simple props deactivated or use primitive box/capsule colliders at the chunk layer.

> [!TIP]
> Use **O(N) Spatial Grid Hashing** filters to quickly reject duplicate points. The spatial hashing engine runs much faster than standard $O(N^2)$ distance sweeps, which saves hours of generation time when processing large chunk counts.
