# Doubt-Clearing: Website Chatbot Assignment (RAG + Q&A)

## TL;DR
This session was a doubt-clearing class focused on the bootcamp’s website chatbot assignment: scrape a domain-relevant site, build PDF datasets (scraped content + AI-generated Q&A), then answer user questions via Q&A matching first and RAG over chunked/vectorized scrape data when no match is found. The instructor reiterated the May 29 deadline, encouraged vibe coding (AI-assisted prompting) for implementation, demoed a resume-scoring platform with a coupon code, and previewed future doubt classes (MCP, deployment, LangGraph/LangChain depth) after a one-week break.

## Topics Covered
- Assignment recap: real-time website chatbot with scraping, PDF datasets, Q&A-first routing, then RAG
- Deadline (May 29), scope expectations, and vibe coding for the full build
- Website selection strategy and scraping approaches (full site vs. top 10–20 manual links)
- Three strategies for handling the Q&A document (without vs. with vectorization; LLM polish for terse answers)
- UI recommendation (Streamlit) and whether sites are “scrapable” (trial and error)
- Instructor leave / schedule: break until next doubt class on May 30; time for project + ML/DL videos
- Resume platform demo (`resume.com`) with coupon `ZEPNEW` and resume-vs-JD scoring example
- Clarification: vibe coding applies to the whole project, not only scraping
- Student progress check-in and feedback on bootcamp pace
- Future doubt-class topics: MCP, deployment (AWS/Azure), LangChain vs LangGraph, job titles
- Shared ML/generative-AI interview question resources (deck + Discord)
- Q&A flowchart for assignment architecture; scraping as the mandatory first step
- Poll at end of session

## Key Concepts Explained

### Assignment architecture (Q&A first, RAG fallback)
- What it is: User questions hit a Q&A PDF first; on a match, return that answer. If no match, retrieve from scraped website content (chunked, optionally vectorized in a vector DB/index), combine with the question, and pass to an LLM for the final answer.
- Why it matters / when to use it: Mirrors a practical production pattern—curated FAQs for fast, reliable answers; RAG for long-tail questions the FAQ does not cover.
- Caveats: The instructor emphasized keeping this deliverable simple; a “version two” may add complexity later. Q&A “exists” means a successful Q&A match in the flow he drew.

### Website scraping as foundation
- What it is: Pick a website aligned with your background (e.g., banking, telecom); scrape content (Beautiful Soup mentioned; students research other tools). Output is textual data, preferably stored as PDF (recommended over .doc/.docx).
- Why it matters / when to use it: Scraping may consume ~50% of project time; there is no shortcut to know if a site is scrapable—you must try.
- Caveats: If full-site scrape fails, manually identify top 10–20 URLs and loop-scrape them. Instructor does not grade *how* you scrape, but scraping is a required step before the chatbot flow.

### Q&A dataset generation
- What it is: After scraping, use prompts/AI to generate a Q&A document from the site content; you end up with two PDF chunks: scraped corpus + Q&A.
- Why it matters / when to use it: Completes “step one to four” of dataset prep before building the app.
- Caveats: Vectorizing the Q&A is **not recommended** but allowed; three handling strategies were outlined (see below).

### Three ways to use the Q&A document
- What it is: (1) NLP similarity—tag user question against stored questions, score, threshold, pick closest; (2) optional vectorization using the same RAG-style pipeline; (3) NLP match + pass matched answer through LLM for richer “chat completion” when answers are too short (e.g., fee is “$299” → fuller sentence).
- Why it matters / when to use it: Lets students avoid vectorizing FAQs while still improving UX with an LLM rewrite step.
- Caveats: Strategy choice and UI are student decisions; Streamlit is recommended for UI.

### RAG over scraped content
- What it is: When Q&A does not match, chunk scraped text, store embeddings in a vector database or index, retrieve relevant chunks for the user question, then prompt the LLM with question + context.
- Why it matters / when to use it: Covers questions not captured in the generated FAQ.
- Caveats: Instructor called this a complex RAG project—most doubts resolve during implementation.

### Vibe coding
- What it is: Interacting with AI via well-framed prompts/commands so AI generates code (scraping, RAG app, Streamlit UI); iterate and test until something works.
- Why it matters / when to use it: Expected approach for the assignment within the given timeline.
- Caveats: Generated scrapers may include safety limits (e.g., max pages); code must be validated on the target site.

### LangChain vs LangGraph (preview)
- What it is: LangChain (already used in RAG chatbots—e.g., `langchain_core`, provider integrations) vs LangGraph (planned for agentic flows); same creator, different problem focus.
- Why it matters / when to use it: LangChain for GenAI/RAG patterns; LangGraph for agentic AI—differences and math will be covered in upcoming LangGraph sessions.
- Caveats: Deep learning in parallel is fine but not a blocker for GenAI; curriculum focuses on agentic solutions, with extra deployment/MCP content in doubt classes around June.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Beautiful Soup | Mentioned for scraping whole sites or a list of URLs in a loop |
| Streamlit | Recommended UI for the chatbot project |
| LangChain | Already used in bootcamp RAG chatbots; deeper explanation planned with LangGraph |
| LangGraph | To be covered in doubt classes; agentic AI focus |
| Vector database / index | Store chunked embeddings for scraped content when Q&A misses |
| LLM | Final answer generation; optional polish step after Q&A match |
| NLP similarity algorithms | Preferred Q&A matching without vectorization |
| Vibe coding | AI prompt-driven code generation for scrape → RAG → UI |
| `resume.com` | Instructor’s platform; resume scorer, cover letter, mock interview, career coach, etc. |
| Coupon `ZEPNEW` | 100 credits on resume platform; instructor may refill after trials |
| Discord | Assignment details, interview question links, async questions |
| Copilot / Copilot agents | Discussed in enterprise context—often blocked by security teams |
| Copilot Studio | Agents not allowed in some companies (instructor + student experience) |
| AWS / Azure | Mentioned for a future deployment example in doubt class |
| MCP | Planned dedicated doubt-class topic |
| Databricks, Airflow, CI/CD | Example “missing keywords” from resume demo—not on resume text but instructor has experience |

