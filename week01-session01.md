# Module 01 Kickoff: Bootcamp Overview & Python Basics (Week 1, Session 1)

## TL;DR
This session introduced the **Building Agentic AI Applications for Beginners** bootcamp (trainer Satyajit), covered market context (generative AI → agentic AI in 2026), the 12-week curriculum path, logistics (Discord, GitHub, assignments, recordings), and began **Week 1 Python** on Google Colab: variables, keywords, data types, operators, and operator-precedence exercises. The instructor emphasized attending live classes, using doubt-clearing Thursdays for extra topics, and practicing after each session with AI-assisted prompts (“vibe coding”).

## Topics Covered
- Zoom housekeeping (rename participants, chat introductions)
- Student intros and shared goals (agentic AI, hands-on projects, career upskilling)
- AI industry timeline: traditional ML/DL/NLP → ChatGPT (2023) → GenAI POCs (2024) → deployments (2025) → agentic AI focus (2026)
- Bootcamp goals, duration (~13 weeks, target completion by end of June with buffer), and commitment expectations
- Full curriculum arc: Python → insight gathering → traditional ML/DL → NLP → transformers → generative AI → agentic AI (LangGraph, CrewAI) → no-code agentic (n8n)
- Instructor background and credibility (industry + teaching)
- Q&A: LLM/tool choices, job readiness, vibe coding, projects, GitHub, hardware, Streamlit/web apps, MCP, recordings
- Discord channels, Codecademy Pro, resources (Google Drive temp folder, SharePoint later, dashboard recordings)
- Short break, then Python readiness poll (0–5)
- Python install guidance (3.12 recommended) vs **Google Colab** as primary classroom environment
- Python theory: popularity vs R, language traits, variables, keywords, data types (mutable vs immutable), operators
- Colab hands-on: `print`, `type`, arithmetic, `//` floor division, `%` modulo
- End-of-class operator-precedence problems (PEMDAS/BODMAS)
- Post-class practice via AI prompt (vibe coding)

## Key Concepts Explained

### Traditional AI vs generative AI vs agentic AI
- **What it is:** Before 2023, industry work was mostly ML, deep learning, NLP, transformers, and computer vision. Post–ChatGPT viral moment, demand shifted toward generative AI; by 2026 many clients prioritize **agentic AI** workflows over standalone GenAI.
- **Why it matters:** Positions the bootcamp’s focus and explains why students are learning coding agents (LangGraph, CrewAI), not only no-code tools.
- **Instructor emphasis:** Stay current with the pace of AI; generative-only use cases are declining in consulting engagements he sees in APAC (excluding India).

### Bootcamp learning path (12 + 1 weeks)
- **What it is:** Week-by-week progression from Python fundamentals through ML/DL, NLP/transformers, generative AI (roughly weeks 6–8), then agentic AI (weeks 9–12), plus no-code automation with n8n; doubt-clearing on **Thursdays**.
- **Why it matters:** Sets expectations for depth before agentic modules and explains why Python basics are mandatory even for managers.
- **Caveats:** Dates on slides may be outdated; occasional break weeks possible; missing multiple live sessions correlates with falling behind.

### Vibe coding
- **What it is:** Using AI assistants to generate, adapt, and debug code so you implement and validate rather than write everything from scratch.
- **Why it matters:** Lowers the barrier for beginners (especially self-rated 0–2 on Python) while still building real projects.
- **Instructor emphasis:** Much of the bootcamp programming will use vibe coding; after each class, give the AI **session context** before asking for practice questions.

### Hallucinations and GenAI evaluation
- **What it is:** LLM outputs can be factual or misleading (“hallucinations”); production GenAI requires evaluation and model selection, not blind trust.
- **Why it matters:** Addresses concern that implementing GenAI is harder than learning it; evaluation is a core implementation concern.
- **Instructor emphasis:** Fact-check technical answers (analogy: ChatGPT script vs technical Q&A).

### Assignments, capstone, and certificate
- **What it is:** Optional assignments after ML/AI and GenAI modules plus a final capstone; deadlines exist for **instructor review**; certificate auto-available on Codecademy dashboard after program end regardless of submission.
- **Why it matters:** Review is incentive-driven; capstone/own project builds portfolio depth.
- **Caveats:** Extensions only for genuine emergencies; late work may not be reviewed.

