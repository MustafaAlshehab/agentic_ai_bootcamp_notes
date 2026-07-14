# Module 03 — Thursday Doubt-Clearing: APIs, Portfolio, Assignment Walkthrough

## TL;DR
This was a low-attendance doubt-clearing session focused on student Q&A (FastAPI vs Streamlit, third-party APIs, TensorFlow vs PyTorch, portfolio scope) and a partial walkthrough of graded Pandas/NumPy assignment questions (Netflix dictionary, cricket player scores). The instructor will share full assignment answers before Saturday’s class; optional ML/DL courses are self-paced and not prerequisites for the bootcamp.

## Topics Covered
- How Thursday doubt-clearing sessions work (questions via Discord / live Q&A)
- FastAPI vs Flask vs Streamlit for APIs
- Third-party / paid vs free APIs in agentic AI
- Pandas: DataFrames vs Series (assignment context)
- TensorFlow vs PyTorch for deep learning
- Bootcamp mandatory vs recommended portfolio projects
- When and how to take supplemental ML/DL courses
- Graded assignment review: Netflix dictionary (Pandas), cricket scores (NumPy)
- Python skill level and “vibe coding” for gen AI / agentic projects
- RAG chatbot safety from PDF documents
- Upcoming end-to-end projects (dashboard, agentic APIs, YouTube automation, PDF-to-course)

## Key Concepts Explained

### Thursday vs weekend classes
- **What it is:** Thursday sessions are doubt-clearing only; Saturday/Sunday classes follow a structured agenda.
- **Why it matters / when to use it:** Students should post doubts on the Discord doubt channel and bring questions to Thursday class; attending without questions is discouraged.
- **Caveats:** Instructor emphasized posting doubts on Discord for those who skip live doubt sessions.

### API frameworks (FastAPI, Flask, Streamlit)
- **What it is:** FastAPI and Flask are common ways to expose Python apps as APIs; Streamlit is another path to deploy/interact with applications.
- **Why it matters / when to use it:** Industry often uses FastAPI, but this bootcamp focuses on **Streamlit** with live coding from scratch (not copy-paste between machines).
- **Caveats:** FastAPI is “good to have,” not mandatory; students interested in FastAPI should self-study; instructor can point to blogs/articles.

### Third-party APIs (agentic AI)
- **What it is:** Agent workflows may call external APIs (weather, foreign exchange, translation, etc.).
- **Why it matters / when to use it:** Practice on **free APIs** where possible; production apps typically use **paid** cloud services (Azure, AWS, GCP) because free tiers have rate limits (e.g., Google Translate: ~5 concurrent requests per minute).
- **Caveats:** Connecting to one cloud service pattern largely repeats for others; free APIs are fine for learning, not for industrial-scale apps.

### Pandas DataFrames vs Series
- **What it is:** Different Pandas constructors/methods yield DataFrames or Series.
- **Why it matters / when to use it:** For this bootcamp stage, **focus on DataFrames**; Series depth is not required now.

### TensorFlow vs PyTorch
- **What it is:** Two deep-learning frameworks; pick **one**, not both.
- **Why it matters / when to use it:** Either can build DL apps; instructor’s background is TensorFlow (and Keras as a wrapper); he has not used PyTorch in projects but still works in the field.
- **Caveats:** No need to master both; choice is personal preference.

### Bootcamp portfolio (mandatory gen AI / agentic)
- **What it is:** Five required projects: (1) generative AI **RAG**, (2) generative AI **fine-tuning**, (3) agentic AI with **LangGraph**, (4) agentic AI with **CrewAI**, (5) agentic AI with **n8n** (bootcamp focuses on n8n, not LangFlow).
- **Why it matters / when to use it:** Instructor stated these five are **enough for interviews/jobs** in this track.
- **Caveats:** Tailor resume/projects to each job description rather than one generic resume (unlike ~5–6 years ago).

### Supplemental ML/DL/NLP (optional)
- **What it is:** Extra projects: one **ML classification** end-to-end (skip regression); one **DL image classification**; one **segmentation or object-detection**-style project; traditional **BERT**-style NLP projects are **not** needed for modern work but can help with odd interview questions.
- **Why it matters / when to use it:** Deep learning is **not** a prerequisite for generative AI, but helps you follow conversations if colleagues discuss DL; some employers want gen AI + DL.
- **Caveats:** ML/DL course access (Udemy/instructor platform) will be shared for **self-study**; doubts can be asked in doubt-clearing; not required to complete before bootcamp content, but recommended **before bootcamp ends**.

