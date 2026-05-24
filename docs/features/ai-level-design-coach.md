---
title: AI Level Design Coach
layout: default
parent: Features
nav_order: 4
---

# AI Level Design Coach

The **AI Level Design Coach** (`Tools -> Decnet -> AI Level Design Coach`) is an interactive, floating chat console that helps you configure profiles, solve design problems, and apply settings changes directly to your scene.

---

## 1. Context-Aware Conversations

The AI Coach isn't a general chatbot—it is actively synchronized with your level scatter environment:
* **Active Profile Sync**: Assign your active profile in the selector or click "Sync Active" to automatically pull the current profile configuration from the main scatter window.
* **Settings Enriched Prompt**: When you send a message, the coach wraps it with a context block detailing your current settings (density, spacing, normal alignment, slope limits), allowing the AI to give precise recommendations.

---

## 2. Suggestion Injection System (`[SUGGESTION]`)

If you ask the coach to optimize settings (e.g. *"Give me settings for rocky hills"*), the AI returns suggestions inside a special tag block:
```text
[SUGGESTION: density=200, normalalignment=0.35, enableslopefilter=true, minslope=5, maxslope=65, enablespacingfilter=true, minspacing=1.8]
```
The editor window parses this tag and renders a custom **⚙ OPTIMIZED PARAMETERS SUGGESTED** panel in the chat thread showing a side-by-side view of the values.

---

## 3. One-Click Apply Parameter Suggestion

To apply the recommendations:
1. Click the green **Apply Parameter Suggestion** button inside the chat thread.
2. The parameters are bound directly to the active `ScatterProfile`.
3. The editor registers the transaction with the `Undo` system.
4. The coach automatically triggers a refresh on the main window, causing the SceneView preview to update in real time.
5. The button changes to "Applied ✓" to prevent duplicate applications.
