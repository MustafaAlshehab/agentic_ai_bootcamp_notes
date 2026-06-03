# Break Week Plan, Assignment Submission, and Intro to Agentic AI

## TL;DR
This doubt-clearing session focused on logistics for two planned break weeks (no live classes April 3–4 and April 11–12; return April 18), curated Python/NumPy/Pandas/Seaborn assignments, and how to submit work via GitHub and Discord. The instructor also previewed break-week study paths (EDA video and projects) and gave a conceptual overview of generative AI vs. AI agents vs. multi-agent (agentic AI) workflows, including architecture, tooling (e.g., n8n), and LLM/token basics.

## Topics Covered
- Instructor context: break weeks, travel, and mixed skill levels in the cohort
- What to do during break weeks (beginner vs. intermediate/advanced paths)
- Where assignments and links will be posted (Discord; Codecademy team announcement)
- GitHub-based submission workflow (step-by-step demo)
- Assignment preview and expectations (avoid AI for core Python; Seaborn/Matplotlib AI use OK if stuck)
- Schedule: no classes this weekend; doubt-clearing vs. regular class cadence
- Generative AI vs. AI agents vs. agentic AI / multi-agent workflows
- EDA as optional parallel skill-building (not core to agentic AI curriculum)
- Companies that own LLMs and example models per company
- Multi-agent framework examples (dental appointment, hiring pipeline)
- Supervisor + single-LLM architecture; role of n8n
- Tokens, billing, open-source deployment, and context windows
- Brief pointers on OpenClau/NanoClau, web scraping, and advanced topics coming later

## Key Concepts Explained

### Break weeks and study plan
- Two break weeks are planned: no classes **April 3–4** and **April 11–12**; classes resume **April 18**. Doubt-clearing sessions in that window are also canceled.
- **Beginners:** practice curated Python assignments, then ping instructor on Discord when done for next steps.
- **Intermediate/advanced:** skip basic Python drills if confident; do assignments, then EDA video and 1–2 EDA projects; optionally request ML/DL or generative/agentic prep material via Discord or LinkedIn.
- Instructor will share assignment packs after this session; more exercises (including MCQs) may be added.

### Exploratory Data Analysis (EDA)
- EDA = exploratory data analysis; traditional data-analytics topic, **not** part of the agentic AI track but useful after Pandas/NumPy.
- Recommended YouTube resource: search **"Satyajit EDA"** (~5–6 hour video); instructor says it explains next steps for self-directed EDA projects.
- EDA projects are encouraged for hands-on practice during the break; not a substitute for formal agentic-AI modules later (NLP, transformers, etc.).

### Generative AI
- Systems/platforms (e.g., ChatGPT, Grok, Claude) that **generate** text, images, or video via underlying **large language models** (GPT, Grok, Claude families, etc.).
- Example: itinerary planning Delhi → Ho Chi Minh is fine; **booking tickets** is beyond typical generative-only behavior without action/tool use.

### AI agent
- An **LLM plus tools** (APIs, actions): the model interprets intent and can **act**—e.g., call Booking.com, Skyscanner, or Klook for hotel booking.
- One agent ≈ one LLM brain wired to one or more tools for a task.

### Agentic AI / multi-agent workflow
- **Multiple agents** coordinating = multi-agent architecture or **agentic AI workflow** (related terms; scale differs from a single agent).
- Future curriculum: **LangGraph**, **CrewAI**, evaluation of LLMs, **n8n** class, practical doubt sessions.
- Instructor emphasized: course is **about agentic flows**; deeper coverage comes in later weeks.

### Multi-agent architecture (supervisor pattern)
- Typical pattern: **one LLM** for the workflow, not one model per agent by default.
- **Supervisor agent** attached to the LLM: classifies intent, routes subtasks to **Agent 1…N**, each with its own tools; tool outputs return to the LLM for final response.
- Optional complexity (different LLMs per agent) is possible but **not recommended** as the default approach.
- Examples: dental clinic booking (booking, cancel, reschedule, doctor info tools); hiring pipeline (draft JD → review → post → collect applications → schedule interviews → offer).

