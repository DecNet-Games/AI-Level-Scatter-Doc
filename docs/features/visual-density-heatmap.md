---
title: Visual Density Heatmap
layout: default
parent: Features
nav_order: 10
---

# Visual Density Heatmap

**Verify placement density and performance hotspots instantly with a SceneView grid overlay**

High asset density is the leading cause of frame rate drops in Unity environments. Over-clustering prefabs leads to severe GPU overdraw, excessive draw calls, and physics collision lag. However, identifying these performance-heavy hotspots visually in a complex 3D scene is incredibly difficult.

The **Visual Density Heatmap** provides an instant, interactive diagnostic overlay directly inside the SceneView. By partitioning your scene bounds into a grid and analyzing object density, it color-codes regions so you can spot dead zones and over-scattered zones immediately.

---

## Technical Architecture

The heatmap engine runs a spatial partitioning grid check across the active scatter boundaries.

### Grid Partitioning & Hotspot Analysis

1. **Spatial Bounds Query**: The system calculates the world-space bounding box containing all objects under the active scatter parent.
2. **Grid Baking**: It partitions this bounding box into a 2D grid (default is $32 \times 32$ cells).
3. **Density Calculation**: For each cell, it calculates the number of active scatter instances overlapping the cell's horizontal boundaries:
   $$\text{Cell Density} = \frac{\text{Objects in Cell}}{\text{Cell Area}}$$
4. **Color-Code Mapping**: Cells are rendered in the SceneView using a color ramp based on target density thresholds:
   - **Blue (Low / Under-saturated)**: $0\% - 20\%$ of expected density. Indicates barren ground or failed raycasts.
   - **Green (Optimal / Balanced)**: $21\% - 70\%$ of expected density. Safe range for gameplay performance.
   - **Yellow (High Warning)**: $71\% - 90\%$ of expected density. Performance warning—LOD setups recommended.
   - **Red (Critical / Over-saturated)**: $>90\%$ of expected density. Hotspot zone containing extreme draw calls or overlapping colliders.

---

## Editor Window Interface

Access this window via **Tools → Decnet → Visual Density Heatmap**. The window displays real-time statistics and drawing options:

* **Grid Resolution**: Configures the subdivision density (from $8 \times 8$ up to $64 \times 64$ cells).
* **Overlay Opacity**: Adjusts the transparency of the SceneView grid tiles ($0\% - 100\%$).
* **Analyze Scene**: Bakes the active scene layout and updates the density metrics.
* **Auto-Refresh**: If enabled, the heatmap dynamically updates as you paint, scatter, or erase objects.
* **Hotspot Statistics Panel**:
  - **Total Bounds Area**: Total square meters analyzed.
  - **Average Density**: Average objects per square meter.
  - **Peak Cell Count**: The highest number of objects crammed into a single cell.
  - **Performance Alert Index**: A color-coded status bar highlighting whether the scene is optimized, moderate, or critically heavy.

```
[SceneView Heatmap Visualizer]
┌───┬───┬───┬───┐
│ B │ G │ G │ Y │   B = Blue (Sparse)
├───┼───┼───┼───┤   G = Green (Optimal)
│ G │ R │ R │ G │   Y = Yellow (Dense)
├───┼───┼───┼───┤   R = Red (Over-saturated Hotspot)
│ G │ G │ B │ B │
└───┴───┴───┴───┘
```

> [!WARNING]
> Keep the heatmap overlay closed or disable **Auto-Refresh** when scattering over 100,000 objects. While the partition algorithm is highly optimized, rendering tens of thousands of transparent Handles in the SceneView every frame can impact editor response times.

> [!TIP]
> If you see large red zones in the heatmap, run the **Static Mesh Combiner** on those specific regions or reduce the **Density** slider in the main scatter window before re-generating.
