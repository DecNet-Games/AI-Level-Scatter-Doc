---
title: Installation
layout: default
nav_order: 2
---

# Installation Guide

To integrate AI Level Scatter into your project, follow this installation guide. 

> [!IMPORTANT]
> **Commercial License Notice**:
> This asset is a premium commercial utility and must be purchased exclusively through the **Unity Asset Store**. Downloading code from GitHub repositories or third-party distribution channels without a valid purchase license is illegal, breaks compiler dependencies, and voids all support.

---

## Technical Requirements

Before importing the package, ensure your project environment meets the following specifications:

| Requirement | Specification |
| :--- | :--- |
| **Unity Version** | Unity 2020.3 LTS, 2021.3 LTS, 2022.3 LTS, or newer. |
| **Render Pipeline** | Compatible with Built-in RP, Universal Render Pipeline (URP), and High Definition Render Pipeline (HDRP). |
| **Scripting Backend** | Compatible with both Mono and IL2CPP. |
| **Physics** | Unity Physics (PhysX) with 3D Physics enabled (required for surface raycasts and collision filtering). |

---

## Step 1: Purchasing and Importing the Asset

1. Open the **Unity Asset Store** in your web browser and purchase **AI Level Scatter**.
2. Open your Unity Project.
3. Open the **Package Manager** window (`Window -> Package Manager`).
4. Select **Packages: My Assets** from the dropdown menu in the top-left.
5. Search for `AI Level Scatter` and click **Download**, then click **Import**.
6. In the Import window, ensure all files under `Assets/Decnet` are checked, and click **Import**.

---

## Step 2: Verifying Assembly Definitions

To keep editor code isolated from your game runtime builds, the package is structured using Unity Assembly Definitions (`.asmdef`).

1. After importing, verify that the assembly definition file `Decnet.LevelScatter.Editor.asmdef` exists under `Assets/Decnet/AI Level Scatter/Editor/Assembly/`.
2. This editor assembly has references configured to `Unity.Editor` assemblies.
3. If you have custom scripts that call the `ScatterProfile` ScriptableObject at runtime, ensure your runtime assemblies reference the core ScriptableObject profile, but **do not** reference the Editor assembly.

---

## Step 3: Initializing Presets

To ensure the placement samplers have working demo prefabs and templates:
1. Open the Unity menu: `Tools -> Decnet -> Setup Default Presets`.
2. A dialog will confirm that three premium presets (`Forest Floor`, `Rock Fall`, and `Debris & Clutter`) along with procedural tree, rock, and grass demo prefabs have been initialized in your project.
3. This creates the folders:
   - `Assets/Decnet/AI Level Scatter/Presets/`
   - `Assets/Decnet/AI Level Scatter/DemoAssets/`

Now that the asset is imported and default assets are configured, you are ready to proceed to the **[Quick Start Guide](quick-start.md)**!