### Google Colab vs local Python
- **What it is:** Colab is the in-class execution environment (`.ipynb` notebooks); local install (`.py`) is encouraged but not required immediately.
- **Why it matters:** Everyone can follow live without setup blockers; local Python helps later and for larger projects.
- **Recommendation:** Python **3.12** for stability; avoid bleeding-edge 3.15/3.16 for class work.

### Python variables
- **What it is:** Named storage in memory; no explicit type declaration; rebinding overwrites prior value (e.g., `a = 10` then `a = "hello"`).
- **Why it matters:** Foundation for all later data/ML/agent code.
- **Instructor emphasis:** Use `print()` to see values; execution is **line by line**.

### Python keywords
- **What it is:** Reserved words (`def`, `print`, `return`, `class`, `if`, `import`, etc.) built into the language; should not be reused as variable names.
- **Why it matters:** Prevents subtle bugs and syntax errors.

### Data types vs data structures
- **What it is:** Core types include `int`, `float`, `str`, `datetime`; **mutable** types include list, dict, set; **immutable** include numbers, strings, tuples. Lists/tuples/dicts/sets are **data structures**, covered later.
- **Why it matters:** Informs when values can be changed in place.
- **Caveat:** Instructor briefly mixed mutable/immutable book analogy during live explanation—verify with practice.

### Operators and precedence
- **What it is:** `+`, `-`, `*`, `/`, `**` (exponentiation), `//` (floor division), `%` (modulo/remainder); precedence follows **PEMDAS/PEDMAS** or **BODMAS**.
- **Why it matters:** Correct pen-and-paper traces before running cells; common interview-style exercises.
- **Instructor note:** He double-checked `//` vs `%` live—floor division drops fractional part toward floor (e.g., `10 // 3` → `3`; `10 % 3` → `1`).

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Codecademy | Host platform; Pro access included; dashboard for recordings and certificate |
| Discord (AGAI cohort) | Doubts, general chat, project showcase, resources; tag Nazia/Aparna (Codecademy), Sat for attention |
| Zoom | Live classes; Q&A in last ~15 min; rename participants |
| Python 3.12 | Recommended local version (13/14 OK; avoid 15/16 for now) |
| Google Colab | Primary teaching environment; new notebook “Week One Basics of Python” |
| GitHub | Required for all project code submissions; create account early |
| LangChain, LangGraph, CrewAI | Core agentic stack later in curriculum |
| OpenAI APIs | Mentioned in curriculum overview |
| Vector databases | Planned topic |
| n8n | No-code agentic automation (week 12 area) |
| Groq / Ollama / Llama | Primary LLM/API direction for course labs [transcript also mentions Grok/xAI and Poe for assistant use] |
| AWS Bedrock | Mentioned as introduction on the go |
| minimax agent | Example tool for explaining code snippets |
| Streamlit / Flask | Python apps for demos; not full JS frontend bootcamp |
| MCP | Not in official curriculum; possible Thursday doubt-clearing around agentic weeks |
| Poe.com, grok.com | Instructor’s vibe-coding assistants (ChatGPT banned in Hong Kong) |
| NumPy, Pandas, Matplotlib, Seaborn | Coming in Python/data weeks |
| VS Code, Spyder, PyCharm | IDE choices for larger projects; instructor prefers Spyder |
| R language | Historical data-science context before Python dominance |
| JavaScript | #1 language globally per instructor; not front-end focus here |

## Code & Examples

### Google Colab workflow
1. Log in with Gmail → open Colab → **New Notebook**.
2. Rename notebook (e.g., `Week One Basics of Python`).
3. Run cells: **Shift+Enter** runs and advances; Run button executes only current cell.

### Variables and print
```python
a = 10
print(a)      # 10

a = 20
print(a)      # 20

b = "hello"
print(b)      # hello
```

### Dynamic typing and `type()`
```python
a = 20
print(type(a))    # <class 'int'>

a = "20"
print(type(a))    # <class 'str'>

a = 20.0
print(type(a))    # <class 'float'>
```

### Basic operators
```python
a = 5
b = 3
print(a + b)   # 8
print(a - b)   # 2
print(a * b)   # 15
print(a / b)   # 1.666...

a = 10
b = 3
print(a / b)    # 3.333...
print(a // b)   # 3  (floor division)
print(a % b)    # 1  (remainder)
print(5 ** 3)   # 125
```

