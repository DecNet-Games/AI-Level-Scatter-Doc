---
title: Google Gemini
layout: default
parent: AI Providers
nav_order: 1
---

# Google Gemini Configuration

**Step-by-step guide to connect Google's Gemini models for rapid environment generation**

Google Gemini is the default recommended provider for Decnet AI Level Scatter. It offers low latency, high token limits, and robust text-to-JSON formatting capabilities, making it ideal for compiling layout profiles and providing real-time design advice in the Editor.

---

## Step 1: Obtain a Gemini API Key

To connect the Unity Editor to Google’s servers, you must have a valid developer API key:

1. Visit the **[Google AI Studio Console](https://aistudio.google.com/)**.
2. Sign in with your Google account.
3. Click the **Get API Key** button in the sidebar.
4. Select **Create API Key in new project** (or choose an existing Google Cloud project).
5. Copy the generated API key string.

---

## Step 2: Configure Unity Editor Settings

Once you have your key, assign it inside the Unity Editor:

1. Open Unity and select **Tools → Decnet → AI Settings Manager** from the top menu.
2. Select **Google Gemini** from the **Active Provider** dropdown.
3. Paste your copied API key into the **API Key** input field.
4. In the **Model Name** dropdown, select your preferred model:
   - `gemini-1.5-flash`: Recommended for high-speed prompt generation and low-latency chat queries.
   - `gemini-1.5-pro`: Recommended for complex 3D room generation and detailed layout analyses.
5. Click **Save Settings**.

---

## Step 3: Verify the Connection

Click the **Test Connection** button at the bottom of the Settings Manager. The engine will send a lightweight ping request to Google's API endpoint:
`https://generativelanguage.googleapis.com/v1beta/models/`

* **Successful Connection**: The status indicator turns green, showing your connection latency (e.g. `Latency: 110ms (OK)`).
* **Failed Connection (Unauthorized)**: If you receive a `403 Forbidden` or `401 Unauthorized` error, verify that your API key is correct and has not expired, and that Google AI Studio services are active in your region.

---

## Advanced API Parameters

The Gemini provider configures request structures behind the scenes to optimize generation stability:

```json
{
  "contents": [{ "parts":[{ "text": "YOUR_PROMPT" }] }],
  "generationConfig": {
    "response_mime_type": "application/json",
    "temperature": 0.2
  }
}
```

* **JSON Mode**: Decnet automatically requests the `application/json` mime-type for prompt presets. This forces Gemini to return structured placement coordinates rather than standard conversational text.
* **Low Temperature**: The temperature is locked at `0.2` to minimize "hallucinated" layout parameters, ensuring that the model strictly generates valid float scales and integer densities.

> [!NOTE]
> Google Gemini provides a free tier with rate limits suitable for development. If you hit rate limits during intense prompt generation sessions, consider upgrading to the pay-as-you-go tier in Google AI Studio to increase your requests per minute.