### Dictionary lookup and aggregations (assignment)
- **What it is:** Netflix-style `dict` of title → views; lookup by key; `max()` / `min()` on values (or manual comparison for multi-option questions).
- **Why it matters / when to use it:** Core Python/Pandas prep for data manipulation exercises.
- **Caveats:** Some questions require comparing four combinations manually (no shortcut beyond checking each key).

### Merging dictionaries (weekly views)
- **What it is:** Add last-week view counts to an existing totals dict, then find the new most popular title and its view count.
- **Why it matters / when to use it:** Models updating cumulative metrics from new data.
- **Caveats:** Instructor’s solution pointed to title **Delhi Belly** for the stated exercise.

### NumPy nested structures (cricket scores)
- **What it is:** `player` list (IDs) and `score` list of tuples `(runs, wickets, catches)`; array dimension 2 when treating nested tuples in a list; questions count zeros in runs, max wickets (2nd tuple element), etc.
- **Why it matters / when to use it:** Practice `ndim`, indexing tuple positions, and `np.max`-style operations on structured lists.
- **Caveats:** **Domain knowledge** (cricket) helps interpret “innings,” runs, wickets, catches; ~1,502 players/records in the exercise data.

### Vibe coding and Python level
- **What it is:** Building apps with AI assistance rather than writing everything from scratch.
- **Why it matters / when to use it:** Industry focus is end-to-end delivery; bootcamp Python so far is sufficient; comfort with coding improves with practice.
- **Caveats:** Extra Python practice recommended if you read code but struggle to write it.

### RAG / chatbot security (PDF)
- **What it is:** Safety considerations for chatbots over PDF content (RAG or fine-tuning).
- **Why it matters / when to use it:** **Open-source models** are preferred when safety is critical; **API** calls send data to unknown servers—vendors may claim no training on your data, but risk remains; **guardrails** matter for personal data (to be taught later).
- **Caveats:** Companies are cautious deploying customer-facing chatbots with external LLM APIs.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| FastAPI | Common in industry; not taught in bootcamp; self-study |
| Flask | Alternative API framework |
| Streamlit | Bootcamp focus; live-built apps |
| Discord (doubt channel) | Post doubts if not attending Thursday class |
| Pandas | DataFrames over Series for now; instructor YouTube ~30–40 min |
| NumPy | Assignment on nested lists/tuples; instructor YouTube ~30–40 min |
| TensorFlow | Instructor’s DL framework of choice |
| Keras | Wrapper over TensorFlow |
| PyTorch | Alternative framework; instructor has not used it |
| LangGraph | Mandatory agentic project |
| CrewAI | Mandatory agentic project |
| n8n | Mandatory agentic project; bootcamp emphasis |
| LangFlow | Mentioned as alternative; not bootcamp focus |
| BERT | Legacy NLP; optional knowledge for interviews |
| OpenAI (APIs) | Data-handling / trust considerations for RAG |
| Azure / AWS / GCP | Paid APIs for production |
| Google Translate API | Example of free-tier rate limits |
| `max()` / `min()` on dict values | Netflix popularity questions |
| `ndim` | NumPy array dimensions for nested score list |
| `np.max` | On wicket column (2nd element of tuples) in walkthrough |
| Udemy / instructor platform courses | ML + DL access to be provided |
| YouTube (instructor channel) | NumPy, Pandas, EDA (~5 hr), news summarizer demo |
| Gmail | Mentioned with automated reports in dashboard project |
| TTS / STT | Needed for PDF-to-course style project (costly) |

## Code & Examples

### Netflix dictionary — lookup by title key
```python
# netflix is a dict: title -> view count
netflix["<movie_title_key>"]  # returns view count for that title
# Instructor example: Indian movie key -> 2,770,879 views; answer choice D
```

### Most / least popular by views
```python
# One approach: max/min on dict values (keys = titles)
max(netflix, key=netflix.get)   # highest-view title e.g. "Gaga 5'2" ~3.71M views
min(netflix, key=netflix.get)   # lowest-view title e.g. "Hilda"
# Answer choice B for the stated question
# If four title combinations given as options, compare netflix[k] for each k manually
```

