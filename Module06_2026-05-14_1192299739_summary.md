# Week 6 Doubt-Clearing: Project Reviews, Schedule, Copilot Agents & LinkedIn Strategy

## TL;DR
This session was mostly Q&A and career guidance rather than new technical curriculum. The instructor reviewed student projects (PDF RAG chatbots), previewed upcoming fine-tuning topics and a class break, then spent the bulk of the hour on LinkedIn profile optimization and a repeatable content workflow (batch posts, scheduling, images, polls). Students also got practical pointers on API keys (Gemini, Groq), Microsoft Copilot agents, and portfolio habits (GitHub + project reports).

## Topics Covered
- Student project troubleshooting (Streamlit + LangChain errors, timeouts)
- Ejim’s PDF RAG chatbot GitHub review and project-report habit
- Bootcamp schedule: fine-tuning this weekend, instructor absence, assignment timing
- When to start independent vs. assigned projects
- Free/open API options (Gemini, Groq) and vibe-coding model swaps
- Microsoft Copilot agents (prompt-based vs. knowledge-based)
- LinkedIn profile reviews and optimization feedback
- LinkedIn posting strategy (batching, scheduling, content mix, formatting, engagement tactics)
- Medium/blogging for learning and visibility
- Portfolio visibility (GitHub timing, skills keywords)
- Non-English markets (German posts, XING vs. LinkedIn)
- Recommended AI influencers to follow on LinkedIn

## Key Concepts Explained

### Project report (4–5 page design doc)
- What it is: A short high-level document (problem statement, context, objectives, how it works, key inputs, architecture diagram, step-by-step flow, product demo screenshots) that summarizes a project beyond the README.
- Why it matters / when to use it: Helps reviewers, recruiters, or future-you understand the system quickly; pairs well with a polished GitHub repo.
- Caveats: README is still valuable; the report is an additional habit, not a replacement. Include RAG diagrams (chunking → embedding → vector DB → UI) when relevant.

### Publish projects on GitHub early
- What it is: Push completed work to GitHub as soon as it’s ready rather than waiting.
- Why it matters: Timestamps show a longer track record of building (e.g., “this was done 6–7 months ago”), which can impress recruiters reviewing your profile over time.
- Instructor emphasis: Keep doing projects and post them immediately.

### Microsoft Copilot agents
- What it is: In paid/org Copilot, “Agents” let you build assistants tied to Microsoft ecosystem data or custom knowledge.
- Why it matters: Org-specific workflows (e.g., summarize documents in a OneDrive folder into an executive report).
- Types:
  - **Prompt-based agent**: Persona/instructions only (like a scoped ChatGPT)—e.g., “financial advisor” with out-of-scope disclaimers.
  - **Knowledge-based agent**: Upload docs (PDF, Excel, limited extensions)—document chatbot style.
  - **Deeper integrations** (org-dependent): Connect SharePoint, OneDrive, Outlook, etc.
- Caveats: Access varies by company; instructor’s org could create basic agents but not connect all sources. Explore what your tenant allows.

### Vibe coding for model/API swaps
- What it is: Take existing course code (e.g., using Groq) and ask an AI to rewrite it for another provider (Gemini, etc.).
- Why it matters: Reduces friction when you can’t use the exact stack from class (region limits, API choice).
- Caveats: You still need to obtain API keys yourself; instructor can’t demo Gemini from Hong Kong historically due to restrictions (noted as possibly lifting).

### RAG-based PDF chatbot as portfolio starter
- What it is: A practical first project aligned with recent lessons—ingest PDFs, chunk, embed, retrieve, chat.
- Why it matters: Demonstrates GenAI/RAG skills; several students already built variants.
- Instructor emphasis: Nobody is holding you back after week 6; week 7 brings a compulsory assignment with “real project” flavor.

### LinkedIn as a distribution channel (not just job search)
- What it is: Regular posting and engagement to build visibility, followers, and opportunities.
- Why it matters: Activity can lead to connections, freelancing, and inbound opportunities—not only when actively hunting.
- Instructor emphasis: Inactivity (e.g., no posts for months) hurts; start being active from week 6 onward (suggested start: Monday 18 May in session).

