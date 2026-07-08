# Bootcamp Final Session: Career Prep — Interviews, LinkedIn & Resumes

## TL;DR
The final session of the Codecademy AI Engineering bootcamp focused entirely on career preparation rather than technical content. The instructor outlined six mandatory portfolio projects (RAG, fine-tuning, LangGraph, CrewAI, and two n8n projects), walked through common generative/agentic AI interview questions, demonstrated how to automate LinkedIn posting with AI, and shared resume formatting best practices including AI-powered resume scoring against job descriptions.

## Topics Covered
- Mandatory portfolio projects and GitHub requirements
- Common interview questions for generative AI and agentic AI roles
- LinkedIn content strategy and automation (15 min/week)
- Resume formatting best practices (simple, parsable, no infographics)
- Using AI to score resumes against job descriptions (Google XYZ format)

## Key Concepts Explained

### Portfolio Project Requirements
- Students must complete six projects by July 31: (1) RAG project, (2) fine-tuning exercise, (3) LangGraph agentic AI project, (4) CrewAI agentic AI project, (5) n8n non-AI data flow, (6) n8n agentic AI project
- Each project must include: code pushed to GitHub, a `requirements.txt`, a `report.pdf`, and a well-structured README with architecture diagrams
- The instructor emphasized that hands-on project experience makes interview preparation almost automatic

### Interview Strategy for GenAI/Agentic AI Roles
- Interviewers rarely ask direct textbook questions — they ask you to walk through a project, then deep-dive from there
- Key areas to prepare: word embeddings/vectorization, transformer architecture (self-attention, encoder-decoder token generation), RAG evaluation and retrieval accuracy, fine-tuning concepts (PeFT, LoRA, QLoRA, quantization), and agentic AI frameworks (LangChain tool calling, LangGraph vs CrewAI differences)
- The instructor emphasized: understand *why* RAG dominates over fine-tuning in production (99% of use cases are solvable with RAG)

### RAG Retrieval Accuracy
- The LLM itself does not validate whether retrieved chunks are relevant — it simply follows the prompt template with whatever context is provided
- Interviewers frequently ask how to evaluate and ensure the retrieval mechanism returns correct information
- This is a "classic" interview question to prepare for

### LinkedIn Automation Strategy
- Spend 15 minutes per week generating seven posts using AI (two on technical topics, two on generic AI topics, three lighthearted but AI-related)
- Posts should start with a hook and end with a question
- Use AI image generation for accompanying visuals, then schedule posts via LinkedIn's native scheduler
- Can also be automated via n8n workflows

### Resume Best Practices
- Use a simple, text-heavy format with no infographics or multi-column layouts — AI parsers cannot read complex layouts
- Maximum three pages (industry standard for 10–15 years of experience)
- Use Google's XYZ format for bullet points (accomplished X, as measured by Y, by doing Z)
- Tailor the skills/keywords section for each job application
- The instructor emphasized: avoid "rage reviews" — fancy formatting causes ATS/AI screening failures

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| eraser.io | Tool for generating architecture diagrams for project READMEs |
| MinMax agent | Alternative tool for creating architecture diagrams |
| LangGraph | Framework for building agentic AI projects (mandatory project) |
| CrewAI | Framework for building agentic AI projects (mandatory project) |
| n8n | No-code automation platform; used for both AI and non-AI workflow projects |
| Google's XYZ format | Resume bullet-point style: accomplished X, measured by Y, by doing Z |
| ChatGPT / Groq | Used for resume scoring, post generation, and general prep |
| Streamlit / Flask | Mentioned as front-end options for project demos/screenshots |
| AWS Bedrock | Referenced in an example agentic AI auditing project |
| Gemma / TinyLlama / Llama 3.1 (2B) | Small models suggested for fine-tuning practice |

## Code & Examples
No code was demonstrated in this session. The instructor showed:
1. A GitHub repository example with structured files and a README for a "Dental Appointment Management System" (multi-agent LangGraph supervisor architecture)
2. A sample `report.pdf` structure: project title → problem statement → context → objectives → how it works → key inputs → architecture diagram → steps → screenshots
3. A live demo of prompting AI to generate seven LinkedIn posts and an accompanying image
4. A live demo of scoring a resume against a job description using a prompt that asks AI to create rubrics, score out of 100, and list missing keywords

## Q&A / Discussion Points
- No significant Q&A was captured in this session; the instructor invited questions but none were raised in the transcript.

## Action Items & Assignments
- [ ] Complete six projects by July 31, 2025: RAG, fine-tuning exercise, LangGraph agent, CrewAI agent, n8n data flow, n8n AI agent
- [ ] Push all projects to GitHub with proper README, `requirements.txt`, and `report.pdf`
- [ ] Include architecture diagrams (use eraser.io or MinMax agent)
- [ ] Include screenshots of any Streamlit/Flask front-ends
- [ ] Start weekly LinkedIn posting routine (15 min/week, 7 posts)
- [ ] Review class recordings for transformer and word embedding deep-dive content
- [ ] Download the agentic AI e-book when shared on Discord (within 1–2 days of session)
- [ ] Optionally send GitHub repository to instructor for review by July 31
- [ ] Reach out to instructor on LinkedIn for resume feedback or technical questions

## Open Questions / Unclear Spots
- [unclear: MinMax agent] — likely refers to a diagramming or AI tool, but the exact product name may be a transcription error
- The specific coupon code / free course offered at the end was not clearly identified in the transcript
- The "Google XYZ format" is a well-known framework, but the instructor's exact phrasing suggests he may also call it "Google's XYZ styling" interchangeably

## Flashcards

**Q: What are the six mandatory portfolio projects assigned?**
A: (1) Generative AI RAG project, (2) Fine-tuning exercise with a small model, (3) LangGraph agentic AI project, (4) CrewAI agentic AI project, (5) n8n non-AI data flow, (6) n8n agentic AI project.

**Q: Why does the instructor say fine-tuning is rarely needed in production?**
A: Because 99% of use cases are solvable with RAG. Fine-tuning is only needed when RAG fails, which the instructor says essentially never happened in his seven or eight production deployments.

**Q: In a RAG pipeline, does the LLM validate whether retrieved chunks are relevant?**
A: No. The LLM simply follows the prompt template with whatever context is passed to it. Retrieval accuracy depends entirely on the embedding/search algorithm, not the LLM.

**Q: What resume format does the instructor recommend and why?**
A: A simple, text-heavy format with no infographics or multi-column layouts, because AI/ATS parsers cannot reliably extract information from complex visual formats.

**Q: What is Google's XYZ format for resume bullet points?**
A: A structure where each point states what you accomplished (X), how it was measured (Y), and how you did it (Z). Example: "Developed and deployed an agentic AI auditing solution, reducing manual audit time by X%, by leveraging AWS Bedrock and multi-agent orchestration."

**Q: How can you use AI to evaluate your resume against a specific job?**
A: Paste the job description and your resume into an AI tool, ask it to create rubrics as a career expert, score the resume out of 100, and list missing keywords — then add those keywords before applying.

**Q: What is the instructor's recommended LinkedIn posting strategy?**
A: Spend 15 minutes per week using AI to generate 7 posts (2 technical, 2 generic AI, 3 lighthearted but AI-related), each starting with a hook and ending with a question, with AI-generated images and scheduled via LinkedIn.

**Q: How should interview preparation differ for agentic AI roles vs. textbook study?**
A: Interviewers ask you to walk through projects and then deep-dive from your answers rather than asking direct textbook questions. Having completed real projects makes preparation almost automatic.
