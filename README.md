# Local AI Development Environment (PARA + Ollama Proxy)

An offline, privacy-first local AI engineering setup configured for hardware environments with restricted VRAM (4GB GPU). This repository outlines the architecture combining an Obsidian knowledge graph, optimized local LLMs, and a custom context-compaction proxy to bypass hardware memory limitations.

---

## 🛠️ Hardware Specifications
* **CPU:** Intel i7-11800H @ 2.30GHz
* **GPU:** NVIDIA RTX 3050 Laptop (4GB VRAM)
* **RAM:** 16GB
* **OS:** Windows 11
* **Model Footprint Constraint:** Maximum ~2.3GB VRAM footprint per model to preserve headroom for the KV cache. Models are strictly restricted to $\le$ 4B parameters at Q4 quantization.

---

## 🗂️ 1. Knowledge Management (Obsidian Vault)
The vault uses the **PARA Method** for structure and enforces strict frontmatter formatting to optimize AI parsing and graph readability.

### Vault Structure
* `00 - Inbox` — Quick captures and ephemeral thoughts.
* `01 - Projects` — Active development work.
* `02 - Areas` — Ongoing personal and professional responsibilities.
* `03 - Resources` — Deep learning, references, and AI session states.
* `04 - Journal` — Weekly reviews and logs.
* `05 - Tasks` — Consolidated task tracking.
* `06 - Templates` — Structural note baselines.
* `07 - Archive` — Inactive or completed work.

### Frontmatter Standard
Every markdown note must include a completed frontmatter block. The `summary` field is mandatory to minimize token consumption during local RAG searches.

## 🧠 3. Local AI Stack & VRAM Optimizations (Ollama)
The local engine layer handles language processing locally via an independent background runtime.

### Active Model Manifest
* **Primary Developer / Coding Model:** `qwen2.5-coder:3b` — Specialized low-latency assistant for syntax, refactoring, and file logic.
* **Reasoning, Processing & Compression:** `deepseek-r1:1.5b` (Migration to `phi4-mini` currently pending resolution of the hardware framework blocker).

### System VRAM & KV Cache Optimization
To prevent heavy CPU memory spillover and unlock maximum execution bounds on the restricted 4GB laptop GPU, the Windows environment profile variables must be configured permanently. 

Run the following commands in **Command Prompt (Admin)**:

```cmd
setx OLLAMA_FLASH_ATTENTION "1" /M
setx OLLAMA_KV_CACHE_TYPE "q8_0" /M
setx OLLAMA_MODELS "D:\local agent1.0\OllamaModels" /M