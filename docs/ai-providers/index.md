---
title: AI Providers
layout: default
nav_order: 6
has_children: true
---

# AI Providers Configuration

**Connect cloud APIs or local instances to power the prompt generator and design coach**

Decnet AI Level Scatter doesn't lock you into a single proprietary AI model. It features a modular **AI API Client Layer** that allows you to connect to a variety of artificial intelligence models, including cloud-based LLMs (like Google Gemini, OpenAI GPT-4o, and Anthropic Claude) and local offline instances (using Ollama).

Configure and manage your API keys, base endpoints, model names, and connection tests in one unified interface.

---

## AI Settings Manager

Open the configuration window via **Tools → Decnet → AI Settings Manager**. 

From this panel, you can choose which API provider is active, toggle offline modes, input credentials, and test connection latency:

```
┌──────────────────────────────────────────────┐
│             AI Settings Manager              │
├──────────────────────────────────────────────┤
│  Active Provider: [ Google Gemini        ]  │
│  Model Name:      [ gemini-1.5-pro-latest ]  │
│  API Key:         [ ********************  ]  │
│                                              │
│  [ Test Connection ]  Latency: 120ms (OK)   │
└──────────────────────────────────────────────┘
```

---

## Provider Documentation

Select a sub-page below to configure your specific AI provider:

1. **[Google Gemini Setup](google-gemini.md)**: Standard provider for text-to-preset compilation and design coach queries.
2. **[OpenAI & Anthropic Claude](openai-and-claude.md)**: Configuring GPT-4o and Claude 3.5 Sonnet cloud endpoints.
3. **[Local Offline Ollama](ollama-and-local.md)**: Running open-source models (like Llama 3 or Mistral) locally on your GPU for a fully offline workflow.

> [!WARNING]
> Keep your API keys private. Never commit your `ProjectSettings` folder or files containing raw API keys to public repositories. Decnet stores API credentials locally inside Unity's `EditorPrefs` (which are unique to your local machine registry), not inside project asset files.
