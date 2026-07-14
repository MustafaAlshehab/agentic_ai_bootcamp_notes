# Module 05 — Q&A, Prompt Engineering, and AI Dashboard Demo

## TL;DR
This was a lightly attended doubt-clearing session rather than a new curriculum lecture. The instructor checked in on Python/ML/transformer confidence, shared study strategies (supplementary videos, mind maps, practice with AI), and answered questions about token usage, prompt structure, and transformer attention. He demoed generating a validated telecom analytics HTML dashboard from a CSV using a refined prompt in an AI chat tool, and previewed optional future topics (MCPs, agent-to-agent) outside the official bootcamp syllabus.

## Topics Covered
- Session logistics and low attendance; optional early wrap-up
- Bootcamp progress check-in (Python, ML, transformers difficulty)
- Learning strategies for hard topics (especially transformers)
- Using AI tools (e.g., Claude Cowork) for mind maps and exercises
- Vibe coding / practical AI use (screenshot folder organization) and token limits
- Prompt engineering: session management, six prompt elements, using AI to refine prompts
- Live demo: CSV → HTML dashboard via AI prompts (Groq mentioned)
- MCPs and agent-to-agent architecture (not in curriculum; instructor’s upcoming videos)
- Transformer attention Q&A (self-attention vs multi-head; “cross-attention” clarification)
- Planning for future doubt-clearing sessions with prepared discussion topics
- Next session: Saturday — generative AI topics

## Key Concepts Explained

### Transformers as a difficult foundational topic
- Core architecture behind modern LLMs; among the hardest material in the bootcamp and in AI broadly.
- Why it matters: understanding transformers unlocks generative AI and much of current industry work.
- The instructor emphasized rewatching, following external videos/blogs, and not expecting one pass to be enough; he referenced his own ~2-hour transformer video and a ~1-hour video he recommends for attention-related doubts.

### Supplementary learning when lectures aren’t enough
- Know the topic names from class, then seek blogs, articles, and other trainers’ videos; take notes and practice.
- Why it matters: bootcamp pace won’t cover every gap; self-directed depth is expected for hard subjects.
- Practice with AI can suggest what to practice and help generate/execute code in the learning loop.

### Token usage and context window limits
- Long, multi-turn chats consume tokens and can hit context limits; the model may “forget” earlier parts of the conversation (risk on free tiers; sometimes on paid).
- Why it matters: heavy tasks (e.g., processing many screenshots, large folders) burn tokens quickly.
- Mitigation: break work into separate sessions; summarize prior work when starting a fresh session rather than assuming the model remembers everything.

### Prompt structure (six elements — partially recalled in session)
- Commonly cited elements: **context**, **persona**, **tone**, **exemplar** (examples), **format**, and a sixth the instructor could not fully recall live ([unclear: possibly “rules,” “constraints,” or “requirements”]).
- Why it matters: clearer prompts improve behavior, output shape, and efficiency.
- **Mandatory (as stated):** context, persona, format. **Tone:** optional. **Examples:** optional but improve results. **Persona** example: “Behave as a top 0.001% data scientist / telecom visualization expert…”
- There is no single “right” prompt, but structure helps; skill improves with use.

### Using AI to refine prompts (meta-prompting)
- Describe the goal prompt and ask the model to improve it for a specific task (e.g., CSV → HTML dashboard with uni/bi/multivariate analysis and KPIs).
- Why it matters: bootstraps better prompts without hand-tuning every word.
- The instructor demonstrated this before running the dashboard generation task.

### Self-attention vs multi-head attention
- Student asked about self-attention vs “cross-attention”; instructor said he did not recognize “cross-attention” in the way asked and redirected to **multi-head attention** vs **self-attention**.
- Why it matters: attention mechanisms are central to transformer blocks.
- Resolution in session: defer to the instructor’s dedicated transformer video(s) for a full explanation rather than a deep live answer. [Note: encoder–decoder models do use cross-attention in the broader literature; the instructor’s live answer was “no cross-attention” — treat as session-specific clarification, not a full architecture survey.]

### AI-assisted analytics dashboards
- A CSV plus a well-crafted prompt can produce an HTML dashboard in minutes; instructor validated KPIs against a dataset he knows well (formatting nits aside).
- Why it matters: illustrates practical “vibe coding” / AI-assisted analytics workflow.
- Minor label edits: safer via direct HTML/CSS tweaks if you know them; re-prompting for small edits is **not recommended** when the prompt/input is already very large (token/context risk).

