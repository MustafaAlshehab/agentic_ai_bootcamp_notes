# Gen AI Foundations: Tokenization, Embeddings, Context Windows, RAG, and LLM Options

## TL;DR
This session builds on Day 1’s transformer recap and walks through core generative-AI building blocks: how prompts become tokens and embeddings, why long chats hit context limits, and how chunking plus vector databases enable RAG pipelines. The instructor framed ~90% of industry Gen AI use cases as RAG-solvable, contrasted LLM deployment options (open source vs APIs), and ran a breakout exercise to design a website + Q&A chatbot architecture. Mostly theory, with live demos via Grok and a generated RAG prompt template.

## Topics Covered
- Schedule clarification (Sat/Sun regular classes, Thursday doubt-clearing; Discord `#doubts`, tag Nazia)
- Recap: transformers (encoder/decoder), prompts as LLM input, generated (not verbatim) outputs
- Tokenization and tokens (words vs sub-words)
- Word embeddings / vectorization (categorical → numerical)
- Context window, forgetting in long chats, and hallucination
- Chunking and chunking overlap (Wikipedia translation analogy)
- Student question: “think step-by-step” prompting for complex tasks
- RAG (Retrieval Augmented Generation): use cases and end-to-end pipeline
- Vector databases vs traditional scalar DBs; similarity-based retrieval (preview)
- LLM landscape: major providers and model families
- LLM serving categories: open source (Ollama, Hugging Face) vs APIs (standalone/public, aggregators, private cloud)
- Aggregators for PoC only; production → private APIs or open source
- Breakout exercise: ZipAnalytics.com chatbot architecture (Q&A first, then scraped site, out-of-scope guardrails)
- Instructor solution walkthrough; SLM vs LLM discussion; Office 365 / Copilot agent question
- Wrap-up: next weekend ML/DL basics; Thursday assignment review

## Key Concepts Explained

### Tokenization
- Splitting natural-language input (a prompt) into **tokens**—units that can be whole words or sub-words (e.g., “running” may split into two tokens depending on the tokenizer).
- **Why it matters:** Everything downstream (embeddings, context limits) is measured in tokens, not characters.
- Instructor noted specific tokenization schemes rarely change real-world outcomes much; focus on the concept, not picking algorithms.

### Tokens
- The pieces produced by tokenization; e.g., “Who is Roger Federer? Answer in two lines.” might be ~8 word-level tokens.
- **Why it matters:** Models consume and bill by tokens; context windows are token budgets.

### Word embeddings / vectorization
- Converting categorical/text tokens into **numerical vectors** so models can process them.
- **Why it matters:** LLMs and retrieval systems operate on numbers, not raw text.
- Dedicated classes will explain *how* embedding values are computed; for now: “categorical → numerical.”

### How LLM chat responses work (high level)
- User prompt → tokenization → embeddings → model matches relevant internal knowledge (also stored as vectors) → **generated** output word-by-word.
- **Why it matters:** Output sentences are often **not** copied verbatim from training data; they synthesize contextually similar information.
- Example: GPT-3.5 trained through ~2021 may know fewer Federer titles than a newer model, but still composes an answer from related snippets.

### Context window
- The maximum token span an LLM can “remember” in one conversation (input + output across turns).
- **Why it matters:** Long multi-turn chats exhaust the window; the model forgets early turns and may **hallucinate** to fill gaps.
- Example cited: GPT-4 ~16k tokens—after large prior outputs, follow-ups may drop earlier context.
- **Instructor tips:** Keep sessions short (3–4 turns), start new chats per topic, summarize prior chat into a fresh session’s first message.

### Hallucination (context-driven)
- Model invents or drifts when it lacks reliable context—not only “making things up” in general, but often because the **context window** overflowed.
- **Mitigation discussed:** Shorter chats, new sessions, summarization handoff.

### Chunking and chunking overlap
- Splitting large documents (PDFs, web pages) into small segments (e.g., 2–3 lines) before translation/retrieval—mirrors how humans translate Wikipedia line-by-line with short-term memory.
- **Why it matters:** Essential for knowledge-base Gen AI (PDFs, Word docs, websites); whole-page one-shot processing fails for both humans and systems.
- **Chunking overlap:** [mentioned on agenda; not deeply elaborated in this session]

### RAG (Retrieval Augmented Generation)
- **Retrieval Augmented Generation:** retrieve relevant document chunks, then augment the LLM prompt with that context for generation.
- **Why it matters:** Instructor estimated **~90%** of industry Gen AI use cases are RAG-solvable; remainder needs more advanced techniques later.
- **Key distinction:** Domain documents are **not** retrained into the LLM; they live in a vector DB. The LLM only generates answers from retrieved context + prompt template.

