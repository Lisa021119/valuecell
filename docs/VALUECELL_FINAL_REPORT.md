# ValueCell: A Local Multi-Agent Financial Research Workspace

**Course:** Cornell ENMGT 5400  
**Project:** ValueCell (ENMGT 5400 Course Demo)  
**Submission Type:** Final project report  
**Repository:** `https://github.com/Lisa021119/valuecell`  
**Local Web UI:** `http://localhost:1420` after running `bash start.sh`

---

## 1. Executive Summary

ValueCell is a locally runnable AI product prototype designed for market research, multi-agent analysis, and strategy workflow exploration. The project demonstrates how a financial research question can be transformed into a structured AI workflow: the user submits a prompt through a web interface, the system interprets intent, generates an execution plan, routes tasks to specialized agents, and streams the resulting analysis back to the user.

The project is not intended to serve as a production trading system or a financial advisor. Its purpose is to demonstrate product thinking and technical implementation around AI-assisted financial research. In particular, it focuses on the product design question of how large language models can be embedded inside a broader workflow system rather than used only as a one-shot chatbot.

The final deliverable includes a runnable local application, a GitHub repository, configuration documentation, and this report. The application can be started with `bash start.sh`; once the frontend and backend are running, the main interface is available at `http://localhost:1420`.

---

## 2. Product Vision

The core vision of ValueCell is to make financial research workflows more structured, traceable, and repeatable through multi-agent AI orchestration.

Traditional financial research requires users to move across many disconnected tools: market data websites, company filings, news feeds, spreadsheets, model provider dashboards, and notes. A user may start with a broad question such as, “What is NVIDIA’s competitive position in the AI accelerator market?” but answering that question typically requires multiple steps: clarifying the scope, identifying relevant sources, extracting evidence, comparing competitors, summarizing risks, and forming a structured conclusion.

ValueCell explores how an AI product can coordinate these steps. Instead of treating the model as a single answer generator, the system treats the model as part of an operating workflow. User requests are routed through orchestration, planning, task execution, and response streaming. This makes the product more similar to an AI-enabled research assistant than to a basic chat interface.

The product vision can be summarized as follows:

> ValueCell turns financial research questions into structured, multi-agent workflows that can be planned, executed, reviewed, and iterated locally.

---

## 3. Problem Statement

Financial research is time-consuming because relevant information is fragmented and analysis steps are often repeated manually. Students, analysts, and individual researchers must collect data from many sources, synthesize written information, evaluate risks, and convert qualitative observations into a coherent investment narrative.

The main problems addressed by this project are:

1. **Fragmented workflow:** Financial research often requires switching between multiple tools and sources.
2. **Unstructured AI output:** A single chat response may be useful, but it does not always expose the plan, intermediate steps, or task status.
3. **Limited repeatability:** Without a workflow layer, users may receive different answer formats for similar research questions.
4. **Model configuration friction:** Users may need to switch between cloud models, lower-cost models, and local models depending on budget or quota constraints.
5. **Security and reproducibility concerns:** Course projects should not upload local API keys, virtual environments, dependency folders, or runtime databases to GitHub.

ValueCell addresses these issues by combining a local web UI, backend orchestration, configurable model providers, and specialized financial agents.

---

## 4. Target Users and Use Cases

### 4.1 Target Users

The intended users for this course demo are:

- **Course evaluators:** Professors and teaching assistants who need to review the project, understand the AI architecture, and run the demo locally.
- **Students and researchers:** Users who want to explore how multi-agent AI systems can support financial research.
- **AI product managers:** Users interested in seeing how an AI workflow product can be structured beyond a simple chat interface.

### 4.2 Primary Use Cases

The main use cases are:

1. **Company research:** Ask a question about a company’s competitive position, risks, or recent performance.
2. **Market intelligence:** Summarize trends or key events in a market sector.
3. **News and event tracking:** Use an agent workflow to organize relevant market information.
4. **Strategy experimentation:** Explore how a strategy agent could configure or simulate trading-related workflows in a controlled setting.
5. **Model provider experimentation:** Switch between model providers depending on availability, cost, and model capabilities.

---

## 5. Product Features

### 5.1 Local Web Workspace

ValueCell provides a browser-based interface that runs locally. After starting the application with `bash start.sh`, the user can access the web interface at:

```text
http://localhost:1420
```

The web workspace gives users a central place to navigate agents, settings, watchlists, conversations, and strategy-related modules. This is important because the product is designed as a workflow tool, not only as a backend script.

### 5.2 Research Agent

The Research Agent is the most important module for the course demo. It allows users to submit research-oriented prompts and receive structured analysis. Example prompts include:

