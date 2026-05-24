---
title: Core Concepts
layout: default
parent: Getting Started
nav_order: 1
---

# Core Concepts

To effectively use and extend AI Level Scatter, it is essential to understand the primary models and structures that drive the environment placement pipeline.

---

## 1. Scatter Profiles (`ScatterProfile.cs`)

A **Scatter Profile** is a ScriptableObject asset that stores all the parameters and constraints for a specific placement style.
* **Prefab Entries**: A list of prefabs to scatter, each with an individual **weight** (relative frequency), rotation offsets, and scale limits.
* **Density**: The target number of candidate points to sample across the selected area.
* **Alignment**: The normal alignment ratio (0 = upright, 1 = fully aligned to the ground normal).
* **Constraints**: Toggles and values for slope limits, height limits, minimum spacing, and physics collision avoidance.

---

## 2. Scatter Candidates (`ScatterCandidate.cs`)

A **Scatter Candidate** is a lightweight C# struct representing a single potential placement position. It contains:
```csharp
public struct ScatterCandidate
{
    public Vector3 position;
    public Vector3 normal;
    public Quaternion rotation;
    public Vector3 scale;
    public ScatterPrefabEntry prefabEntry;
    public bool isValid;
    public string rejectionReason;
}
```
Candidates are generated during the **Sampling** stage and modified (or marked invalid) during the **Filtering** stage.

---

## 3. Surface Samplers (`ISurfaceSampler.cs`)

Surface samplers generate candidate points across the target boundary.
* **Terrain Sampler (`TerrainSurfaceSampler.cs`)**: Samples coordinates across the terrain boundary. It queries heights using `TerrainData.GetHeight` and surface normals via `TerrainData.GetInterpolatedNormal`.
* **Mesh Sampler (`MeshSurfaceSampler.cs`)**: Utilizes downward physics raycasts (`Physics.Raycast`) from a ceiling height to detect mesh surfaces. If a raycast hits the target collider, a candidate point is created.

---

## 4. Filter Rules (`IScatterRule.cs`)

Rules evaluate candidates and mark them invalid if they violate constraints:
* **Slope Filter (`SlopeRule.cs`)**: Rejects points where the angle between the surface normal and the world up vector exceeds the profile limit.
* **Height Filter (`HeightRule.cs`)**: Rejects points with world Y coordinates outside the profile's min/max limits.
* **Spacing Filter (`SpacingRule.cs`)**: Rejects candidates that are too close to already accepted candidates.
* **Collision Avoidance Filter (`CollisionAvoidanceRule.cs`)**: Uses physics overlap checks (like box or sphere casts) to reject points that collide with static scene obstacles.

---

## 5. Placement Service (`ScatterPlacementService.cs`)

Once candidates are filtered, the **Placement Service** instantiates the accepted candidates in the scene:
* **Prefab Linking**: Utilizes `PrefabUtility.InstantiatePrefab` to preserve prefab links in the scene (crucial for staging and future prefab updates).
* **Named Grouping**: Groups all placed instances under a single parent object named `[Scatter] <ProfileName>_Seed<SeedValue>`.
* **Undo/Redo Integration**: Registers all spawned GameObjects and parent folders with Unity's `Undo` system (supporting full single-stroke reversion).
