AgentWriter-AI
AgentWriter-AI is an open-source technical blog generation app that turns a single topic prompt into a structured, research-aware article workflow using LangGraph, FastAPI, and Ollama-powered prompting.

It is designed as a lightweight multi-agent writing pipeline:

route the topic into a research or closed-book flow
plan a blog outline with concrete tasks
generate sections in parallel
merge the output into a final markdown article
optionally generate supporting images for the article
✨ Features
Multi-step LangGraph workflow for blog planning and writing
FastAPI + Jinja frontend for an interactive web experience
Optional research mode using Tavily web search
Markdown article generation with section-by-section writers
Image placeholder planning and Gemini image generation support
PostgreSQL-backed LangGraph checkpointing for resumable execution
🏗️ Architecture
This repository contains two core parts:

backend.py — the LangGraph workflow, schemas, planning logic, research node, worker nodes, and reducer/image pipeline
app.py — the FastAPI server that serves the UI and streams execution results
🚀 Quick Start
1. Clone the repository
git clone https://github.com/entbappy/AgentWriter-AI.git
cd AgentWriter-AI
2. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate
On Windows PowerShell:

python -m venv .venv
.\.venv\Scripts\Activate.ps1
3. Install dependencies
pip install -r requirements.txt
4. Start Ollama and pull the model
Make sure Ollama is installed and running locally.

ollama list
ollama pull llama3.1:8b
The workflow expects the Ollama server at:

http://localhost:11434
5. Configure environment variables
Create a .env file in the project root with the required keys:

DATABASE_URL=postgresql://<user>:<password>@<host>:<port>/<db>
TAVILY_API_KEY=your_tavily_key
GROQ_API_KEY=your_groq_key
GOOGLE_API_KEY=your_google_ai_key
Notes:

DATABASE_URL is required for the Postgres checkpointer.
TAVILY_API_KEY is used for research-backed article generation.
GOOGLE_API_KEY is used when image generation is enabled.
GROQ_API_KEY is included in the dependency stack and may be used for alternate model routing flows.
6. Run the app
uvicorn app:app --reload
Then open the local UI in your browser:

http://127.0.0.1:8000
📁 Project Structure
.
├── app.py                 # FastAPI frontend entrypoint
├── backend.py             # LangGraph workflow and article generation logic
├── requirements.txt       # Python dependencies
├── static/                # CSS/JS frontend assets
├── templates/             # Jinja HTML templates
├── notebooks/             # Experiment notebooks
└── images/                # Generated article visuals
🧠 How it Works
The user provides a topic.
The router decides whether the request is closed-book, hybrid, or open-book.
If research is needed, the workflow queries Tavily and gathers evidence.
The orchestrator creates a structured plan with section goals, bullets, and word targets.
Worker nodes write each section independently in Markdown.
The reducer merges all sections and optionally inserts image placeholders.
The final Markdown is saved and exposed through the app UI.
🛠️ Development Notes
The backend.py workflow is intentionally designed to remain the source of truth for the LangGraph graph.
The app reads the compiled graph from backend.app and exposes it through FastAPI.
Generated article outputs are written to the repository root as markdown files.
Generated images are stored in the images/ directory.
🤝 Contributing
Contributions are welcome.

If you would like to improve the project:

Fork the repository
Create a feature branch
Make your changes
Open a pull request with a clear description
📄 License
This project is licensed under the MIT License. See the LICENSE file for details.

🙌 Acknowledgements
This project builds on top of excellent open-source tooling, including:

LangGraph
LangChain
FastAPI
Ollama
PostgreSQL
Tavily
Google GenAI