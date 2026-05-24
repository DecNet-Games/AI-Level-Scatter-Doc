---
title: AI Prompt Generator
layout: default
parent: Features
nav_order: 2
---

# AI Prompt Generator

The **AI Prompt Generator** tab (located in the main AI Level Scatter window) allows you to translate plain English prompts into mathematical scatter rules.

---

## 1. How Prompt Translation Works

When you enter a prompt (e.g., *"A dense pine forest on steep grassy hills"*), the prompt engine:
1. Enriches the text with standard level design context parameters.
2. Formulates a request containing system instructions to return a structured JSON response matching a specific schema:
   ```json
   {
     "density": 800,
     "normalAlignment": 0.85,
     "surfaceOffset": -0.05,
     "enableHeightFilter": false,
     "enableSlopeFilter": true,
     "minSlope": 0.0,
     "maxSlope": 35.0,
     "enableSpacingFilter": true,
     "minSpacing": 0.4
   }
   ```
3. Sends this request asynchronously via the REST Router.
4. Deserializes the response and modifies the active `ScatterProfile` parameters instantly under Unity's Undo history.

---

## 2. Flagship Feature: Auto-Apply & Spawning

By default, the **Auto-Apply & Place Objects in Scene Hierarchy** toggle is enabled:
* When you click **Generate AI Settings**, the AI configures the profile, immediately runs the surface sampler, executes rule filtration, and instantiates the prefabs in your scene hierarchy in one operation.
* A dialog box will report the exact number of objects placed.
* If you do not like the results, press `Ctrl+Z` to undo the configuration and placement instantly.

---

## 3. Prefab Validation Safeguards

To prevent placement failures, the Prompt Generator features a real-time validation diagnostic panel:
* **Empty Profile Check**: Warns you if the active profile has no prefabs assigned before querying the API.
* **Surface Target Check**: Warns you if no target surface is selected in Mesh mode, disabling the Generate button to prevent exceptions.
* **Validation HelpBox**: Explains missing parameters and guides you on how to resolve them before generating.