### MCP and agent-to-agent (outside curriculum)
- Model Context Protocol (MCP) and agent-to-agent networks/architecture are **not** part of the bootcamp program.
- Instructor is recording weekend content on these; may cover in a future doubt-clearing class and announce when videos are live.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| GitHub | Account setup referenced from first class |
| Python | Bootcamp foundation; students found ML portion manageable |
| Machine learning (general) | Prior modules; confidence check-in |
| Transformers | Last week’s class; students found it new/difficult |
| Claude / Claude Cowork | Student used for mind maps and small exercises |
| Groq | Instructor opened Groq for prompt refinement / dashboard demo |
| HTML / CSS | Dashboard output format; manual edits for small changes |
| CSV | Input data for dashboard demo (`Agentic AI` file mentioned) |
| Power BI | Contrasted with pre-2015 manual HTML/CSS dashboards (~4-month projects) |
| MCP (Model Context Protocol) | Instructor learning/recording; not in bootcamp syllabus |
| Agent-to-agent (A2A) architecture | Upcoming instructor videos; optional future discussion |
| Instructor YouTube channel | ~2 hr transformers video; ~1 hr video for attention topics; ~5 min dashboard demo video; search instructor name + keyword |
| Slack | Student context (engineers’ discussions at AI company) |
| Poll (class) | Instructor raised a lighthearted attendance poll |

## Code & Examples

No formal bootcamp repo walkthrough. Reconstructed workflow from the live demo:

### 1. Meta-prompt to refine a dashboard prompt (paraphrased from transcript)

```text
I want to fine-tune this prompt:

Behave as a top 0.001% visualization expert in the telecom industry.
Analyze my data properly for relevant insights.
Do in-depth analysis including univariate, bivariate, and multivariate analysis.
Come up with proper KPIs for my final dashboard.

Goal: fine-tune this prompt so I can use it to generate a beautiful HTML-based dashboard from a CSV file.
```

The model returned an expanded prompt (e.g., “world-class telecom data visualization expert and full-stack dashboard engineer,” plus technical output expectations when CSV or pasted image is provided).

### 2. Dashboard generation steps (approximate)

1. Start a **new** chat window/session (session hygiene).
2. Paste the refined prompt.
3. Attach/upload the CSV (demo file name sounded like “Agentic AI”).
4. Wait for generation (slow in live demo).
5. Save output as HTML (e.g., `dashboard new.html`).
6. Open in browser; validate metrics if you know the dataset.
7. Optional small edits: edit HTML/CSS directly rather than extending an already token-heavy thread.

### 3. Prompt hygiene for heavy tasks (instructor practice)

- Split long investigations across sessions.
- When switching sessions, **summarize** prior conclusions into the new session’s opening message.
- Avoid endless follow-ups in one thread when tokens and context are a concern.

## Q&A / Discussion Points

| Question / topic | Answer / outcome |
|------------------|------------------|
| How is the bootcamp going (Python, ML)? | Non-technical student: steep but helpful for understanding engineers on Slack; practicing. Monica: Python/ML OK; transformers very new, needs deeper review. |
| How to learn transformers? | Expect difficulty; multiple passes, external videos/blogs; instructor’s long transformer video on his channel. |
| Vibe coding: sort/rename many screenshots with Claude — tokens run out | Improve prompts; break into sessions; summarize when restarting; learn prompt elements over time; heavy batch jobs are inherently token-expensive. |
| What are the six prompt elements? | Context, persona, tone, exemplar (examples), format, plus a sixth not recalled live [unclear]. Mandatory: context, persona, format. |
| MCPs in the program? | Not in curriculum; instructor doing MCP + agent-to-agent videos; may discuss later. |
| Self-attention vs cross-attention? | Instructor: not familiar with “cross-attention” as asked; pointed to self-attention vs **multi-head attention**; watch his ~1 hr transformer video first. |
| Can we end class early? | Low attendance; demo ran while waiting; class wound up after dashboard finished. |
| Future doubt-clearing format? | Instructor may prepare discussion topics instead of ending early when few students attend. |

## Action Items & Assignments

- [ ] Review **transformers** again (multiple passes); use instructor’s channel videos (including ~2 hr overview and ~1 hr attention-focused video).
- [ ] Continue **practicing** Python/ML and upcoming **project** (students expected it to clarify concepts).
- [ ] Attend **Saturday** class — **generative AI** topics begin.
- [ ] When stuck on a topic: find **blogs/articles/other videos**, take notes, practice (optionally with AI-generated exercises/code).
- [ ] Optional: watch instructor’s **latest short (~5 min) dashboard video** (two-prompt CSV → HTML demo).
- [ ] Optional: watch upcoming instructor videos on **MCP** and **agent-to-agent** (outside official syllabus).
- [ ] Instructor: prepare **discussion topics** for future doubt-clearing sessions; announce MCP/A2A videos when live.

## Open Questions / Unclear Spots

- Sixth prompt element named in “six elements” framework was not recovered in session ([unclear: rules / constraints / requirements]).
- **Cross-attention:** student saw it elsewhere; instructor denied it in the moment and redirected to multi-head attention — left without an in-class technical comparison.
- Exact AI product for the live dashboard demo was loading under “Groq” / agent UI; full UI and export steps were partially obscured by ASR (“Technopals” / “TechnoTelcoPals” likely telecom demo branding).
- Whether “cross-attention” in the student’s source was encoder–decoder cross-attention vs a mislabeled diagram was not resolved in this session.
