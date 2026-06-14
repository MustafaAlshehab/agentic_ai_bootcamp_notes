# Introduction to CrewAI: Multi-Agent Orchestration from Scratch

## TL;DR

This session introduced **CrewAI** as the third pillar of the agentic AI curriculum (alongside LangChain and LangGraph). The instructor recapped agents, ReAct, tools, and LangGraph state graphs, then walked through CrewAI’s core model—**crews** made of **agents** and **tasks**—and the standard workflow: define an LLM, agents, tasks, a crew, then call `kickoff()`. Students scaffolded a **report summarization** project locally with `crewai create crew`, ran it (with dependency fixes), and finished with a **Google Colab** notebook building a single-agent **email writer** from scratch using `kickoff_async`.

## Topics Covered

- Course recap: agentic AI, AI agents, tools/toolkits, LangChain project, LangGraph ReAct + StateGraph
- Upcoming schedule: tomorrow = industry LangGraph + CrewAI projects; next week = security, guardrails, MCP, deployment; week 12 = **n8n**
- RAG assignment feedback (Victoria winner; strict scoring; Discord DM limits)
- LangChain vs LangGraph (interview framing)
- LangGraph state graph: nodes, edges, conditional edges
- CrewAI overview: crews, agents, tasks, orchestration flow
- Agentic AI components and agent anatomy (brain, tools, memory, orchestrator, supervisor)
- Chain-of-thought (CoT) prompting recap
- Brief AI evolution timeline (2017 → 2026)
- CrewAI installation: Python 3.10–3.13, **UV**, `crewai create crew`
- Local project walkthrough: folder structure, YAML configs, `.env`, `crew.py`, `main.py`
- Hands-on: **report summarization** crew (researcher + reporting analyst)
- Dependency troubleshooting (LiteLLM, FastAPI, APScheduler)
- Break (~10 minutes)
- Google Colab: CrewAI from scratch (email writer agent)
- `kickoff` vs `kickoff_async` in Colab
- Wrap-up: vibe-coding homework, interview guides, midterm poll

## Key Concepts Explained

### AI Agent & ReAct

- **What it is:** An LLM (reasoning/thought) combined with the ability to call **tools** (action). **ReAct** = **Re**asoning + **Act**ing.
- **Why it matters:** This is the building block of agentic workflows—models decide *what* to do; tools execute it.
- **Emphasized:** Used in prior LangChain/LangGraph projects via `create_react_agent`.

### Agentic AI

- **What it is:** Workflows that leverage one or more AI agents for automation—single-agent or **multi-agent** systems.
- **Why it matters:** Real business processes (hiring, onboarding, negotiation) are rarely linear; agents coordinate specialized steps.
- **Core components to memorize:** **Autonomy**, **goal-oriented**, **planning**, **reasoning**, **adaptivity**, **context awareness**.

### LangChain vs LangGraph

- **What it is:** Both from the same creators; **LangChain** targets **generative AI** (sequential/linear flows); **LangGraph** targets **agentic AI** (non-linear, graph-based decision flows).
- **Why it matters:** Interviewers often ask why you’d pick one over the other.
- **LangChain limitations (for agentic use):** control-flow complexity, cannot handle non-linearity well, no built-in **state** management.
- **LangGraph advantages:** automatic **state** tracking node-by-node; supports **human-in-the-loop** and branching (e.g., offer yes → onboarding vs no → negotiation).
- **Emphasized:** Do **not** memorize full API code for interviews—explain concepts and sketch a simple state graph.

### LangGraph State Graph

- **What it is:** A graph with **nodes** (`add_node`), **edges** (`add_edge`), and **conditional edges** for branching logic.
- **Why it matters:** Models multi-step agent workflows where state changes as execution moves between nodes.
- **Agent types in LangGraph:** **ReAct** and **supervisor-based** (supervisor project planned for next session).

### CrewAI Framework

