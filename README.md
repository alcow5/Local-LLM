# 🧠 Private GPT-Like Assistant (Self-Hosted LLM Stack)

This repo outlines how to set up a private, GPT-style assistant on your **personal machine**, capable of processing **documents**, **images**, and performing **web lookups**, all while ensuring **data privacy**.

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
| **Document Parsing** | PyMuPDF, LangChain                     |
| **Image Support**    | LLaVA via Ollama or BLIP2              |
| **Web Lookups**      | Tavily API or SerpAPI + requests       |
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

### 5. Install Open WebUI (Docker Recommended)
```bash
git clone https://github.com/open-webui/open-webui
cd open-webui
docker compose up -d
```
Visit: `http://localhost:3000`

---

## 🧠 Document Question Answering

Install dependencies:
```bash
pip install langchain pymupdf
```

Example script:
```python
from langchain.document_loaders import PyMuPDFLoader
from langchain.chains.question_answering import load_qa_chain
from langchain.llms import Ollama

loader = PyMuPDFLoader("./docs/sample.pdf")
docs = loader.load()

llm = Ollama(model="mistral")
chain = load_qa_chain(llm, chain_type="stuff")
result = chain.run(docs, question="Summarize this document.")
print(result)
```

---

## 🖼️ Image Understanding with LLaVA
```bash
ollama run llava
```
Then in Open WebUI or API, send an image + prompt for description or analysis.

---

## 🌐 Web Lookup Integration
Example using Tavily API:
```python
import requests

def search(query):
    url = f"https://api.tavily.com/search?q={query}&api_key=YOUR_KEY"
    response = requests.get(url)
    return response.json()['answer']
```

---

## 🔒 Remote Access with Tailscale
- [Install Tailscale](https://tailscale.com/download)
- Login and connect your work machine and home PC
- Access Open WebUI via `http://<tailscale-ip>:3000`
- Access Ollama API via `http://<tailscale-ip>:11434`

---

## 🛠️ Optional Enhancements

- Whisper (speech-to-text)
- Text-to-speech readbacks
- ChromaDB + LangChain RAG
- CrewAI or AutoGen for agent workflows

---

## 📦 Roadmap
- [ ] Add Dockerfile for Python + LangChain tools
- [ ] Add WebUI presets for DocQA and ImageQA
- [ ] Bundle setup into single `docker-compose.yml`

---

## 📜 License
MIT

---

## 🙋‍♂️ Questions or Feedback?
Feel free to open an issue or reach out!