### Bag of words (intro embedding example)
- Simple baseline: encode word presence/absence per sentence as binary vectors.
- **Why it matters:** Illustrates “text → vector”; full NLP class will cover proper embedding algorithms.

### Vector database
- Specialized DB for storing/querying **vector** (embedding) data with fast similarity search.
- **Why it matters:** Traditional scalar DBs (Oracle, MySQL, MS SQL) are poor for vector retrieval; some (MongoDB, Cassandra) now support vectors, but dedicated vector DBs are standard for RAG.
- Examples mentioned: **Pinecone**, MongoDB vector, AWS vector offerings, Supabase vector.

### Embedding algorithm consistency
- The **same** embedding model must be used at ingest and query time, or vector spaces won’t align and retrieval breaks.

### Similarity retrieval (preview)
- Input query is embedded; system scores similarity against stored vectors (e.g., 0.6, 0.4, …) and passes top matches to the LLM.
- **Advanced:** locality-sensitive hashing (LSH) and retrieval mechanisms—deferred; Pinecone docs suggested for curious students.

### Prompt template (in RAG)
- Developer-written system instructions (e.g., “behave as a research analyst”) plus injected **context** and **user question**.
- Demo: Grok generated a research-analyst template from a natural-language spec.

### LLM parameters
- Parameter count (1B, 8B, 40B, 200B, etc.) indicates model size/complexity; larger ≠ always best for every task.

### LLM deployment categories (instructor framework)
| Category | Examples | Pros | Cons |
|----------|----------|------|------|
| Open source | Ollama, Hugging Face | Free, best data privacy | Large downloads, storage, higher latency |
| Standalone public API | OpenAI, Anthropic direct | Cheap, low latency | Data privacy less reliable |
| Aggregators | Groq (platform) | One fee, many models—good for students/PoC | Extra broker cost; **not recommended for production** |
| Private cloud APIs | Azure, AWS, GCP | Reliable for enterprises already on cloud | Privacy “more reliable” than public, not as strong as on-prem open source |

### Small Language Models (SLM)
- Suggested in discussion for limited-domain bots; instructor cautioned **smaller context windows** may be insufficient for this use case.

### NLP similarity without vector DB (Q&A path)
- For structured FAQ tables, **cosine similarity** (and related NLP checks) on scalar DB text can answer without calling an LLM—cost-efficient when threshold match is strong.

### Step-by-step prompting (“chain-of-thought” style)
- Asking the model to reason step-by-step for math/complex tasks improves results by acting as **human-in-the-loop** guidance—you steer intermediate reasoning.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Grok (xAI) | Instructor’s preferred chat UI for demos |
| ChatGPT / OpenAI GPT | Front end vs GPT-3.5/4/4o backend; standalone public API |
| Groq | Mentioned as **aggregator** platform (multiple model APIs) |
| Gemini, Claude, Copilot, Perplexity, NotebookLM | Named by students; Copilot called “not great” for some uses |
| Transformers (architecture) | Encoder/decoder recap from prior session |
| Bag of words | Introductory embedding example |
| Pinecone | Vector DB example; official docs linked for LSH/retrieval curiosity |
| MongoDB, Cassandra | Some vector support noted |
| Oracle, MySQL, MS SQL | Traditional scalar DBs—not for vector workloads |
| AWS S3 | Raw/scraped document storage in breakout solutions |
| Ollama, Hugging Face | Open-source model distribution |
| Meta Llama, Alibaba Qwen, Anthropic Claude, Google Gemini, Amazon Nova, XAI Grok, DeepSeek, Mistral, Zephyr | LLM families listed |
| Azure / AWS / GCP | Private cloud API hosting |
| Roger Federer Wikipedia | Context-window / chunking teaching example |
| “Attention Is All You Need” paper | Example doc for research-paper chatbot |
| ZipAnalytics.com | Breakout scenario website |
| Discord `#doubts` | Tag **Nazia** for async help |
| Copilot / Copilot Studio | Office 365 agent creation—Studio needed for custom agents |
| Cosine similarity | FAQ matching without LLM generation |
| Locality-sensitive hashing (LSH) | Mentioned in passing for retrieval efficiency |

## Code & Examples

### Simplified RAG pipeline (conceptual flow)

```
PDF(s) → chunking → embedding (algorithm E1) per chunk → vector DB
                                                              ↑
User question → same embedding E1 → similarity search → top chunks
                                                              ↓
                    prompt template + {retrieved context, user question} → LLM → answer
```