- **What it is:** Open-source framework for orchestrating autonomous AI agents and building complex multi-agent workflows.
- **Why it matters:** Alternative to LangGraph for multi-agent orchestration; increasingly asked about in interviews.
- **Core metaphor:** A **crew** = a team of specialized **agents** working on defined **tasks** (like a flight crew or dance crew).
- **Emphasized:** Documentation is strong enough to scaffold a dummy project by following install steps alone. Instructor has project knowledge but **no production deployment experience** with CrewAI; recommends implementing the same project in both LangGraph and CrewAI to compare speed and ergonomics.

### CrewAI Agents

- **What it is:** Specialized workers, each tied to a role in the workflow.
- **Required fields (memorize):** **Role**, **Goal**, **Backstory** (persona/context).
- **Also pass:** `llm=llm` when defining in code.
- **Example roles from class:** researcher, writer, editor, publisher (blog workflow analogy).

### CrewAI Tasks

- **What it is:** A unit of work assigned to an agent.
- **Required fields (memorize):** **Description**, **Expected output**.
- **Why it matters:** Tasks decompose a high-level goal (e.g., “publish a blog”) into research → write → edit → publish.

### Agent Anatomy (General)

- **Brain:** the LLM.
- **Tools:** APIs, functions, etc.
- **Memory:** **Short-term** (within session/task) vs **long-term** (persistent goal/context).
- **Orchestrator:** the framework coordinating agents (LangGraph, CrewAI, etc.).
- **Supervisor (optional):** interprets user input and routes to the right agent/task/tool; can be a dedicated supervisor agent or human-in-the-loop.

### Chain of Thought (CoT)

- **What it is:** Prompting technique that breaks a goal into **sub-tasks** and solves them sequentially, storing intermediate results in short-term memory.
- **Example:** “Add 5 and 3, then multiply by 3” → Task 1: 5+3=8; Task 2: 8×3=24.
- **Why it matters:** Foundation for reasoning models and step-wise agent planning.

### CrewAI Execution Flow

- **Code order (define):** LLM → Agents → Tasks → Crew → `kickoff()`.
- **Runtime order (execute):** `kickoff()` → Crew → Tasks → Agents → LLM.
- **Emphasized:** Writing code top-down; execution runs bottom-up.

### CrewAI Project Structure (Template)

- **What it is:** Scaffold created by `crewai create crew <project_name>`.
- **Key folders/files:**
  - `knowledge/` — user preferences / knowledge base
  - `src/<project>/config/agents.yaml` — agent role, goal, backstory
  - `src/<project>/config/tasks.yaml` — task description, expected output
  - `src/<project>/tools/custom_tool.py` — custom tools via `BaseTool`
  - `src/<project>/crew.py` — crew orchestration
  - `main.py` — entry point; calls `crew.kickoff()`
  - `.env` — model name, API keys
  - `pyproject.toml`, `README.md`
- **Two ways to start:** from scratch (manual folders/files) or **template** then tweak (recommended by CrewAI).

### kickoff vs kickoff_async

- **What it is:** `crew.kickoff()` for normal runs; `crew.kickoff_async()` for async environments.
- **Why it matters:** Google Colab already runs an event loop—using `kickoff()` can fail with “event loop already running”; use `kickoff_async` in Colab.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| **CrewAI** | Multi-agent orchestration framework; `from crewai import Agent, Task, Crew, LLM` |
| **LangChain** | Sequential generative AI workflows; prior tool-calling projects |
| **LangGraph** | Agentic AI; ReAct + StateGraph; supervisor project tomorrow |
| **UV** | Python package/install tool; required before CrewAI install |
| **Ollama** | Local LLM provider used in template project (`llama3.1`, `tinyllama`) |
| **Groq** | API provider used in Colab demo |
| **LiteLLM** | Missing dependency during local run; install via `uv add litellm` |
| **FastAPI** | Missing dependency during local run |
| **APScheduler** | Another missing dependency encountered |
| **Google Colab** | Notebook-based CrewAI from-scratch demo (`crewai.ipynb`) |
| **`.env`** | Stores model name and API keys from `crewai create crew` wizard |
| **`agents.yaml` / `tasks.yaml`** | Declarative agent and task definitions in template projects |
| **`crewai create crew <name>`** | Scaffolds a new CrewAI project |
| **`crewai install`** | Installs project dependencies (run inside project folder) |
| **`crewai run`** | Executes the crew from template project |
| **`uv add <package>`** | Add missing packages to UV-managed project |
| **`ollama list`** | Check locally available Ollama models |
| **n8n** | No/low-code workflow automation planned for week 12 |
| **Pydantic / structured parsing** | Mentioned as 2026-era improvement for JSON output |
| **Generative AI Interview Guide** | Shared to AGI 3 Drive folder |
| **Agentic AI Interview Guide** | Instructor preparing; coming in a few days |