### End-of-class pen-and-paper problems (solve with precedence; verify in Python after)
```python
# Problem 1 (answer discussed: -40)
24 - 4 * 2 + 2 % 2 - (10 % 3 - 58 // 11 + 60)

# Problem 2 (answer discussed: 68)
print(4 + 8**2 - 3**2 % 1)
```

### Post-class AI practice prompt (paraphrased from instructor)
```
Today was my first class on Python. We studied keywords, variables, data types,
and operators (+, -, *, /, **, //, %).
I want to practice more on these basic topics.
Can you please generate a list of 10 questions to practice?
```
Include what you studied each day so questions stay at the right level.

## Q&A / Discussion Points

| Question | Answer / clarification |
|----------|-------------------------|
| Will you cover Claude, ChatGPT Codex, alternatives to n8n? | LLM-agnostic mindset; course labs lean on Groq, Ollama/Llama; vibe coding can use Anthropic/OpenAI/Poe; extra tools (e.g., Bedrock, minimax) introduced ad hoc; **Thursday doubt-clearing** for off-curriculum tools |
| Job readiness and industry expectations? | Honest: no hype; roles expect **both** coding and no-code; GenAI + agentic skills; architects expected at 7–8+ years; stay technical to reduce layoff risk |
| Strong programming emphasis? | Yes, but much via vibe coding |
| What is vibe coding? | AI writes/debugs; you integrate and validate |
| World models in course? | Curriculum reflects current industry build topics; instructor still active in market |
| Own projects allowed? | Yes; flexible domain (healthcare, banking, insurance, etc.) with polls |
| Where to submit assignments? | Code on **GitHub** (deployment = repo); not a separate LMS upload described |
| Contact co-learners? | Discord |
| Python fundamentals course on Codecademy too? | Optional; bootcamp covers Python from scratch |
| Hardware requirements? | Strong GPU only if running open-source models locally; API-based work needs modest machine |
| Real-world web apps? | Streamlit (and Flask flavor), not JS frontend track |
| MCP coverage? | Not official; possible doubt-clearing ~week 8 |
| Recording access? | Dashboard “View Recording”; instructor believes long-term access—confirm with Nazia/Aparna |
| VS Code for big projects? | Fine; instructor uses Spyder personally |

## Action Items & Assignments

- [ ] Rename yourself in Zoom each session (or message instructor if blocked)
- [ ] Join Discord; fix access via Codecademy email or contact Nazia/Aparna / Sat on LinkedIn
- [ ] Post intro in chat format: name, location, why joined, one excited-to-learn item
- [ ] Create **GitHub** account; plan to push all projects
- [ ] Bookmark temporary **Google Drive** folder shared in class; await SharePoint resources from team
- [ ] Explore **Codecademy Pro** courses if access missing—contact support
- [ ] Self-rate Python 0–5; if 0–2, push extra practice weeks 1–2
- [ ] Install Python **3.12** (optional now; Colab sufficient for class)
- [ ] Open **Google Colab**; create `Week One Basics of Python` notebook
- [ ] After class: run vibe-coding prompt for **10 practice questions** with today’s topics
- [ ] Solve operator-precedence homework on paper, then verify in Colab
- [ ] Watch recording if lost mid-session (noted for some students)
- [ ] Complete session **poll**; negative feedback → Nazia/Aparna (anonymous to instructor)
- [ ] Connect with Satyajit on **LinkedIn**; optional YouTube playlist pre-read (not mandatory)
- [ ] Attend live sessions; if absent, watch recordings quickly
- [ ] Prepare for next class: **data structures** in Python

## Open Questions / Unclear Spots

- Exact **assignment submission** workflow beyond “push to GitHub” (grading rubric, links) not fully specified in this session.
- **Capstone project list** may be updated; healthcare RAG chatbot mentioned as likely GenAI group project; mini “Alexa” FX/weather multi-agent for agentic intro.
- Transcript mixes **Groq** (inference API) and **Grok** (xAI); instructor stated “Groq by xAI” which is likely ASR/conflation—use course materials when labs start.
- **“Insight gathering”** week label after Python—exact topics not detailed in this session.
- Recording retention “forever” needs confirmation with Codecademy staff.
- Student **Chip960** identity unresolved in session (rename request repeated).