### Merge last-week views (conceptual)
```python
# Given netflix (totals) and new_week dict (same keys, last-week views)
# Add counts per title, then find argmax of combined totals
# Instructor solution title: Delhi Belly
```

### NumPy / cricket `score` structure
```python
# player: list of player IDs (length 1502)
# score: list of 1502 tuples -> (runs, wickets, catches)
import numpy as np

arr = np.array(score)   # or equivalent from list
arr.ndim                # 2 — list of tuples treated as 2D structure

# Count players with 0 runs: filter/count where tuple[0] == 0
# "First innings" variant: separate exercise on same tuple layout
# Max wickets: max of second element, e.g. np.max(...) on wicket column
```

*Exact assignment solution code was not fully read line-by-line in session; instructor will distribute complete answers.*

## Q&A / Discussion Points

- **FastAPI vs standard API for bootcamp?** Use **FastAPI** in industry often, but course uses **Streamlit**; FastAPI is optional self-study.
- **Third-party agentic integrations?** Yes—weather, FX, etc.; free APIs for practice, paid cloud for production.
- **DataFrames vs Series in Pandas?** Focus **DataFrames**; skip deep Series study for now.
- **TensorFlow or PyTorch?** Pick one; instructor uses TensorFlow only; both are valid.
- **ML/DL portfolio in addition to gen AI?** Five bootcamp projects are sufficient for jobs; optional ML classification + DL image + detection/segmentation helps; BERT optional for quirky interviews.
- **When to take ML/DL courses?** At own pace (1.5x–2x); not prerequisite; finish before bootcamp ends if possible.
- **Codecademy course for NumPy?** Instructor recommends his **YouTube** NumPy/Pandas shorts + ~5 hr EDA video instead.
- **Python level for gen/agentic projects?** What’s taught + vibe coding is enough; practice if uncomfortable writing code.
- **Safety for PDF chatbot?** Prefer open-source for sensitivity; API data residency/trust issues; guardrails for PII (covered later).
- **Can’t code 100% independently?** Normal early in bootcamp; practice Python; compare to people with 10 years in field.
- **n8n in projects?** Part of agentic track; see upcoming classes.
- **Prompt engineering?** Dedicated class coming (Srisisha).
- **End-to-end project ideas?** Dashboard/report automation (Gmail), generic agentic multi-API app, **YouTube shorts automation** (class agreed), **course generation from PDF** (TTS/STT cost; still accepted as project idea).

## Action Items & Assignments

- [ ] Post doubts on **Discord doubt channel** if skipping Thursday doubt-clearing
- [ ] Bring questions to Thursday sessions (avoid attending with nothing to ask)
- [ ] Complete graded **Pandas/NumPy** assignment; **self-check** against instructor answers **before Saturday**
- [ ] Expect full assignment solutions from Codecademy team **by Saturday** (instructor traveling to Hong Kong; answers shared after arrival)
- [ ] Complete five **mandatory** bootcamp projects (RAG, fine-tuning, LangGraph, CrewAI, n8n)
- [ ] Optionally: ML classification, DL image classification, segmentation/detection projects
- [ ] Watch instructor **YouTube**: NumPy (~30–40 min), Pandas (~30–40 min), EDA (~5 hr) when needed
- [ ] Self-study **FastAPI** if interested (blogs/articles TBD from instructor)
- [ ] Practice **Python** if reading code is easier than writing it
- [ ] Propose **fascinating end-to-end project ideas** on Discord (YouTube automation, PDF-to-course, etc.)
- [ ] Attend **Saturday** regular class (instructor plans to arrive ~3 hours before class)

## Open Questions / Unclear Spots

- Exact **movie/show title strings** in the Netflix dict (e.g. `"Gaga 5'2"`, Indian movie key) may be ASR/typo variants from the assignment PDF—verify against your assignment handout.
- Instructor said he could not quickly find **"Gaga 5'2"** in the on-screen dict during live demo (large list); relied on `max()` output and manual Notepad check (~3.71M views).
- **"Replicate Google PDF learning document course"** — clarified as generating a **course from a PDF** (not free due to TTS/STT); accepted as a project direction without full spec.
- Which **LLM(s)** and specific **APIs** for upcoming agentic labs were asked in chat but not fully answered in the recorded portion beyond general patterns above.
