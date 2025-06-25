# 🧠 Private GPT-Like Assistant (Self-Hosted LLM Stack)

This repo outlines how to set up a private, GPT-style assistant on your **personal machine**, capable of processing **documents**, **images**, generating visual content using Stable Diffusion, and performing **web lookups**, all while ensuring **data privacy**.

---

## 🖥️ System Requirements

- GPU: NVIDIA 4070 (12GB+) or better
- RAM: 32GB minimum (64GB recommended)
- OS: Windows, Linux, or WSL2
- Tools: Docker, Python 3.10+, Git

---

## 🧩 Stack Overview

| Component            | Tool                                  |
|----------------------|----------------------------------------|
| **LLM Host**         | [Ollama](https://ollama.com)           |
| **Chat UI**          | [Open WebUI](https://github.com/open-webui/open-webui) |
| **Document Parsing** | PyMuPDF, python-docx, pandas           |
| **Image Support**    | LLaVA via Ollama or BLIP2              |
| **Web Lookups**      | Tavily API or SerpAPI + requests       |
| **Image Generation** | Stable Diffusion + FastAPI (custom)    |
| **Remote Access**    | Tailscale                              |

---

## ⚙️ Setup Steps

### 1. Clone This Repo
```bash
git clone https://github.com/yourname/private-llm-stack.git
cd private-llm-stack
```

### 2. Install Ollama
```bash
# macOS/Linux
curl -fsSL https://ollama.com/install.sh | sh

# Windows (PowerShell)
winget install Ollama.Ollama
```

### 3. Pull Base Models
```bash
ollama pull mistral          # Text LLM
ollama pull llava            # Vision LLM (image support)
```

### 4. Run Ollama
```bash
ollama serve &
```

---

### 5. Install Open WebUI with Docker Compose
Create a `docker-compose.yml` file:

```yaml
version: "3"
services:
  webui:
    image: ghcr.io/open-webui/open-webui:latest
    ports:
      - "3000:8080"
    volumes:
      - open-webui-data:/app/backend/data
    environment:
      - OLLAMA_BASE_URL=http://host.docker.internal:11434
      - WEBUI_HOST=0.0.0.0
    restart: unless-stopped

  sd-api:
    build: ./sd-api
    ports:
      - "5000:5000"
    volumes:
      - ./outputs:/app/outputs
    restart: unless-stopped

volumes:
  open-webui-data:
```

Then run:
```bash
docker compose up -d
```
Visit: `http://localhost:3000`

---

## 🧠 Document + Excel Summarization Tool

Install dependencies:
```bash
pip install langchain pymupdf python-docx pandas openpyxl
```

Python script (CLI or FastAPI-based) parses:
- PDFs via `PyMuPDF`
- Word files via `python-docx`
- Excel files via `pandas`

Use LangChain to summarize extracted content via Ollama's API.

---

## 🖼️ Image Understanding with LLaVA
```bash
ollama run llava
```
Use image prompts inside WebUI to analyze and describe images.

---

## 🎨 Image Generation with Stable Diffusion
Use included container `sd-api`:
- Hosted on `localhost:5000`
- Accepts POST request with `{ "prompt": "..." }`
- Generates an image using Stable Diffusion and saves to `/app/outputs`

Example request:
```bash
curl -X POST http://localhost:5000/generate -H "Content-Type: application/json" -d '{"prompt":"a cyberpunk city at night"}'
```

Returns path or base64 image URL.

---

## 🌐 Web Lookup Integration
Use Tavily or SerpAPI in a Python or LangChain wrapper to enrich answers when internet access is needed.

---

## 🔒 Remote Access with Tailscale
- [Install Tailscale](https://tailscale.com/download)
- Log into the same account on your PC + work laptop or phone
- Use `http://<tailscale-ip>:3000` from any device

---

## 📱 Mobile Access
- Install Tailscale on iOS/Android
- Add `http://<tailscale-ip>:3000` to your home screen (Safari/Chrome "Add to Home Screen")

---

## 📦 Roadmap
- [x] Add Dockerfile for Python + LangChain tools
- [x] Add WebUI presets for DocQA and ImageQA
- [x] Bundle Stable Diffusion as second container
- [ ] Add UI buttons to call SD and doc parser directly
- [ ] Add voice-to-text via Whisper

---

## 📜 License
MIT

---

## 🙋‍♂️ Questions or Feedback?
Feel free to open an issue or reach out!
