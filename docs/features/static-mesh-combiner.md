---
title: Static Mesh Combiner
layout: default
parent: Features
nav_order: 13
---

# Static Mesh Combiner

**Group scattered environment props by material and merge them into optimized 32-bit meshes in one click**

Scattering thousands of trees, rocks, and grass tufts across a landscape can quickly tank game frame rates. Each individual object requires its own draw call (CPU-to-GPU render commands), leading to major CPU bottlenecks, even with GPU instancing. 

The **Static Mesh Combiner** resolves this bottleneck. It groups scattered GameObjects by their shared materials, merges their geometries into single meshes, and configures them with optimized 32-bit index buffers. This can reduce hundreds of draw calls down to a single digit, significantly improving rendering performance.

---

## Technical Mechanism

The mesh merging process uses Unity’s `Mesh.CombineMeshes` API with custom sub-mesh groupings:

1. **Hierarchy Scan**: The engine locates all `MeshFilter` and `MeshRenderer` components parented under the active scatter container.
2. **Material Grouping**: It builds a dictionary grouping all meshes by their assigned material:
   $$\text{Material Group} = \{ \text{Mesh } A, \text{Mesh } B, \dots \} \quad \text{where } \text{Mat}_A = \text{Mat}_B$$
3. **Matrix Transformation**: The vertex positions of each individual mesh are transformed from local space to the parent's world space using their local-to-world transformation matrices.
4. **32-Bit Index Buffer Allocation**: By default, Unity meshes use 16-bit index buffers (limiting meshes to 65,535 vertices). The Static Mesh Combiner sets `mesh.indexFormat = IndexFormat.UInt32` (32-bit buffers) when combining, allowing it to merge hundreds of thousands of vertices into a single combined mesh.
5. **Collider Generation**: The tool automatically combines collider geometries into a single compound mesh collider or retains box/sphere colliders in place for physics performance.

---

## Editor Window Controls

Open the utility from the main window or via **Tools → Decnet → Static Mesh Combiner**. The GUI features a premium dark header with a sharp steel-grey/green accent.

* **Target Object**: Assign the parent GameObject containing the scattered props you wish to merge (e.g. `[Scatter] Forest_Floor`).
* **Combine Settings**:
  - **Generate Colliders**: Automatically generates a single optimized mesh collider for the combined geometry.
  - **Deactivate Originals**: Toggles whether to deactivate or completely destroy the original individual GameObjects after combining.
  - **Save Combined Meshes**: If enabled, saves the newly baked `.mesh` assets to a `Baked_Meshes` folder in your project directory (necessary for creating prefabs or build staging).
* **Bake Meshes**: Runs the combine operation. The original GameObjects are organized into a backup folder (or deleted), and the combined mesh is rendered in their place.
* **Undo Combine**: Reverts the bake, restoring the individual GameObjects back to their active state with full transform data.

```
[Before: Individual GameObjects]      [After: Static Combined Mesh]
   Parent [Scatter]                      Parent [Scatter]
     ├── Tree_01 (Mesh + Material)          └── CombinedMesh_Grass (1 Draw Call)
     ├── Tree_02 (Mesh + Material)          └── CombinedMesh_Tree (1 Draw Call)
     └── Grass_01 (Mesh + Material)
     [Total Draw Calls: 300+]               [Total Draw Calls: 2]
```

---

## Draw Call Reduction Metrics

In a typical scene containing 1,500 low-poly trees and rocks:

| Metric | Before Combination | After Combination | Improvement |
| :--- | :--- | :--- | :--- |
| **Draw Calls** | $1,524$ | $2$ | **$99.8\%$ Reduction** |
| **CPU Frame Time** | $8.4\text{ms}$ | $1.1\text{ms}$ | **$7.6\times$ Performance Boost** |
| **GPU Overdraw** | High (overlapping draw order) | Low (optimized depth sort) | **Smooth Frame Pacing** |

> [!WARNING]
> Combined meshes cannot be individually moved or edited after baking. If you need to paint, erase, or rearrange objects, click **Undo Combine** (or restore from the backup folder), perform your edits, and then re-bake.

> [!TIP]
> Combined meshes are fully compatible with **Unity Occlusion Culling**. For extremely large environments, partition your scatters into $50\text{m} \times 50\text{m}$ grid chunks and combine each chunk separately. This allows Unity to cull entire chunks when they are out of the camera's viewport.
