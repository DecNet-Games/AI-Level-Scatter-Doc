---
title: Quick Start
layout: default
nav_order: 3
---

# Quick Start Guide

This guide will walk you through your first environment placement in under 5 minutes. You can choose between the **Automated Demo Workflow** (recommended for testing the tool instantly) or the **Manual Placement Workflow** (for adding objects to your own scene).

---

## Method A: Automated Demo Workflow (Recommended)

To bypass all manual collider, bounds, and profile setup:
1. Open the Unity menu: **Tools -> Decnet -> Setup Demo Scene**.
2. Click **Awesome** in the popup dialog.
3. The editor will automatically:
   - Create and load a new scene: `Assets/Decnet/AI Level Scatter/Demo/DemoScene.unity`.
   - Instantiate a ground plane with a grass material and collider.
   - Run the scattering algorithm using the `ForestFloorProfile` preset.
   - Spawn a floating **3D Text User Guide** directly in your scene view.
4. You can immediately hit Play or use the editor tools to experiment.

---

## Method B: Manual Placement Workflow

If you want to configure scattering on your own terrains or custom meshes, follow these steps:

### 1. Select the Target Surface
1. Open the main window: **Tools -> Decnet -> AI Level Scatter**.
2. Select your ground plane or terrain in the Hierarchy.
3. In the **⚙ Scatter** tab of the tool window, click the **Use Selected** button next to the **Target Surface** field.
4. If your target mesh does not have a collider, the tool will automatically add a `MeshCollider` and log a notification in the Console.

### 2. Configure Selection Bounds
1. Click the **Fit Bounds to Selection** button.
2. An emerald-colored bounding box will overlay your selected ground object in the SceneView.
3. *(Optional)* Adjust the **Boundary Padding** slider to pull placement away from the outer edges (this is automatically clamped to 40% of the bounds size to prevent errors).

### 3. Assign a Scatter Profile
1. Click **Create New Profile** to write a new ScriptableObject configuration asset, or assign an existing preset profile (e.g. `ForestFloorProfile` from `Assets/Decnet/AI Level Scatter/Presets/`).
2. > **Self-Healing Feature**: If you create a brand-new profile and leave its prefab list empty, the tool will automatically generate and attach high-quality procedural trees, rocks, and grass demo prefabs on the fly, allowing you to test right away.

### 4. Preview and Apply
1. Click **Generate Preview**. The tool will calculate valid placement candidates and draw wireframes in the SceneView:
   - **Green Circles / Lines**: Accepted candidates.
   - **Red Crosses**: Rejected candidates (clumped too close, slope is too steep, or outside height boundaries).
2. Adjust the **Density** (sample points) and **Min Spacing** sliders, and click **Generate Preview** to see the changes update in real time.
3. Once satisfied, click **Apply Placement**.
4. All spawned prefabs are grouped under a `[Scatter]` parent in the Hierarchy. Press `Ctrl+Z` to undo the placement at any time.

---

Next up, learn about the core architectural concepts of the tool in the **[Core Concepts Guide](getting-started/core-concepts.md)**!
