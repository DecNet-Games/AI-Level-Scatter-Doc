---
title: FAQ & Troubleshooting
layout: default
nav_order: 8
---

# FAQ & Troubleshooting

**Frequently Asked Questions and troubleshooting guides for Decnet AI Level Scatter**

Below is a list of common questions, configuration errors, and validation warnings encountered while setting up and using the tool.

---

## 1. Scene & Placement Troubleshooting

### Q: I get the error: *"AI configured settings successfully, but no objects could be placed on the surface. Rejection Reason: No preview data available to place."* How do I fix this?
This warning indicates that the placement engine completed its sampling sweep, but every candidate point was rejected during the filtering phase. Check the following:
1. **Terrain or Mesh Presence**: Ensure your target bounds overlap with a valid terrain or mesh collider.
2. **Raycast Heights**: If using the **Mesh Surface Sampler**, verify that the raycast starts high enough above your meshes. If your ceiling height is too low, the downward raycasts will miss your geometry.
3. **Slope and Height Limits**: Your scatter profile might have restrictive constraints. Try opening your profile and increasing the **Max Slope** angle (e.g. to $45^\circ$ or $60^\circ$) and widening the **Min/Max Height** limits.
4. **Collision Avoidance Layer**: If collision avoidance is checked, ensure the prefabs you are placing do not collide with their own layers, and that the detection radius isn't larger than the spacing between points.

### Q: I get the error: *"Profile is invalid: Total weight of all entries must be greater than zero."*
This error means your active `ScatterProfile` asset does not have any prefabs configured with positive placement weights.
1. Select your active `ScatterProfile` in the Project tab.
2. Look at the **Prefab Entries** list.
3. Ensure you have added at least one prefab entry and that its **Weight** field is set to a value greater than zero (e.g. `1.0` or `5.0`). If all weights are set to `0`, the sampling engine cannot decide which objects to place.

---

## 2. Editor & UI Troubleshooting

### Q: Why is my AI Coach chat window not scrolling when I receive a long response?
This issue has been resolved in version 1.0.0-beta. The layout engine now uses a decoupled scroll-view structure:
- The chat text area uses a dedicated `GUILayout.BeginScrollView` loop.
- The parameter injection panels and input boxes are placed outside the chat scroll container, allowing you to scroll through logs without losing focus on your text input field.
If you are still experiencing scroll freezes, verify that you have imported the latest editor package updates.

### Q: I cannot find the AI Level Scatter tools in the top menu bar.
All editor tools are organized under the unified **Tools → Decnet** hierarchy. If you do not see the menu items:
1. Check the Unity console for compilation errors. If there is a compilation error in another folder of your project, Unity will disable editor menu extensions until it compiles successfully.
2. Ensure you have the `Decnet.AILevelScatter.Editor` assembly definition (`.asmdef`) properly configured and referenced if you are utilizing custom assembly structures.

---

## 3. API & AI Troubleshooting

### Q: The AI Settings Manager connection test fails with a "403 Forbidden" error.
This is almost always related to API key configuration:
- **Google Gemini**: Ensure you have copied the key from the correct Google AI Studio project and that your account has not been suspended.
- **OpenAI**: Ensure you have active developer credits. Note that OpenAI API keys require a pre-paid balance; they do not work on a standard ChatGPT Plus subscription.
- **Local Ollama**: Ensure the Ollama background service is running on your machine. Open a terminal and run `ollama list` to verify.

### Q: Why did the prompt generator create 1,000 lights in my scene instead of scattering pine trees?
If you give an ambiguous prompt (e.g. *"light pine forest"*), local models can occasionally misinterpret the word "light" and generate light source assets rather than spacing rules.
- **Solution**: Refine your prompts to be specific about physical assets (e.g. *"a dense pine forest with grass on flat ground"*).
- **Pro Tip**: Use the **Design Coach** to refine your queries or check the generated settings preview panel before clicking **Instantiate**.

---

## 4. Support & Community

If your issue is not resolved by this FAQ, reach out to our team:

* **[Join the Community Discord](https://discord.gg/decnet)**: Get real-time help from other environment designers and share your procedurally generated maps.
* **[Submit an Issue on GitHub](https://github.com/DecNet-Games/AI-Level-Scatter/issues)**: Report bugs or request new features.
* **Official Support Email**: `support@decnetgames.com`
* **Unity Asset Store Page**: Purchase license and review the asset.

> [!NOTE]
> Please do not distribute the raw package files on public forums. AI Level Scatter is a commercial asset, and sales directly fund future updates, features, and model optimizations.
