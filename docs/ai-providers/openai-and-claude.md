---
title: OpenAI & Claude
layout: default
parent: AI Providers
nav_order: 2
---

# OpenAI & Claude Configuration

**Integrate OpenAI's GPT-4o and Anthropic's Claude 3.5 Sonnet into your editor workflow**

For developers who already have active subscriptions or credits with OpenAI or Anthropic, AI Level Scatter supports full integration with these services. Both platforms offer top-tier layout calculations and design advice.

---

## OpenAI Configuration

### 1. Retrieve API Credentials
1. Go to the **[OpenAI Developer Platform](https://platform.openai.com/)**.
2. Navigate to the **API Keys** tab and click **Create new secret key**.
3. Copy the secret key (it begins with `sk-`).

### 2. Enter Credentials in Unity
1. Select **Tools → Decnet → AI Settings Manager**.
2. Change the **Active Provider** to **OpenAI**.
3. Paste the key into the **API Key** input field.
4. Select the model name:
   - `gpt-4o`: Best overall layout generator.
   - `gpt-4o-mini`: High-speed, cost-effective generation.
5. Click **Test Connection** to verify connection to `https://api.openai.com/v1/chat/completions`.

---

## Anthropic Claude Configuration

### 1. Retrieve API Credentials
1. Go to the **[Anthropic Console](https://console.anthropic.com/)**.
2. Navigate to **API Keys** and generate a new key (begins with `sk-ant-`).

### 2. Enter Credentials in Unity
1. In the **AI Settings Manager**, set **Active Provider** to **Anthropic Claude**.
2. Paste the key into the **API Key** field.
3. Select your model:
   - `claude-3-5-sonnet-latest`: Recommended for its extreme precision in spatial layout rules and design feedback.
   - `claude-3-haiku-latest`: Fast response times for quick scatters.
4. Click **Test Connection** to ping `https://api.anthropic.com/v1/messages`.

---

## API Comparison

| Feature | Google Gemini | OpenAI | Anthropic Claude |
| :--- | :--- | :--- | :--- |
| **Response Latency** | Low ($100-300\text{ms}$) | Medium ($300-600\text{ms}$) | Medium ($400-700\text{ms}$) |
| **JSON Consistency** | Excellent (Native) | Excellent (JSON Mode) | Good (Schema via system prompt) |
| **Setup Overhead** | Extremely Easy | Moderate | Moderate |
| **Free Tier Available** | Yes | No (Developer Credits) | No (Developer Credits) |

---

## Custom API Endpoints (Proxies)

If you use a proxy server or custom endpoint to route your API calls:

1. Enable the **Use Custom Endpoint** checkbox in the AI Settings Manager.
2. Enter your custom base URL (e.g. `https://my-custom-proxy.dev/v1/chat/completions`).
3. Ensure your proxy correctly passes the authorization headers and forwards payload structures without modifying JSON properties.

> [!WARNING]
> Do not use untrusted proxy endpoints. Standard API requests contain your private developer keys, which could be captured by malicious intermediate servers.

> [!TIP]
> If your company restricts external network access in the corporate firewall, look into setting up a local offline proxy or switching to the **Local Offline Ollama** setup to keep all data local.
