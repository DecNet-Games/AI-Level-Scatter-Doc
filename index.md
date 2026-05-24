---
title: Getting Started
layout: default
nav_order: 1
permalink: /
has_children: true
---

# AI Level Scatter

**AI-Powered Environment Generation & Placement Assistant for Unity**

Manual object placement is a tedious, time-consuming bottleneck that drains creative energy. Placing every rock, grass tuft, and tree by hand costs hours and often results in robotic-looking, uniform layouts.

**Decnet AI Level Scatter** solves this bottleneck. It combines standard spatial geometry constraints (O(N) grid spacing filters, slope checks, normal-aligned surface snapping) with advanced cloud and local AI prompt completions to procedurally construct, paint, and organize 3D environments directly in the Unity Editor.

---

## Why AI Level Scatter?

| The Pain (Manual Placement) | The Solution (Procedural AI) |
| :--- | :--- |
| **Robotic Grid-Spam**: Placing objects mathematically looks artificial and tiled. | **Handcrafted Feel Mode**: Infuses positions with asymmetry, natural clustering, and spacing jitter. |
| **Draw Call Bottlenecks**: High object counts tank gameplay frame rates. | **Static Mesh Combiner**: Groups scattered props by material into optimized 32-bit meshes in one click. |
| **Editor Freezes**: Streaming scene captures to external models lags and hangs. | **Asynchronous Update Engine**: Background operations process on non-blocking main-thread events. |
| **Fragile Rules**: Hard to configure spacing, slope, and height limitations. | **AI Prompt Configurator**: Plain text prompts (e.g. *"jungle cliffs"*) automatically configure profile math. |

---

## Core Features Tour

### 🏰 AI Modular Layout Builder
Go from text description to fully connected 3D room shells natively. The layout builder utilizes a local BFS AABB overlap-free solver to align floors, ceilings, walls, doorways, lighting, and scattered details based on your prompt.

### 🖌 Scatter Paint Brush
Paint prefabs from your profile onto any collider in your SceneView with natural brush strokes. Erase with Shift and resize the brush on-the-fly using the mouse scroll wheel.

### 🛣 Road Path Spline Decorator
Create waypoints in the scene to decorate alongside pathways. Supports left, right, alternating, and double-sided offsets with snap-to-ground physics calculations.

### 🔥 Visual Density Heatmap
Verify placement density instantly with a 32x32 SceneView grid overlay. Easily identify dead zones and performance-heavy hotspots.

---

## Quick Navigation

Ready to get started? Follow the paths below:

* **[Installation Guide](docs/installation.md)**: Import the asset and set up Assembly Definitions.
* **[Quick Start Guide](docs/quick-start.md)**: Populate your first scene in under 5 minutes.
* **[AI Settings Manager](docs/ai-providers/)**: Connect Gemini, OpenAI, Claude, Grok, or local Ollama instances.
* **[Modular Layout Builder Guide](docs/features/ai-modular-layout-builder.md)**: Generate complete 3D levels.

> [!NOTE]
> **Unity Asset Store Mandate**: This tool is a premium product and is **only** officially available through the Unity Asset Store. The GitHub repository is exclusively used for documentation hosting and issue tracking.

> [!TIP]
> Use the menu command `Tools -> Decnet -> Setup Demo Scene` inside Unity to immediately generate a fully working demo scene pre-populated with procedurally generated trees, rocks, and grass to test the editor tools out of the box!

---

## Section Contents

To understand the core design and mechanics, browse the sub-pages in this category:

1. **[Core Concepts](docs/getting-started/core-concepts.md)**: Explains Scatter Profiles, Candidates, Samplers, and Rules.
2. **[Architecture Overview](docs/getting-started/architecture.md)**: Details the interaction between Editor scripts, physics raycasting, and AI prompt routers.