```text
Summarize NVIDIA's competitive position in the AI accelerator market,
focusing on demand drivers, key competitors, and risks.
```

The Research Agent demonstrates how AI can help convert a broad research question into a more organized output. It is especially useful for company analysis, market trend summaries, and risk factor identification.

### 5.3 News Agent

The News Agent is designed for event monitoring and information summarization. In a financial context, news is often noisy and time-sensitive. An agent-based workflow can help filter and summarize relevant information for a user-defined topic.

### 5.4 Strategy Agents

The project includes strategy-related agents such as grid strategy and prompt-based strategy agents. These modules demonstrate how research outputs could eventually connect to strategy design or simulated trading workflows.

For this course demo, strategy functionality should be treated as experimental and educational. It should not be presented as a production trading system.

### 5.5 Model Provider Configuration

ValueCell supports multiple model provider configurations through YAML files and environment variables. The project can be configured to use providers such as OpenAI, Google, DeepSeek, OpenRouter, DashScope, SiliconFlow, OpenAI-compatible endpoints, Azure OpenAI, or local Ollama models.

This flexibility matters because model access is not always stable. During development, OpenAI quota limits required switching to a local Ollama-based setup for some workflows. This illustrates a real product consideration: AI applications must plan for provider cost, quota, capability differences, and fallback options.

---

## 6. System Architecture

### 6.1 High-Level Architecture

ValueCell uses a frontend-backend architecture:

- **Frontend:** Built with Bun, React Router, Vite, and TypeScript.
- **Backend:** Built with Python and FastAPI.
- **Agent core:** Includes orchestration, planning, task execution, event routing, and response streaming.
- **Configuration layer:** Uses provider YAML files and runtime environment variables.
- **Storage:** Uses local persistence for conversations, tasks, strategies, and user state.

The core architecture is documented in `docs/CORE_ARCHITECTURE.md`.

### 6.2 Multi-Agent Orchestration

The central AI workflow can be described in six steps:

1. The user submits a question through the web UI.
2. The backend creates or loads the conversation context.
3. A Super Agent evaluates the user’s intent.
4. If the request requires multi-step work, a Planner generates an execution plan.
5. A Task Executor routes planned tasks to specialized agents or tools.
6. Responses are streamed back to the UI and persisted for review.

This architecture is more sophisticated than a direct prompt-to-model call. It separates intention recognition, planning, execution, event routing, and presentation.

### 6.3 Key Components

| Component | Role |
|---|---|
| Super Agent | Performs initial triage and decides whether to answer directly or hand off to the planner. |
| Planner | Converts a user request into an executable plan. |
| Task Executor | Executes planned tasks and coordinates with agent connections. |
| Agent Cards | Describe available agents, their metadata, and their capabilities. |
| Response Router | Converts task status events into UI-consumable responses. |
| Response Buffer | Aggregates partial streaming responses and assigns stable item IDs. |
| Conversation Store | Persists conversations, tasks, and related history. |
| Provider Configs | Manage model providers, model IDs, and fallback choices. |

### 6.4 Why This Architecture Matters

From a product management perspective, this architecture demonstrates that useful AI products often require more than a model API call. A production-style AI workflow must handle planning, state, failures, user feedback, provider configuration, and auditability. ValueCell demonstrates these concerns in a course-scale prototype.

---

## 7. Implementation and Setup

### 7.1 Repository Structure

The repository is organized around three main areas:

```text
frontend/      Web interface and Tauri-related client code
python/        Backend, agents, model adapters, and server logic
docs/          Reports, configuration guide, and architecture notes
```

Supporting files include:

- `start.sh`: macOS / Linux launcher.
- `start.ps1`: Windows launcher.
- `.env.example`: safe template for runtime configuration.
- `README.md`: project overview and run instructions.

### 7.2 Running the Project

On macOS or Linux:

```bash
bash start.sh
```

After startup:

```text
Web UI: http://localhost:1420
API:    http://localhost:8000
```

On Windows:

```powershell
.\start.ps1
```

### 7.3 What the Start Script Does

The launcher performs several setup and runtime tasks:

1. Checks that `bun` and `uv` are installed.
2. Syncs Python dependencies.
3. Initializes the database if needed.
4. Installs frontend dependencies.
5. Starts the frontend development server.
6. Starts the backend API server.

This makes the project easier for evaluators to reproduce because they do not need to manually start each service.

---

## 8. Configuration and Security

### 8.1 Environment Variables

Runtime configuration is handled through environment variables and `.env` files. The repository includes safe templates but does not upload local secrets.

The following files and directories are intentionally excluded from GitHub:

