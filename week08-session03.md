# RAG Project Showcase & Feedback Session

## TL;DR
This was a doubt-clearing and project showcase session where students presented their RAG chatbot projects. The instructor reviewed several projects live, provided feedback on architecture choices and impact, briefly discussed AWS EC2 deployment options and costs, and previewed the upcoming agentic AI module.

## Topics Covered
- Project competition reminder (LinkedIn posting deadline)
- Live showcase of student RAG projects (NASA, Pandas, blog, Japan travel, Viscom, National Geographic)
- Feedback on project impact, architecture, and topic selection
- AWS EC2 deployment overview (instance types and pricing)
- Guardrails preview (to be covered in the agentic AI module)
- Upcoming syllabus: agents, LangChain, LangGraph, CrewAI

## Key Concepts Explained

### Choosing an Impactful RAG Topic
- A RAG project adds the most value when it brings in knowledge the LLM does **not** already have. Scraping widely-known content (e.g., Pandas docs, Wikipedia bios) is fine for practice but adds little real impact because the base model already knows that information.
- Prefer niche, proprietary, or frequently-updated sources where retrieval genuinely augments the model.

### Deployment on AWS EC2
- When deploying a RAG app, server size depends on whether you use an API-based LLM or a local model.
- **API-based**: a small instance (t2.small / t2.medium, ~2 GB RAM) is usually sufficient since the heavy lifting is done remotely; you still need enough memory for LangChain and other libraries.
- **Local model**: if the model itself is ~10 GB, you need a much larger instance (e.g., t3.xlarge, 16 GB RAM, 4 vCPUs), which can be ~40x more expensive than a micro instance.
- Always shut down instances after testing to avoid unexpected bills. AWS provides initial free-tier credits.

### Controlling Out-of-Scope Answers
- The primary mechanism is the **prompt template** — instruct the model to refuse or redirect off-topic questions.
- A more advanced approach is **guardrails**, which will be introduced in the agentic AI section of the course.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Groq (API) | Cloud inference API used by multiple students; free tier available |
| Llama 3.3 70B | LLM accessed via Groq API in two student projects |
| Ollama | Local LLM runtime used by two students (Pandas RAG, blog RAG) |
| Gemma 4 (4B) | Local model run via Ollama for the blog RAG project |
| Beautiful Soup | HTML scraper used in several projects |
| ChromaDB | Vector store used in the blog RAG project |
| LangChain | Orchestration framework used across projects |
| MiniLM (all-MiniLM) | Lightweight sentence-transformer embedding model |
| Claude Sonnet | LLM used in the Japan travel RAG project |
| OpenAI API | Used in the National Geographic environmental assistant |
| AWS EC2 | Server hosting for deployment; instance types t2.nano through t3.2xlarge discussed |
| Streamlit | Implied frontend framework (common template across projects) |

## Code & Examples
No new code was written in this session. The instructor walked through student repositories and live demos.

## Q&A / Discussion Points
- **"How can we deploy our chatbot when we use a local LLM?"** — Same deployment flow, but you need a larger EC2 instance since the model weights must reside on the server. API-based setups are cheaper and simpler.
- **"How can we make the chatbot reject out-of-topic questions more robustly?"** — Primarily through prompt template engineering. Guardrails will be introduced later in the agentic AI module.
- **"Should we add evaluation to the current project or a new one?"** — The instructor recommended doing evaluation as a separate, new project.

## Action Items & Assignments
- [ ] Post your RAG project on LinkedIn for the project showcase competition (deadline: today, evaluation starts tomorrow)
- [ ] Consider adding an evaluation notebook to your existing RAG project
- [ ] Work on additional projects to build out your GitHub profile (aim for 6-7 projects by end of bootcamp)
- [ ] Optionally try deploying your RAG app on AWS EC2 following the instructor's video guide (be cautious of costs)

## Flashcards

**Q: Why is scraping Pandas documentation a poor choice for a production RAG project?**
A: LLMs are already trained on widely-available documentation like Pandas, so RAG adds little value. Choose niche or proprietary content the model doesn't already know.

**Q: What AWS EC2 instance size is sufficient for deploying a RAG app that uses an API-based LLM?**
A: A t2.small or t2.medium (~2 GB RAM) is usually enough, since the LLM inference happens remotely and you only need to host the app and libraries like LangChain.

**Q: Why does a locally-hosted LLM require a much larger EC2 instance?**
A: The model weights (e.g., 10 GB+) must fit in server memory alongside the application, requiring instances like t3.xlarge (16 GB RAM), which can be ~40x more expensive.

**Q: What is the primary way to prevent a RAG chatbot from answering off-topic questions?**
A: Through the prompt template — explicitly instruct the model to refuse or redirect questions outside the intended domain.

**Q: What is the more advanced alternative to prompt-based topic control?**
A: Guardrails — rule-based or model-based filters that enforce hard boundaries on agent behavior. These are covered in the agentic AI module.

**Q: What is the key risk to watch for when deploying on AWS EC2 for testing?**
A: Forgetting to shut down the instance after testing, which can result in a large unexpected bill. Always terminate or stop instances when done.
