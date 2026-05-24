---
title: Ollama & Local Offline
layout: default
parent: AI Providers
nav_order: 3
---

# Ollama & Local Offline Configuration

**Run open-source LLMs locally on your GPU for a completely offline environment generation workflow**

Sending scene data or prompt queries to cloud servers can raise security and privacy concerns in some studio settings. It also leaves your workflow dependent on an active internet connection.

Decnet AI Level Scatter supports full integration with **Ollama**, a local model runner. This allows you to run models like Llama 3, Mistral, or Qwen directly on your development machine's GPU. It is completely free, runs offline, and doesn't require any API keys.

---

## Step 1: Install Ollama on your Machine

Ollama handles download, orchestration, and GPU acceleration for local models.

1. Go to the **[Ollama Official Website](https://ollama.com/)** and download the installer for your OS (Windows, macOS, or Linux).
2. Run the installer and launch the Ollama application.
3. Open a terminal or command prompt (PowerShell) on your machine and verify the installation:
   ```bash
   ollama --version
   ```

---

## Step 2: Download a Compatible Model

You need to pull a model to run locally. We recommend the following models for environment scattering:

* **Llama 3 (8B)**: Best overall local model. High spatial logic and reliable JSON formatting.
  ```bash
  ollama run llama3
  ```
* **Mistral (7B)**: Fast response times with lower VRAM requirements.
  ```bash
  ollama run mistral
  ```
* **Qwen 2 (7B / 1.5B)**: Excellent performance on lower-end systems, with the 1.5B model running comfortably on entry-level laptops.
  ```bash
  ollama run qwen2:1.5b
  ```

Once the model finishes downloading, you can close the terminal or keep the command running. The Ollama service will run in the background.

---

## Step 3: Configure Unity Settings

Now point your Unity Editor to your local Ollama port:

1. In Unity, select **Tools → Decnet → AI Settings Manager**.
2. Select **Ollama (Local)** from the **Active Provider** dropdown.
3. Keep the **Ollama Endpoint** set to the default:
   `http://localhost:11434/api/generate`
4. In the **Model Name** field, type the exact name of the model you pulled (e.g. `llama3` or `mistral`).
5. Leave the API Key field blank (Ollama does not require keys).
6. Click **Save Settings**.

---

## Step 4: Run a Connection Test

Click the **Test Connection** button. The engine will send a lightweight query request to your local Ollama port.

* **Successful Connection**: The indicator turns green, showing local response latency.
* **Failed Connection**:
  - Double check that the Ollama app is running in your taskbar.
  - Verify that the port `11434` is not blocked by third-party firewalls or anti-virus systems.
  - Ensure the model name spelled in Unity matches the model downloaded in Ollama exactly.

---

## Local Hardware Requirements

Running LLMs locally requires dedicated hardware to prevent editor frame rate drops during generation:

* **Recommended**: NVIDIA RTX 3060/4060 (or higher) with at least 8 GB of VRAM. This allows the model to stay loaded in GPU memory alongside the Unity Editor.
* **Minimum**: 16 GB of System RAM if running on CPU (note that CPU generation is significantly slower and may cause brief freezes during layout baking).

> [!NOTE]
> When you click generate, the local GPU will spike briefly as the model compiles the layout. This is expected. If you experience editor lag, close other GPU-intensive applications (e.g. Photoshop, blender, or open web browser tabs) before running scatters.

> [!TIP]
> If you run a local model on a separate machine inside your local area network (LAN) to save resources, change `localhost` in the endpoint URL to the target machine's IP address (e.g. `http://192.168.1.50:11434/api/generate`).