### n8n
- **Low-code/no-code** workflow tool: drag-and-drop nodes to build agent-like flows without Python.
- Trade-off: organizations often resist broad integrations (mail, SharePoint, etc.) due to **security, guardrails, and data privacy**.
- **14-day trial**; instructor noted using multiple accounts for learning and pointed to his **n8n AI news summarizer** project video.

### LLM providers vs. models
- Distinction: **companies** that own/develop LLMs vs. **product names** (e.g., Microsoft Copilot uses **OpenAI** models via partnership; **Amazon Bedrock** aggregates models including **Nova**).
- Class brainstorm included OpenAI, Anthropic, Google, DeepSeek, xAI, Alibaba, Meta, Amazon, Mistral, Zephyr, etc.

### Tokens and context window
- **Paid APIs** (e.g., GPT): input/output tokens tied to **cost**, whether cloud or self-hosted if still calling the API.
- **Open-source models** (e.g., Llama): tokens still matter for **context window** limits; no per-token API cash if self-hosted, but **hardware/server cost** for large weights (e.g., 8 GB–80 GB).
- **Context window** is model-specific (e.g., TinyLlama ~2048); long chats on small/free tiers cause **forgetting** prior context—not the same as hallucination, but similar user experience.
- Tip: shorter sessions and new chats on free tiers; prompting skills can reduce need for paid accounts.
- Production handling when context fills (restart/workarounds) to be covered later in the program.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Discord | Primary channel for assignments, links, and submission posts (#project showcase, tag instructor) |
| GitHub | Create account/repo; upload **answers** notebook (not questions); share link on Discord |
| Python | Break-week assignments: dicts, `max`/`min` with `key`, etc. |
| NumPy | Exercise sets in assignment pack |
| Pandas | `df.head()` and related exercises; data file provided with assignments |
| Matplotlib / Seaborn | Plotting (e.g., churn counts); docs/AI help OK if stuck on plot code |
| ChatGPT / Groq | Mentioned for plotting help; instructor uses free Groq without paid account |
| YouTube (Satyajit EDA) | ~5–6 hr EDA video; search by instructor name + EDA |
| LangGraph, CrewAI | Named as future multi-agent frameworks |
| n8n | No-code workflows; trial; security concerns in enterprises |
| Booking.com, Skyscanner, Klook | Example travel-booking APIs in agent scenario |
| OpenAI (GPT), Anthropic (Claude Sonnet), Google (Gemini/Bard), DeepSeek, xAI (Grok), Alibaba (Qwen), Meta (Llama), Amazon Bedrock (Nova), Mistral, Zephyr | LLM landscape discussion |
| LinkedIn | Fallback contact if Discord unavailable |
| [unclear: OpenClau / NanoClau] | Compared to multi-agent setup; recently launched; not in core curriculum; may cover late in course |

## Code & Examples

### Assignment Q1-style: Netflix views dictionary
Inspect dictionary, find a title (e.g., Indian film), return view count; document answer in notebook.

```python
# Example pattern from demo (titles/keys from provided dict)
views = netflix["Some Movie Title"]
# Answer recorded in markdown, e.g. "Answer is D" or numeric result
```

### Assignment Q2: most / least popular show
Use `max` and `min` with `key` on dictionary values, or inspect manually.

```python
most_popular = max(netflix, key=netflix.get)
least_popular = min(netflix, key=netflix.get)
```

### Pandas / Seaborn exercise (approximate)
Dataset file shipped with assignments; place file so paths resolve.

```python
import pandas as pd
import seaborn as sns

df = pd.read_csv("provided_dataset.csv")  # filename from assignment pack
df.head()

# Example task: plot relationship between churn and count of churn
# Instructor: consult Seaborn docs; scatter between specified columns
sns.scatterplot(data=df, x="column_a", y="column_b")  # exact columns from notebook
```

### GitHub submission workflow (procedure)
1. Create GitHub account and new repository (e.g., `python-assignments`).
2. Complete assignment notebook: code + answers; remove empty/template cells as needed.
3. Download/save notebook as answers version; **upload answers notebook only** (not question template).
4. Commit via GitHub web UI (Add file → Upload).
5. Post in Discord **project showcase**, tag instructor: link to repo + short message.

## Q&A / Discussion Points

| Topic | Summary |
|-------|---------|
| Rubric / side panel links (Ryan) | Instructor will post EDA video and helpful course links on Discord. |
| Where homework is posted (AJ, others) | Discord; Codecademy team (Nazia/Aparna) to announce; GitHub submission process demonstrated. |
| Small projects alongside assignments (Monica) | Do EDA video + 1–2 EDA projects; can also ask AI for next projects given completed work context. |
| Wednesday classes | **Doubt-clearing**, not regular lectures; prepare questions; some may become practical catch-up sessions. |
| Separate LLM per agent? | **No** by default—one LLM, supervisor routes to agents/tools. |
| Different LLMs per agent for cost/quality? | Possible but unnecessarily complex; not the recommended pattern. |
| n8n vs. Python architecture | n8n can mirror flows; enterprises often avoid due to access/security; class planned. |
| Free n8n alternatives | n8n has trial; multi-email workaround mentioned; instructor’s n8n tutorial project referenced. |
| On-prem deployment and tokens (Vicky/Balaram) | API usage still token-based and billed for commercial models; self-hosted open-source avoids API token **fees** but not context limits; server sizing matters. |
| Context window vs. hallucination | Forgetting from full context window explained; not equated strictly with hallucination. |
| Global deployment when context fills | Restart/workarounds for production to be covered later. |
| OpenClau / NanoClau vs. multi-agent | Similar in spirit; not in curriculum yet; possible end-of-course coverage. |
| Web scraper project | No dedicated scraping project; instructor may do related practical in a future doubt session if reminded. |
| Negative session feedback | Instructor acknowledged doubt sessions should still be useful. |

## Action Items & Assignments

- [ ] Complete curated **Python** assignments (shared on Discord after class).
- [ ] Complete **NumPy**, **Pandas**, and plotting (**Matplotlib/Seaborn**) exercises in the pack; use provided data files.
- [ ] Avoid using **AI** for core Python/NumPy/Pandas logic; use AI only if stuck on Seaborn/Matplotlib plotting.
- [ ] Watch **Satyajit EDA** YouTube video (~5–6 hours) after Python assignments.
- [ ] Complete **1–2 EDA projects** for self-practice (intermediate learners especially).
- [ ] Submit work: GitHub repo with answers notebook → link in Discord **project showcase** (tag instructor).
- [ ] When finished early, message instructor on **Discord** (or **LinkedIn**) for advanced ML/DL or generative/agentic prep resources.
- [ ] Watch for Codecademy team announcement of full exercise bundle (more MCQs coming).
- [ ] No live classes until **April 18**; use break for practice only.

## Open Questions / Unclear Spots

- [unclear: **OpenClau / NanoClau**] — Transcript names tools compared to multi-agent setups; exact products not spelled clearly; deferred to late course.
- [unclear: **Zephyr / Mistral parent companies**] — Instructor unsure of corporate entity names vs. model brands.
- [unclear: **Microsoft-owned LLM**] — Instructor believed Microsoft does not own an LLM (uses OpenAI); offered to verify.
- Exact **assignment file names**, **dataset CSV name**, and **Seaborn column names** for churn plot were not fully specified in the demo portion of the transcript.
- Whether **web scraping** practical will be scheduled depends on a future reminder in doubt class.
