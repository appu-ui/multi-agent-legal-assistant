# ⚖️ Multi-Agent Legal Assistant (CrewAI)

An intelligent, multi-agent legal workflow that turns a plain-English legal problem into a structured legal output.

This project orchestrates specialized AI agents to:
- Understand and classify a legal issue,
- Map it to relevant **Indian Penal Code (IPC)** sections,
- Retrieve supporting precedent references from trusted legal sources,
- Draft a formal legal complaint/notice-style document.

---

## ✨ Why this is cool

Most legal assistants stop at “chat.” This one runs a **full legal reasoning pipeline** with role-based agents and task context handoffs.

You get a practical, end-to-end legal drafting flow in one run:
1. **Case Intake Agent** parses and structures your issue,
2. **IPC Section Agent** performs vector-based IPC retrieval,
3. **Legal Precedent Agent** fetches relevant case references,
4. **Legal Drafter Agent** produces a formal draft output.

---

## 🧠 Architecture at a glance

### Agents
- `Case Intake Agent` → classifies case type, domain, entities, and summary.
- `IPC Section Agent` → uses a vector search tool for top IPC matches.
- `Legal Precedent Agent` → searches trusted domains for precedent references.
- `Legal Document Drafting Agent` → composes final legal document.

### Tools
- **IPC Sections Search Tool** (`tools/ipc_sections_search_tool.py`)
  Uses Chroma + HuggingFace embeddings to retrieve top IPC sections from local vector DB.

- **Legal Precedent Search Tool** (`tools/legal_precedent_search_tool.py`)
  Uses Tavily search and filters to trusted legal domains (currently `indiankanoon.org`).

### Orchestration
`crew.py` wires all agents + tasks into a single CrewAI pipeline with contextual task dependencies.

---

## 📁 Project structure

```text
multi-agent-legal-assistant/
├── agents/
│   ├── case_intake_agent.py
│   ├── ipc_section_agent.py
│   ├── legal_precedent_agent.py
│   └── legal_drafter_agent.py
├── tasks/
│   ├── case_intake_task.py
│   ├── ipc_section_task.py
│   ├── legal_precedent_task.py
│   └── legal_drafter_task.py
├── tools/
│   ├── ipc_sections_search_tool.py
│   └── legal_precedent_search_tool.py
├── vectordb/
├── app.py                # Streamlit UI
├── main.py               # CLI-style runner
├── crew.py               # Crew definition
├── requirements.txt
└── README.md
```

---

## 🚀 Quickstart

### 1) Clone and install

```bash
git clone <your-repo-url>
cd multi-agent-legal-assistant
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 2) Configure environment variables

Create a `.env` file in the project root:

```env
# LLM provider keys/config for CrewAI (as needed by your setup)
GROQ_API_KEY=your_groq_api_key

# Tavily for precedent search
TAVILY_API_KEY=your_tavily_api_key

# IPC vector DB settings
PERSIST_DIRECTORY_PATH=./vectordb
IPC_COLLECTION_NAME=ipc_sections
```

> Note: Ensure your vector DB collection name and persist path match the values used when building/loading your IPC database.

### 3) Run the Streamlit app

```bash
streamlit run app.py
```

### 4) Or run via script

```bash
python main.py
```

---

## 🧪 Example input

> “A man broke into my house at night, stole jewelry and cash, threatened me with a knife, and ran away. Which IPC charges apply?”

The pipeline returns structured analysis + relevant IPC references + precedent summary + a draft legal document.

---

## 🛠️ Tech stack

- **CrewAI** (agent/task orchestration)
- **Groq-hosted LLM** (`llama-3.3-70b-versatile` in current config)
- **LangChain + Chroma** (vector retrieval)
- **HuggingFace Embeddings**
- **Tavily Search API**
- **Streamlit** (UI)

---

## ⚠️ Important disclaimer

This project is for **educational and prototyping purposes only** and is **not legal advice**.
Always consult a qualified lawyer before taking legal action.

---

## 🤝 Contributing

PRs are welcome.
If you contribute, consider adding:
- Better legal source validation,
- Jurisdiction-specific drafting templates,
- Stronger citation formatting,
- Evaluation benchmarks for legal retrieval quality.

---

## ⭐ If you like this project

Give it a star and share it with builders working on legal-tech + agentic AI.