- `.env`
- API keys
- `frontend/node_modules/`
- `python/.venv/`
- runtime database files
- logs
- cache files
- Python `__pycache__` folders

### 8.2 Model Configuration

Model provider settings live under `python/configs/providers/`, and agent-specific model settings live under `python/configs/agents/`. This makes the product configurable without changing application logic.

For example, the system can be configured to use a cloud model for stronger reasoning or a local Ollama model to avoid API quota issues. This tradeoff is important for both product design and live demonstration reliability.

### 8.3 Security Considerations

The project avoids uploading sensitive runtime files to GitHub. The submitted repository includes source code, configuration templates, and documentation, but it does not include local API keys, virtual environments, dependency folders, databases, or runtime logs.

---

## 9. Validation

The current project has been validated at the basic system level.

| Validation Item | Result |
|---|---|
| GitHub repository uploaded | Completed |
| README local run instructions | Updated |
| Frontend URL | `http://localhost:1420` |
| Backend URL | `http://localhost:8000` |
| Frontend health check | HTTP 200 during local verification |
| Backend health check | HTTP 200 during local verification |
| Large dependency folders excluded | Yes |
| Local secrets excluded | Yes |

This validation confirms that the project is runnable and reviewable as a course submission.

---

## 10. Product Risks and Limitations

### 10.1 Model Reliability

Different model providers have different capabilities. Some local models may not support tool calling or may produce weaker reasoning output. Cloud models may be stronger but can fail due to quota, billing, or rate limits. This is a real limitation of AI products and should be acknowledged in evaluation.

### 10.2 Financial Accuracy

AI-generated financial analysis can be incomplete or inaccurate. The system should not be used as a substitute for professional financial advice. All outputs should be treated as educational and exploratory.

### 10.3 Data Source Reliability

Market data, news data, and filings may change over time or be unavailable. A production version would need stronger source validation, citation tracking, and error handling.

### 10.4 Live Demo Risk

Because the project depends on multiple services running at the same time, a live demo may fail if a port is occupied, a provider key is missing, or a model is unavailable. For a graded presentation, it is recommended to prepare a backup recording or screenshots.

---

## 11. Product Management Analysis

### 11.1 User Value

ValueCell provides value by reducing the friction between asking a research question and organizing a structured response. It helps users move from a broad prompt to a workflow with planning, execution, and results.

### 11.2 Technical Feasibility

The project demonstrates feasibility through a working frontend, backend, agent configuration layer, and local startup script. It also shows that a course-scale AI workflow product can be assembled using open-source components and local configuration.

### 11.3 Business and Educational Relevance

The project is relevant to AI product management because it shows how a model can be embedded in a product system. It raises practical considerations around cost, provider selection, reliability, privacy, compliance, and user trust.

### 11.4 Differentiation

Compared with a generic chatbot, ValueCell is differentiated by:

- multi-agent architecture;
- task planning;
- model provider configurability;
- workflow state tracking;
- local-first deployment;
- financial research use cases.

---

## 12. Future Improvements

Several improvements could make ValueCell stronger:

1. **Demo Mode:** Add preloaded prompts and cached successful responses to make classroom demos more stable.
2. **Citation Support:** Attach source links and citations to research outputs.
3. **Model Benchmarking:** Compare output quality across OpenAI, Gemini, DeepSeek, and Ollama.
4. **Risk Controls:** Add explicit guardrails before any strategy execution.
5. **Evaluation Metrics:** Measure response structure, factuality, latency, and cost per task.
6. **User Experience Refinement:** Simplify model setup and improve error messages when providers fail.

---

## 13. Conclusion

ValueCell is a working prototype of a local multi-agent financial research workspace. It demonstrates how an AI product can move beyond a single chat response toward a structured workflow with intent recognition, planning, task execution, streaming, and configuration.

For the ENMGT 5400 final project, the key contribution is not only the existence of a runnable application, but also the product reasoning behind it: identifying a research workflow problem, designing an AI-enabled solution, implementing a multi-agent architecture, managing model provider constraints, and preparing a reproducible GitHub submission.

The project shows that AI product value depends on the system around the model. Planning, routing, configuration, state management, and risk communication are all essential parts of building a useful AI workflow product.

---

## 14. References and Sources

1. Project repository: `https://github.com/Lisa021119/valuecell`
2. Project README: `README.md`
3. Architecture notes: `docs/CORE_ARCHITECTURE.md`
4. Configuration guide: `docs/CONFIGURATION_GUIDE.md`
5. Apache License 2.0: `LICENSE`
6. Cornell ENMGT 5400 final project instructions and proposal requirements
