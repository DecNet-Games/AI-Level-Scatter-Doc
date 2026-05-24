---
title: Main Scatter Window
layout: default
parent: Features
nav_order: 1
---

# Main Scatter Window

The **Main Scatter Window** (`Tools -> Decnet -> AI Level Scatter`) is the primary workspace for environment placement. It contains sliders, settings, and preset dropdowns to manage the core placement logic.

---

## 1. Target Surface Selection

The scatter engine supports two primary ground modes:
* **Terrain Mode**: Automatically identifies the active Unity Terrain. Samples points using native height and normal interpolation APIs.
* **Mesh Mode**: Raycasts down onto a targeted 3D mesh collider (e.g. standard planes, custom terrain meshes, or rock formations).
  * > **Pro Tip**: If your mesh object lacks a collider, the editor will automatically add a `MeshCollider` to enable raycast placement without manual intervention.

---

## 2. Selection Bounds & Viewport Alignment

To target a specific region:
1. Select your ground object in the Hierarchy.
2. Click **Fit Bounds to Selection**. An emerald green boundary wireframe will appear in the SceneView.
3. If no ground exists, the tool will automatically create a plane named `[Decnet Auto Ground]` positioned at your current **SceneView camera focus pivot**, rather than standard origin coordinates.
4. Adjust the **Boundary Padding** slider to prevent elements from bleeding over cliff edges or walls.

---

## 3. Prefab Entry Management

Manage the environmental assets in the active profile:
- **Prefabs**: Assign 3D asset prefabs (trees, stones, grass).
- **Weight**: Relative frequency multiplier. If Prefab A has a weight of 5 and Prefab B has a weight of 1, Prefab A will spawn 5 times more frequently.
- **Scale Range**: Customize scale variance (Min Scale, Max Scale). You can force uniform scaling to prevent stretching.
- **Rotation Offset**: Set random rotation boundaries. By default, the Y axis is set to 0–360° to rotate props naturally.