## Code & Examples

### CrewAI workflow (conceptual)

```python
from crewai import LLM, Agent, Task, Crew

llm = LLM(model="...", temperature=0.7)  # + api_key if needed

researcher = Agent(
    role="Senior Data Researcher",
    goal="Find relevant information on the topic",
    backstory="Expert researcher with deep domain knowledge...",
    llm=llm,
)

research_task = Task(
    description="Conduct thorough research about {topic} for year {current_year}",
    expected_output="A detailed research summary with key findings",
    agent=researcher,
)

crew = Crew(agents=[researcher], tasks=[research_task])
result = crew.kickoff(inputs={"topic": "AI LLMs", "current_year": 2026})
```

### Template project entry point (approximate)

```python
# main.py
result = report_summarization.crew().kickoff(
    inputs={"topic": "AI LLMs", "current_year": datetime.now().year}
)
```

### Colab email-writer agent (from session)

```python
from crewai import Agent, Task, Crew, LLM

llm = LLM(model="...", api_key="...")

email_writer = Agent(
    role="Professional Email Writer",
    goal="Write clear, polite, and perfectly toned professional emails",
    backstory=(
        "Executive communications expert with 12+ years helping CEOs "
        "write high-response emails."
    ),
    llm=llm,
)

write_email_task = Task(
    description=(
        "Write a professional email based on: recipient, purpose, "
        "key points, tone..."
    ),
    expected_output="Complete email ready to copy/paste",
    agent=email_writer,
)

crew = Crew(agents=[email_writer], tasks=[write_email_task])
result = await crew.kickoff_async(inputs={...})  # Colab: use async
```

### LangGraph state graph (interview sketch)

```python
# Conceptual: nodes for create_jd → post_jd → candidates
# Edges connect sequential steps; conditional edge branches on yes/no
graph.add_node("create_jd", ...)
graph.add_node("post_jd", ...)
graph.add_node("candidates", ...)
graph.add_edge("start", "create_jd")
graph.add_edge("create_jd", "post_jd")
graph.add_conditional_edges("candidates", router_fn, {"yes": "end", "no": "continue"})
```

### Local setup steps

```bash
# 1. Verify Python 3.10–3.13
python --version

# 2. Install UV (per CrewAI docs)
# 3. Install CrewAI CLI
# 4. Scaffold project
crewai create crew report_summarization
# Select provider (OpenAI, Anthropic, Ollama, etc.)

# 5. Inside project folder
crewai install
crewai run

# Fix missing deps as needed
uv add litellm
uv add fastapi
```

### Blog-crew customization (instructor suggestion)

Replace default `agents.yaml` agents (researcher, reporting analyst) with four agents: **researcher**, **writer**, **editor**, **publisher**—each with matching tasks in `tasks.yaml`.

## Q&A / Discussion Points