### LinkedIn content batching & scheduling
- What it is: Block ~2 hours once per week; draft ~7 posts; schedule Mon–Sun using LinkedIn’s scheduler; ask AI for best posting time for your timezone/audience.
- Why it matters: Consistency without daily friction.
- Content mix (example week):
  - ~3 **learning posts** (what you studied—e.g., RAG)
  - ~2 **AI news** posts (web-grounded)
  - ~1–2 **inspired posts** (rephrase high-performing posts you saw in feed)
  - Optional: **polls** (~1/week) for engagement
- Formatting tips: Strong first-line hook; hashtags; emojis via “LinkedIn symbols” search; **attach images** for traction; **avoid public links in post body**—put video/links in **comments** instead.
- Tagging: Tag willing connections (instructor offered to be tagged) for reach—don’t random-tag.

### LinkedIn profile optimization
- What it is: Keyword-rich skills, third-person About section, bulletized experience, featured posts, headshot-friendly summary for recruiters.
- Why it matters: Recruiters often see a “headshot” view plus attached resume; sparse experience bullets can lead to quick rejects.
- Skills to add (examples): GenAI, generative AI, agentic AI, LangGraph, CrewAI, n8n, ML, DL, NLP, Python, vibe coding—maximize relevant keywords.
- About section: Prefer third person (“Ajay is an engineer…” vs. “I am…”).

### Blogging (Medium)
- What it is: Longer-form writing (~1/month minimum; 1/week ideal).
- Why it matters: Researching for an article deepens knowledge even on familiar topics; viral potential increases visibility.
- Instructor preference: English on Medium for wider audience.

### Germany / non-English markets
- What it is: Split strategy—English for global tech reach; local language/platforms for local employers.
- Caveats: Instructor was uncertain on English vs. German for LinkedIn in Germany—suggested experimenting (e.g., alternate weeks) or focusing German on **XING** while LinkedIn stays English for wider reach.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Streamlit | Student-built research-paper chatbot UI; reported instability/frontend errors |
| LangChain | Module import/chain errors in student project; Gemini workaround removed module, led to timeout |
| Gemini | Student used for vibe coding; API via Google AI Studio; region restrictions mentioned (Hong Kong) |
| Google AI Studio | Where to create Gemini API keys |
| Groq | Free-tier API keys for some models; used in instructor’s course code (not Elon Musk’s Grok) |
| ChatGPT | Free option for vibe coding |
| Mistral | Student asked about free usage; detailed walkthrough deferred to Saturday (open-source models) |
| Microsoft Copilot (paid/org) | Agents feature: prompt-based, knowledge-based, optional M365 data sources |
| SharePoint / OneDrive / Outlook | Potential Copilot agent data sources (org-dependent) |
| GitHub | Standard repo layout praised (requirements.txt, README, structure); publish early |
| LinkedIn | Profiles reviewed; scheduling, posts, polls, symbols/emojis, tagging |
| Medium | Recommended for blogs; English for reach |
| XING | German professional network; suggested for German-local content |
| Spotify (podcasts) | Student mentioned KI podcast; brief discussion |
| Discord | Channel for async questions between classes |
| PDF RAG chatbot | Reference project pattern for bootcamp participants |

## Code & Examples

No new instructor-led code was written in this session. Reconstructed workflows from discussion:

**Ejim-style project layout (described, not typed live)**
- GitHub repo with clear name, file structure, `requirements.txt`, README, and application files for a PDF RAG chatbot.

**Vibe-coding model swap (conceptual)**
```text
1. Start from course code that calls Groq (or another provider).
2. Prompt your AI assistant: "Rewrite this code to use the Gemini API instead, keeping the same behavior."
3. Add your Gemini API key from Google AI Studio.
4. Test locally; fix Streamlit/LangChain errors iteratively with the AI.
```

**Example LinkedIn learning-post prompt (paraphrased from instructor demo)**
```text
I was recently taught about RAG in class, including building a PDF-based RAG chatbot.
Write a LinkedIn post explaining RAG for my audience: features, advantages, and a strong opening hook.
Add a question if helpful, and include relevant hashtags.
```

