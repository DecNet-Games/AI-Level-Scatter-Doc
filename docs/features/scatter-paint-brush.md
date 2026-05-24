---
title: Scatter Paint Brush
layout: default
parent: Features
nav_order: 5
---

# Scatter Paint Brush

The **Scatter Paint Brush** (`Tools -> Decnet -> Scatter Paint Brush`) is an interactive brush tool that lets designers paint assets from their profile directly onto scene geometry.

---

## 1. Activation & Visual Guides

1. Open the tool window and select your **Scatter Profile**.
2. Adjust brush radius, density, and stroke strength parameters.
3. Click **▶ ACTIVATE BRUSH**. 
4. Move your mouse into the SceneView. A circular brush indicator showing the current radius will track your cursor across any collider surface.

---

## 2. Dynamic Controls

| Action | Control Shortcut |
| :--- | :--- |
| **Paint Assets** | Click and Drag Left Mouse Button (LMB). |
| **Erase Painted Assets** | Hold `Shift` while dragging LMB. |
| **Resize Brush Radius** | Scroll Mouse Wheel (without holding Alt). |

---

## 3. Placement Safeguards & Spacing

Even though you are painting manually, the brush respects your profile constraints:
* **Minimum Spacing**: Before spawning a prefab at a hit coordinate, the brush scans neighboring painted objects. If a collision falls within the profile's `minSpacing`, the prefab is rejected, preventing overlap.
* **Physics Collision Check**: Integrates with layer mask settings to prevent placing grass or trees inside static stone pillars or walls.
* **Normal Alignment**: Projects the painted prefab's up vector according to the hit surface normal (useful for painting moss on cliff slopes or flowers on hills).

---

## 4. Undo and Stroke Collapse

Every mouse drag represents a "Paint Stroke". To prevent cluttering your Unity Undo queue:
* Spawning hundreds of painted objects in a single drag is grouped into a single Undo transaction.
* Click **Undo Last Stroke** or press `Ctrl+Z` to revert the entire stroke.
* Click **Clear All Painted** to completely delete the session's painted assets.
