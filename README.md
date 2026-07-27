# ✈️ AI Travel Planning System (LangGraph Multi-Agent)

A real-world multi-agent AI system that plans a complete trip automatically — flights, hotels, and a day-by-day itinerary — using **LangGraph**, **LangChain**, and **Groq's Llama 3.3 70B**, with a Streamlit web interface.

🔗 **Live Demo:** [travel-plan-ai-stvmgwq2e6zarzqntdcy2g.streamlit.app](https://travel-plan-ai-stvmgwq2e6zarzqntdcy2g.streamlit.app/)

---

## Overview

The system uses **4 cooperating AI agents**, orchestrated as a LangGraph state graph, to turn a single natural-language travel request into a complete trip plan:

```
User Query → Flight Agent → Hotel Agent → Itinerary Agent → Final Response Agent
```

Each agent has a focused responsibility, and their outputs are combined into one coherent travel plan — with conversation memory persisted across sessions using PostgreSQL.

## Features

- ✈️ **Flight Search Agent** — fetches flight options in real time
- 🏨 **Hotel Search Agent** — finds relevant hotel results via web search
- 🗓️ **Itinerary Planning Agent** — builds a day-by-day travel plan
- 🤖 **Final Response Agent** — synthesizes everything into one clear answer
- 🧠 **Long-term memory** — conversation state persisted in PostgreSQL via LangGraph checkpointing
- 🌐 **Real-time API integration** — live flight and web search data
- 💻 **Streamlit web interface** — clean, interactive chat-style UI

## Tech Stack

| Layer | Technology |
|---|---|
| Agent orchestration | LangGraph, LangChain |
| LLM | Groq — Llama 3.3 70B |
| Memory / persistence | PostgreSQL (via `langgraph-checkpoint-postgres`) |
| Web interface | Streamlit |
| Search | Tavily API |
| Flight data | AviationStack API |

## Architecture

```
┌──────────────┐   ┌──────────────┐   ┌───────────────────┐   ┌────────────────┐
│ Flight Agent │ → │ Hotel Agent  │ → │ Itinerary Agent    │ → │ Final Response │
│ (AviationStack)│  │ (Tavily)     │   │ (Groq / Llama 3.3) │   │ Agent          │
└──────────────┘   └──────────────┘   └───────────────────┘   └────────────────┘
        │                                                              │
        └──────────────────── PostgreSQL Checkpoint Memory ────────────┘
```

## Running Locally

### 1. Clone and set up the environment
```bash
git clone https://github.com/kokitkarganesh/travel-plan-ai.git
cd travel-plan-ai
python -m venv langgraph_env3
langgraph_env3\Scripts\activate   # Windows
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Set up PostgreSQL
Create a database (locally or via a hosted provider like Render/Supabase):
```sql
CREATE DATABASE langgraph_memory;
```

### 4. Configure environment variables
Create a `.env` file in the project root:
```
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
AVIATIONSTACK_API_KEY=your_aviationstack_api_key
DATABASE_URL=postgresql://user:password@host:port/langgraph_memory
```

Get your keys here:
- Groq → [console.groq.com](https://console.groq.com)
- Tavily → [tavily.com](https://tavily.com)
- AviationStack → [aviationstack.com](https://aviationstack.com)

### 5. Run it

**Terminal (test the agent pipeline directly):**
```bash
python main.py
```

**Web app:**
```bash
streamlit run frontend.py
```

## Example Prompt

```
Plan a complete 7 days Japan trip including flights, hotels and sightseeing under 2 lakhs.
```

## Deployment

This app is deployed on **Streamlit Community Cloud**, with a managed PostgreSQL database for persistent memory. Environment variables are configured via Streamlit's Secrets manager rather than a committed `.env` file.

## Project Structure

```
travel-plan-ai/
├── main.py              # LangGraph agent pipeline (CLI entry point)
├── frontend.py           # Streamlit web interface
├── tools/
│   ├── flight_tool.py     # AviationStack flight search
│   └── tavily_tool.py     # Tavily hotel/web search
├── requirements.txt
└── README.md
```

## Author

**Ganesh Kokitkar**
- GitHub: [github.com/kokitkarganesh](https://github.com/kokitkarganesh)
- LinkedIn: [linkedin.com/in/ganesh-kokitkar](https://linkedin.com/in/ganesh-kokitkar)