**Weekly LinkedIn workflow**
```text
1. Pick one day; block 2 hours.
2. Draft 7 posts (3 learning, 2 AI news, 1–2 inspired/rephrased).
3. Generate images for posts; format with emojis/symbols.
4. Avoid links in post body; put links in first comment.
5. Schedule Mon–Sun; ask AI for best time (e.g., audience in India, poster in Hong Kong).
6. Optional: 1 poll/week for engagement.
```

**Project report outline**
```markdown
# Project Title
## Problem Statement
## Context & Objectives
## How It Works / Key Inputs
## Architecture (diagram: chunk → embed → vector DB → UI)
## Steps / Flow Explanation
## Product Demo (screenshots, e.g., Streamlit)
```

## Q&A / Discussion Points

| Question / topic | Answer / clarification |
|------------------|------------------------|
| Streamlit + LangChain module error and timeout on research-paper chatbot | Likely frontend/module issue; Gemini removed LangChain module; timeout followed; app still not working at time of call—student understood procedure |
| When to start first project? | Can start now (end of week 6); optional PDF/RAG projects; compulsory assignment after week 7 |
| Gemini / Mistral with free API keys | Gemini via Google AI Studio (self-serve); Saturday class will cover open-source models; instructor limited on live Gemini demo in HK; use AI to port Groq-based code |
| Microsoft Copilot agents use cases | Prompt persona agents; doc-upload agents; org may allow M365-connected agents for folder/report workflows—explore tenant permissions |
| Portfolio projects to improve job chances | Deferred into LinkedIn/GitHub visibility strategy; build RAG projects, document well, post actively |
| Articles vs. posts on LinkedIn | Both fine; mix recommended; polls also help engagement |
| English vs. German content (Germany) | Medium: English; LinkedIn: uncertain—try mix or German on XING + English LinkedIn; local employers may prefer German |
| Is LinkedIn used in Germany? | Some use XING more; can maintain both |
| Instructor absence | Classes suspended 23, 24, 28; assignment by 17th; doubt-clearing 21st still on |

## Action Items & Assignments

- [ ] Add a **4–5 page project report** to strong repos (architecture diagram, demo screenshots)—Ejim and others as reference.
- [ ] Keep building **PDF/RAG chatbots**; use Ejim’s GitHub repo as structure reference.
- [ ] **Publish projects on GitHub immediately** when ready.
- [ ] Prepare for **fine-tuning** and related topics this weekend (Sat/Sun classes continue).
- [ ] Expect **assignment** (RAG/fine-tuning related, vibe-coding friendly) by **17th**; ask questions on **21st** doubt-clearing.
- [ ] Note **no class** on **23rd, 24th, and 28th** (instructor unavailable).
- [ ] Obtain API keys as needed: **Google AI Studio** (Gemini), **Groq**, or wait for **Saturday open-source model** guidance.
- [ ] **LinkedIn**: dedicate 2 hours/week; create and **schedule 7 posts**; include images; ~3 learning / ~2 news / ~1–2 inspired; consider polls.
- [ ] Rewrite About in **third person**; add extensive **skills** keywords; add **experience bullets** where missing.
- [ ] Increase LinkedIn activity starting **18 May** (instructor suggestion); instructor open to being **@tagged**.
- [ ] Optional: **Medium** blog at least monthly (English).
- [ ] Germany-based students: consider **XING** for German posts, LinkedIn for English/wider audience.
- [ ] Fix broken/private **GitHub** links on LinkedIn (noted for Thisara).
- [ ] Follow AI creators for inspiration (e.g., Eduardo Odax, Shubham Saboo—and others mentioned in session).

## Open Questions / Unclear Spots

- [unclear: "Script 380" / student identifier at ~line 53] — likely a display name or platform handle; student confirmed as **Ejim**.
- [unclear: "standardization vs. normalization" ~line 245] — question was raised but answered by pivoting to Gemini access; no explanation captured in transcript.
- [unclear: "Sadia" vs. "Nadia" ~line 522] — instructor greeting may not match chat name spelling.
- Best language strategy for **LinkedIn in Germany** (English vs. German) — instructor explicitly not sure; experiment or use XING for German.
- Exact **assignment** spec not yet created (only described as RAG/fine-tuning-related, due by 17th).
- Instructor may demo **Copilot Agents** in a future session if agent access can be enabled on their account.