## Code & Examples

### Approximate scraping pattern (instructor vibe-coded example)
The instructor showed AI-generated scraping code with a base URL and a safety cap (e.g., max pages 1,000) to avoid runaway requests on non-scrapable sites. Exact code was not fully read aloud; students should generate and test their own variant.

```python
# Approximate structure from session discussion — not verbatim from transcript
BASE_URL = "https://example.gov.hk"
MAX_PAGES = 1000  # safety limit

# Option A: crawl/scrape full site (tool of choice, e.g. Beautiful Soup)
# Option B: manual top-N URLs
urls = [
    "https://example.gov.hk/page1",
    # ... 10–20 important links
]
for url in urls:
    # fetch + parse + extract text
    pass
```

### Manual fallback scrape loop
```python
# Pass explicit links if full-site scrape fails
links = ["...", "..."]  # top 10–20 pages
for link in links:
    # scrape with Beautiful Soup
    pass
```

### Assignment data-prep steps (procedure)
1. Choose a profile-relevant website (example: `immd.gov.hk` — Hong Kong immigration; no chatbot, many pages).
2. Scrape → textual content → save as **PDF** (recommended).
3. Prompt AI to generate a **Q&A PDF** from the scraped material.
4. Build chatbot: user question → Q&A match? → return answer (optionally LLM-enhanced) **else** RAG on scraped PDF chunks → LLM answer.
5. Wrap in **Streamlit** (or UI of choice).

### User-question flow (instructor flowchart)
```
User question
    → Search Q&A document
        → Match? → Return Q&A answer (optional: LLM rewrite)
        → No match? → Retrieve from scraped/vectorized content
            → LLM(question + retrieved context) → Answer
```

### Resume platform demo (live)
- Log in → AI services → Resume scorer & recommendation
- Paste job description (JD copied via image-to-text when copy was blocked) + resume
- Output: score (~80 in demo), missing keywords, readability/ATS notes, weak vs impactful phrases, skill gaps

## Q&A / Discussion Points

- **Is there a way to know if a website is scrapable without asking AI?** No—you must try; scraping may take half the project time.
- **What did “vibe coding” mean—scraping only?** The whole project (scrape, RAG app, Streamlit) can be vibe coded via iterative AI prompts and testing.
- **Should the Q&A PDF be vectorized?** Not recommended; NLP similarity or optional LLM polish are preferred; vectorization is optional.
- **Assignment flow confirmation (Balaram / Discord):** UI receives question → Q&A first → on match, answer from Q&A; on no match, use scraped data (vector DB/index) → LLM. “Q&A exists” = match found. Scraping and storing data is the **first** step before chatbot logic.
- **Scrape first, then PDF, then LLM?** Yes—web scrape and persist data first, then build the chatbot pipeline.
- **Can we change the flow?** Brainstorming allowed, but instructor has explained the standard flow multiple times.
- **Deep learning behind schedule?** Not a blocker for GenAI; can study DL in parallel during the break.
- **Job titles to target?** Agentic AI developer/engineer, AI engineer.
- **ML interview questions?** Instructor shared a deck/link in chat and will post on Discord (older + three newer question sets).
- **LangChain / LangGraph sessions?** LangGraph on curriculum; LangChain differences explained during LangGraph depth sessions.
- **Enterprise Copilot agents blocked?** Common—security blocks agents; RAG-style work still possible in constrained environments.
- **Future topics:** MCP class, deployment walkthrough (AWS or Azure), optional harder “v2” of the same project after feedback.

## Action Items & Assignments

- [ ] Complete the **website chatbot project** by **May 29** (shared on Discord; instructor chose date as doable).
- [ ] Pick a **relevant website** for your background; build scrape + Q&A PDF + Q&A-first/RAG chatbot.
- [ ] Use **vibe coding** (clear prompts/commands) for implementation; prefer **Streamlit** UI.
- [ ] Aim for a **matured** [transcript: “nurtured”] deliverable by deadline; refine further after instructor feedback later—not required for v1.
- [ ] During instructor break (~9–10 days): **priority 1** = project; **priority 2** = other ML/DL bootcamp videos.
- [ ] Try coupon **`ZEPNEW`** on resume platform for 100 credits; explore resume/JD tools.
- [ ] Bookmark/share **interview question** links from class/Discord.
- [ ] Reach out on **Discord** for unanswered questions (e.g., Balaram audio/clarification).
- [ ] Complete end-of-class **poll** (“do the needful”).

## Open Questions / Unclear Spots

- Exact **scraping tools/libraries** beyond Beautiful Soup left to student research.
- Instructor resume platform may be **`resume.com`** or a similarly named product—login demo had a temporary glitch (“reach out to my developer”).
- [unclear: **ISE**] — listed among skills the instructor has worked on but not on the demo resume keywords.
- Transcript term **“DAG project”** — almost certainly **RAG project** given context; not discussed as graph DAGs.
- **“Nurtured project”** — likely **matured** submission for May 29.
- Balaram’s full question about PDF vs LLM ordering was **not resolved live** (audio issues); partial answer via Discord flow diagram.
