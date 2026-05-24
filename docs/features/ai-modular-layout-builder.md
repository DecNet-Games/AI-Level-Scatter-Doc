---
title: AI Modular Layout Builder
layout: default
parent: Features
nav_order: 3
---

# AI Modular Layout Builder

The **AI Modular Layout Builder** (`Tools -> Decnet -> AI Modular Layout Builder`) is a native C# room solver that parses layout graphs designed by AI and constructs complete 3D connected dungeon levels directly inside the Unity Editor.

---

## 1. The Generative Workflow

1. **User Description**: Input a layout description (e.g. *"A sci-fi research facility with an entrance lobby, two storage zones, and a reactor core"*).
2. **AI Layout Graph Compilation**: The AI compiles a structured graph mapping:
   - Room IDs and names.
   - Room Types: `spawn` (entrance), `battle`, `loot`, `boss`, or `corridor`.
   - Dimensions: Width, length, and height bounds.
   - Connection pathways (identifying which rooms link together).
3. **Local Math Placement Solver**: A local Breadth-First Search (BFS) solver places rooms sequentially, checking for Axis-Aligned Bounding Box (AABB) overlaps. If an overlap is detected, the room is rotated or pushed in a cardinal direction until clear.

---

## 2. Procedural Geometry Construction

Once room positions are solved, the builder instantiates modular primitives:
* **Shell Generation**: Spawns procedural floors, ceilings, and walls sized dynamically to room dimensions.
* **Doorway Splitting**: Instead of placing a solid wall, if a room connects to another, the wall is procedurally split into three distinct segments:
  - **Left Segment**: Solid wall panel.
  - **Right Segment**: Solid wall panel.
  - **Lintel (Top)**: Solid header wall panel.
  - **Doorframe**: Spawns a decorative dark wood frame around the carved opening.

---

## 3. Theme-Based Ambient & Local Lighting

The builder reads the layout's theme to construct atmosphere using lights:
* **Ambient Sun**: Spawns a directional sun styled to match the theme (e.g. cyan/blue for SciFi, red/dark violet for Horror, warm yellow/gold for Medieval/Industrial).
* **Local Point Lights**: Spawns a point light in the center of each room and an offset fill light in the corner. Intensities and ranges scale automatically based on room dimensions.

---

## 4. Preset Prop Scattering

To make the generated dungeon rooms feel complete:
1. The builder retrieves prefabs from the active Scatter Profile.
2. Based on the room type, it determines clutter density (e.g. high density in loot rooms, focal props in boss chambers, clear paths in corridors).
3. Raycasts scatter the props onto the procedural floors, applying random rotation and scale jitter.
