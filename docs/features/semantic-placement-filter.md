---
title: Semantic Placement Filter
layout: default
parent: Features
nav_order: 7
---

# Semantic Placement Filter

**Context-aware placement rules utilizing physics overlap detection**

Manual environment scattering often ignores surrounding context. Placing grass inside rocks, spawning bushes in the middle of roads, or leaving wall corners barren are common issues with default random scattering tools. 

The **Semantic Placement Filter** solves this by scanning the immediate 3D surroundings of each candidate point using physics overlap queries. It dynamically checks layer names, collider geometry, and naming conventions to apply semantic tags (such as `NearWall`, `NearRoad`, or `OpenField`) and filters out candidates that violate placement restrictions.

---

## Technical Details

The core spatial query engine is powered by `SemanticScatterEngine.cs`. It runs physics overlap checks and filters placement lists prior to scene instantiation.

### Context Analysis Lifecycle

1. **Sphere Overlap Check**: For each candidate placement point, `AnalyzeContext` runs a `Physics.OverlapSphere` check with a configurable detection radius ($0.5\text{m} - 25\text{m}$).
2. **Semantic Tagging**: The engine analyzes returned colliders:
   - **NearWall**: Detected if a collider is on the `"Wall"` layer or the mesh bounds align with a vertical flat surface.
   - **NearRoad**: Detected if a collider contains `"Road"`, `"Path"`, or `"Spline"` in its name or layer.
   - **NearCorner**: Detected if two or more wall colliders intersect at near-orthogonal angles within the search radius.
   - **NearWater**: Detected if a collider represents a trigger boundary with a water tag or shader keyword.
   - **Elevated**: Detected if the point's vertical coordinate is significantly higher than the average surrounding surface heights.
   - **OpenField**: Assigned if no walls, roads, or obstacles are detected within the scan radius.
3. **Filtering Execution**: The `FilterBySemantic` method compares candidates against the active `SemanticFilterSettings` configuration, discarding non-matching entries.

```csharp
[Serializable]
public struct SemanticContext
{
    public bool NearWall;
    public bool NearRoad;
    public bool NearCorner;
    public bool OpenField;
    public bool NearWater;
    public bool Elevated;
    public string DominantTag;
}
```

---

## Editor Window Controls

Open the editor GUI via **Tools → Decnet → Semantic Scatter Filter**. The interface features a premium dark header with a vibrant purple accent.

* **Target Profile**: Select the `ScatterProfile` you wish to apply the filter constraints to.
* **Scan Radius**: Adjust the slider ($0.5\text{m} - 25\text{m}$) to define how far the physics engine looks for surrounding geometry.
* **Semantic Toggles**:
  - **Require Near Wall**: Only scatter assets directly adjacent to wall colliders (ideal for debris, moss, and crates).
  - **Require Open Field**: Discard any candidates near roads, walls, or large obstacles (ideal for meadows and large forests).
  - **Avoid Walls / Avoid Roads**: Explicit exclusion filters to prevent asset clipping.
* **Scan Scene Geometry**: Runs a context scan at the scene origin or active camera pivot and displays detected semantic tags as color-coded badges in the Editor window.
* **Apply Filter**: Commits the active semantic rule parameters to your placement profile.

> [!IMPORTANT]
> Ensure your scene obstacles, walls, and pathways have appropriate colliders (e.g. `BoxCollider`, `MeshCollider`) and are on descriptive layers (or named appropriately). The scanner relies on these tags to build its semantic environment representation.

> [!TIP]
> Use **Avoid Roads** when decorating landscapes with dense forest profiles to ensure your travel routes stay perfectly clear of trees and boulders without needing to define manual exclusion zones.