- LLM is **not** involved until retrieval completes.
- Prompt template example (paraphrased from Grok demo): expert research analyst persona, synthesize academic papers, answer from provided context only.

### Bag-of-words toy vectors

Sentences: “The boy is good.” / “The girl is good.”

| Word   | Sentence 1 | Sentence 2 |
|--------|------------|------------|
| the    | 1          | 1          |
| boy    | 1          | 0          |
| is     | 1          | 1          |
| good   | 1          | 1          |
| girl   | 0          | 1          |

→ Vectors `11110` and `10111` (illustrative; instructor used a condensed token count).

### Context window accounting (example)

```
Turn 1: input 150 tokens, output 1,000 tokens
Turn 2: input + output grows (e.g., 13,000 token output)
Turn 3: model may “forget” turn 1–2 content when total exceeds ~16k (GPT-4 example)
```

### Breakout architecture requirements (ZipAnalytics chatbot)

1. Scrape website → store text (e.g., S3); optional scheduled/webhook re-scrape.
2. Client provides Q&A document.
3. On user query: search **Q&A first**; if miss, search **scraped site** vectors; if irrelevant, refuse (out of scope).
4. Decide LLM provider, vector DB, file storage, monitoring (tokens, relevance, Q/A logs for fine-tuning).

### Instructor’s preferred architecture (summary)

```
User → NLP similarity on scalar Q&A table (id, Q, A, score) → if score > threshold: return A (no LLM)
     → else: embed query → Pinecone (scraped content) → LLM + prompt template (in-scope only)
     → optionally cache new Q&A pairs from LLM answers to reduce future LLM calls
```

## Q&A / Discussion Points

- **Class schedule / daylight saving:** Regular classes Sat–Sun (Indian time); doubt-clearing **Thursday 8–9 PM IST**; unchanged since bootcamp start—confirm with Codecademy for timezone confusion.
- **Step-by-step vs direct math prompt:** Step-by-step generally yields better results; you guide/revise model reasoning (human-in-the-loop).
- **How is RAG different from training an LLM?** RAG does **not** put documents into the model weights; it retrieves chunks and uses the LLM only for framing the final answer.
- **How is data retrieved from vector DB?** Similarity scores across stored vectors vs query vector; algorithms and LSH covered in a future class; Pinecone blog for self-study.
- **Embedding algorithms:** Deferred to dedicated NLP/embedding sessions.
- **Supabase:** Confirmed as a vector DB option (instructor hasn’t used it).
- **Thinking/reasoning tokens:** Basics first; covered in later sessions.
- **Local agent with codebase access (e.g., Codex-like):** Acknowledged; distinct from RAG-with-static-docs pattern.
- **SLM for limited ZipAnalytics data:** Possible in theory; context window limits may block—open-source SLM not clearly sufficient here.
- **NLP tools on scalar DB for FAQ:** Store Q&A as text; apply similarity (cosine, etc.)—no separate vector store required for FAQ path.
- **Office 365 chatbot for ~150 staff without Copilot Studio:** Basic Copilot can’t share custom agents; need **Copilot Studio** or a **custom Python app** (one student used SharePoint/S3 folder + API token approach—contact Balaram on Discord).

## Action Items & Assignments

- [ ] Use Discord `#doubts` and tag **Nazia** for questions (not live class timing debates).
- [ ] Practice short AI sessions: 3–4 interactions, then new chat with a summary of prior context.
- [ ] Optionally read **Pinecone docs** on retrieval/LSH after class (not during class).
- [ ] Join **Thursday** doubt-clearing for assignment answer walkthrough.
- [ ] Instructor may review submitted assignments or share answer keys for self-validation—time permitting.
- [ ] Next **Saturday/Sunday:** more Gen AI topics; upcoming weekend also **ML/DL basics**.

## Open Questions / Unclear Spots

- **Chunking overlap:** Listed on the whiteboard agenda but not explained in depth this session.
- **Prompt engineering & deeper LLM topics:** Explicitly deferred to the second half (partially covered after break).
- **Exact embedding algorithms** for production RAG: postponed to future classes.
- **Thinking/reasoning tokens and recall impact:** Asked during break; promised for later curriculum.
- Student reference to **“SAD folder”** + Windows API token for Office 365 integration—likely SharePoint or similar; details not fully captured in transcript [unclear: exact Microsoft storage integration path].
- Instructor’s “two types” vs diagram showing **three** API subcategories (standalone/public, aggregators, private)—framework is instructor-specific, not from standard textbooks.