- **Do I need to memorize LangGraph/LangChain code for interviews?** No—explain purpose, LangChain vs LangGraph trade-offs, state graph concepts (nodes/edges/conditional edges), and sketch architecture.
- **LangChain vs LangGraph?** LangChain = linear generative AI; LangGraph = non-linear agentic AI with state and branching.
- **What is “state” in LangGraph?** Shared data updated as execution moves node-to-node (e.g., offer/onboarding/negotiation fields).
- **CrewAI web app / drag-and-drop UI?** Instructor unsure; may exist but not verified.
- **Can CrewAI run without the template folder structure?** Yes—all-in-one script is possible, but CrewAI recommends the scaffold.
- **Jupyter/Colab vs IDE for CrewAI?** Different workflow; Colab demo shows scratch coding without template.
- **Why FastAPI needed for CrewAI?** Hit as missing dependency during logging/runtime (exact reason not deeply explained).
- **Report output location?** Generated in the project folder (not a separate manual save step).
- **CrewAI vs LangGraph—which is better?** Subjective; try both on the same project and compare implementation time and execution time. Instructor has more LangGraph deployment experience.
- **Shared files from last week?** Google Drive bookmark shared; files also going to SharePoint.
- **XAI not working for a student?** Fall back to Ollama or another API provider.

## Action Items & Assignments

- [ ] Bookmark the instructor’s **Google Drive** (AGI 3 folder) for class materials.
- [ ] Complete local **report summarization** crew: `crewai create crew`, configure `.env`, `crewai install`, `crewai run`; fix any missing deps (`litellm`, `fastapi`, etc.).
- [ ] **Vibe-code tonight:** adapt the report summarization project into a **blog-writing crew** (researcher, writer, editor, publisher) and report back tomorrow.
- [ ] Run the shared **`crewai.ipynb`** on Google Colab (email writer example).
- [ ] Start thinking about **portfolio project** from Monday (agentic AI capstone/assignment expected by end of week).
- [ ] Complete **midterm satisfaction poll** and end-of-class feedback poll.
- [ ] Review **Generative AI Interview Guide** in Drive; watch for upcoming **Agentic AI Interview Guide**.
- [ ] Attend **June 14** session: end-to-end LangGraph + CrewAI industry projects.
- [ ] Optional: compare same use case in **LangGraph vs CrewAI** to form your own preference.

## Open Questions / Unclear Spots

- Instructor was approximate on evolution timeline years (2017 paper, 2018 GPTs, 2022 ChatGPT)—cross-check dates independently.
- Whether CrewAI has a production **web/drag-and-drop UI** was left unresolved.
- Exact spec/deadline for the upcoming **agentic AI assignment/capstone** not finalized (“by end of this week”).
- **Supervisor-based LangGraph project** use case still being chosen for tomorrow.
- Some students hit provider-specific issues (XAI, Ollama slowness); instructor’s Ollama run was slow/high CPU vs API users who finished faster.
- [unclear: “Movie add FastAPI”] — likely ASR error for **`uv add fastapi`**.

## Flashcards

**Q: What are the two core components of a CrewAI crew?**  
A: **Agents** and **Tasks**.

**Q: What three fields define a CrewAI agent?**  
A: **Role**, **Goal**, and **Backstory**.

**Q: What two fields define a CrewAI task?**  
A: **Description** and **Expected output**.

**Q: What is the CrewAI code/setup order before execution?**  
A: Define **LLM** → **Agents** → **Tasks** → **Crew** → call **`kickoff()`** (runtime order is reversed).

**Q: Why use LangGraph instead of LangChain for agentic AI?**  
A: Agentic flows are **non-linear**, need **state** management and conditional branching—LangGraph handles this; LangChain is better for **sequential** generative AI.

**Q: What three building blocks make up a LangGraph state graph?**  
A: **Nodes**, **edges**, and **conditional edges**.

**Q: What does ReAct stand for?**  
A: **Reasoning** + **Acting** (LLM thinks, tools act).

**Q: Name six characteristics of agentic AI from class.**  
A: **Autonomy**, goal-oriented, **planning**, **reasoning**, **adaptivity**, **context awareness**.

**Q: When should you use `kickoff_async` instead of `kickoff`?**  
A: In **Google Colab** (or other async environments) where an event loop is already running.

**Q: What Python versions does CrewAI support?**  
A: **3.10 through 3.13**.
