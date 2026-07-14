# Week 6: Generative AI Deep Dive — LLMs, RAG, Vector DBs, and Chunking

## TL;DR

This session introduces generative AI as decoder-only transformer architectures (GPT family), surveys major LLM providers and their limitations, and positions **Retrieval-Augmented Generation (RAG)** as the primary way to ground models on external knowledge and reduce hallucinations. The instructor walked through the full RAG pipeline (chunking → embeddings → vector storage → similarity retrieval → prompt + LLM), plus chunking parameters, vector databases vs. local indexes, and several real-world RAG use cases. Practical live coding (research-paper RAG bot) is planned for the next session.

## Topics Covered

- Week 6 agenda: generative AI concepts today; practicals tomorrow; open-source models and first RAG assignment next week
- Recap: full transformer (encoder + decoder); split into encoder-only (BERT family) vs. decoder-only (GPT)
- Generative AI definition: decoder-only; input is mostly **prompts**
- Large language models (LLMs): pretrained on large public text corpora
- LangChain as a Python framework (like PyTorch/TensorFlow)
- Major LLM vendors and model families
- Problems/limitations of LLMs (class discussion + instructor detail)
- Hallucination: definition, Vodafone billing scenario, research-paper example
- RAG (Retrieval-Augmented Generation): architecture and motivation (99% vs. 1% fine-tuning)
- Text chunking: `chunk_size`, `chunk_overlap`, and why overlap matters
- Scalar vs. vector data; why vector databases exist
- Prompt templates for RAG use cases
- How relevant chunks are fetched (vectorization + similarity scores; cosine similarity)
- Evolution of GenAI capabilities (NLU → structured output → reasoning → agents)
- Vector index (local `.index` / FAISS) vs. vector database (e.g., Pinecone)
- RAG application examples and vote for tomorrow’s demo
- Instructor platform signup, credits, and feedback request

## Key Concepts Explained

### Generative AI and decoder-only models

- What it is: Generative AI centers on the **generative** (decoder-only) part of the transformer stack—input (typically a prompt) goes in, text output comes out.
- Why it matters: This is the architectural basis of GPT-style LLMs and most modern chatbots.
- Caveats: Contrasts with encoder-only models (e.g., BERT) used for understanding tasks; researchers split encoder/decoder to specialize (BERT ~2017–2022 era; GPT became widely known via ChatGPT).

### Large language models (LLMs)

- What it is: Pretrained **language** models trained on large amounts of text (Wikipedia, Reddit, other public sources).
- Why it matters: They are the core engine behind generative AI applications in this bootcamp.
- Caveats: “Large” refers to scale of pretraining data and model size, not a single vendor.

### LangChain

- What it is: A **Python framework** for building generative AI apps—analogous in role to PyTorch or TensorFlow, but aimed at LLM workflows.
- Why it matters: Students will use it in upcoming practicals; instructor will introduce it hands-on in the next session.
- Caveats: Installation/use described as simple; deeper coverage deferred to tomorrow.

### LLM limitations

- What it is: Common failure modes when using LLMs alone.
- Why it matters: Motivates RAG, guardrails, agents, and careful prompting.
- Instructor/student list: **hallucination**, **bias**, **limited context window**, **outdated knowledge**, **toxicity/sensitive outputs**, **no built-in tool/automation** (agents add tools externally), **lack of real-time memory**, **security/privacy** risks.
- Caveats: Context window means long chats can “forget” earlier instructions; outdated knowledge depends on model training cutoff (example given: trained through ~Jan 10, 2026); toxicity partly addressable with **guardrails**.

### Hallucination

- What it is: The model answers confidently even when the answer is wrong, unrelated to prior context/instructions, or fabricated.
- Why it matters: Unsafe for domain-specific or factual tasks (billing, medical, research papers) without grounding.
- Instructor emphasized: Hard to detect without **domain expertise** (e.g., math expert spotting wrong math); generic ChatGPT questions on niche papers can yield plausible but fake answers; **external knowledge** (docs, DBs) is the fix path leading to RAG.

### External knowledge and RAG

- What it is: **Retrieval-Augmented Generation**—ground the LLM with retrieved documents/data so answers come from provided sources, not only parametric memory.
- Why it matters: Instructor stated ~**99%** of generative AI use cases can use RAG; ~**1%** may need **fine-tuning** instead.
- Caveats: RAG does not eliminate all errors; good prompts and retrieval quality still matter; “agent” is not the right term for a simple research-paper bot—prefer names like “Research Paper Bot.”

### RAG pipeline (end-to-end)

- What it is: (1) Source documents (e.g., PDFs) → (2) **chunking** → (3) **embedding/vectorization** → (4) store in DB/index → (5) user question embedded → (6) **retrieve** top similar chunks → (7) pass **user input + relevant chunks + prompt template** to LLM → (8) response to UI.
- Why it matters: Same architecture applies across use cases (research papers, insurance, medical reports, etc.).
- Instructor emphasized: Pay attention to this diagram—it generalizes to all RAG apps; user question is converted to embeddings with the **same algorithm** as document chunks.

### Chunking (`chunk_size`, `chunk_overlap`)

- What it is: Split long documents into smaller pieces for embedding and retrieval; **chunk_size** = tokens per chunk; **chunk_overlap** = shared tokens between adjacent chunks.
- Why it matters: Models/brains don’t process huge documents at once; overlap preserves continuity across chunk boundaries.
- Typical defaults mentioned: **chunk_size 1000**, **chunk_overlap 200** (treated as common starting points, not universally optimal).
- Caveats: Optimal values are problem-dependent; instructor noted strategies exist but often start with these hard-coded numbers.

### Embeddings

- What it is: Text converted to numerical **vectors** (example: OpenAI embeddings); chunks become points in high-dimensional space (~**1500 dimensions** per word/vector mentioned in similarity example).
- Why it matters: Enables similarity search between user query and stored chunks.
- Caveats: Already covered in prior NLP week; same embedding model should be used for ingest and query.

### Scalar vs. vector data and vector databases

- What it is: **Scalar** data fits traditional RDBMS (Oracle, MySQL); **vector** embeddings are large numeric data ill-suited for slow storage/retrieval in those systems.
- Why it matters: Vector DBs optimize storage size and **fast retrieval** at scale.
- Examples mentioned: MongoDB (vector capabilities), **AWS S3 vector**, **Pinecone**, **Cosmos**, **PGvector**; hundreds exist—instructor does not enumerate all.
- Caveats: Bootcamp exercise will store vectors in an index; **Cassandra** noted as not having this capability in instructor’s comment.

### Relevant information retrieval

- What it is: Compare query embedding to stored chunk embeddings via **similarity algorithms** (e.g., **cosine similarity**); rank chunks by score and pass top matches to the LLM.
- Why it matters: Determines which context the model actually sees.
- Caveats: Number of chunks retrieved can vary (2, 3, 4, etc.); only high-scoring chunks should be included (illustrated with cricket vs. Roger Federer / other topics example).

### Prompt templates (for RAG)

- What it is: Instructions telling the LLM how to behave (e.g., “expert research assistant,” use provided context, explain clearly).
- Why it matters: Reduces hallucination in document-specific apps; shapes tone and format.
- Instructor demonstrated generating a template by asking an LLM for a short, contextual prompt for a research-paper RAG bot; full live coding tomorrow.

### GenAI evolution (instructor’s framing)

- What it is: Stages—(1) NLU, (2) **structured output** (e.g., JSON), (3) **reasoning** (slower, step-by-step), (4) **AI agents** (LLM + tool access to act).
- Why it matters: Explains why newer models hallucinate less and why agents are separate from “plain” generative AI.
- Caveats: LLMs alone don’t call tools internally; external tool use + LLM = agent pattern.

### Vector index vs. vector database

- What it is: Both are called “indexes”; **local vector index** stores embeddings in files on disk (e.g., **`.index`**, **FAISS**—two files created on app start); **vector database** (e.g., Pinecone) hosts indexes in a managed service for production.
- Why it matters: **PoC/prototype** → local index is enough; **production** → move to vector DB; switching described as a few lines of code change once Pinecone index exists.
- Instructor emphasized: Read a referenced blog later for conceptual clarity; tomorrow’s demo will show local vs. Pinecone flow.
- Caveats: Local index requires **restarting/rebuilding** when PDF set changes (e.g., adding PDFs recreates indexes).

### Preventing hallucination (Q&A)

- What it is: Better base models (GPT-3.5 vs. newer GPT-5 class), **good prompts**, RAG with document-specific context, and prompt-template tuning for expected answers.
- Why it matters: Document-specific RAG apps can cut hallucinations substantially; generic web-scale queries still need user judgment.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Transformers (architecture) | Encoder + decoder; basis for BERT vs. GPT split |
| BERT / RoBERTa / ALBERT / DistilBERT | Encoder-only family (~2017–2022) |
| GPT / ChatGPT | Decoder-only; generative pretrained transformers |
| LangChain | Python framework for GenAI apps; practical intro next session |
| PyTorch / TensorFlow | Compared as general ML frameworks vs. LangChain’s GenAI focus |
| OpenAI | GPT models; OpenAI embeddings mentioned for vectorization |
| Meta Llama | Llama 3.1, 3.2, 3.3 |
| Google Gemini | Replaces older Bard-era models in discussion |
| AWS Nova | AWS LLM offering |
| xAI / Grok | Grok 3, 3.5, 4 mentioned |
| Mistral | Standalone LLM company |
| ChatGPT | Used for examples, prompt generation, and contrast (2023 vs. newer models) |
| OCR | Backend technique to extract info from uploaded bills (Vodafone scenario) |
| MySQL / Oracle | Traditional scalar databases |
| Pinecone | Vector database; demo index “Aga index”; practical tomorrow |
| FAISS | Local vector index files (`faiss.index` + companion file) |
| MongoDB | Listed among vector-capable storage options |
| AWS S3 vector | Vector storage option |
| Azure Cosmos | Vector database example |
| PGvector | Suggested by student; confirmed |
| Cosine similarity | Similarity metric for retrieval (also covered prior week) |
| Instructor’s platform | Sign up for credits; coupon ~100–200 credits tomorrow; feedback welcome |

## Code & Examples

### RAG architecture (conceptual flow)

```text
[PDF docs] → chunking → embedding → [vector DB / local index]
                                              ↑
[User UI] → user question → embed query ──────┘
                              ↓
                    similarity search → top-k chunks
                              ↓
        [prompt template] + query + retrieved chunks → LLM → answer → UI
```

### Vodafone-style grounded app (described steps, not live-coded)

1. Front-end: user details + upload bill/complaint documents.
2. Backend: OCR on bill; DB lookup (e.g., roaming, postpaid vs. prepaid routing).
3. Combine extracted + DB facts with LLM under controlled prompts → non-hallucinated, account-specific answers.

### Chunking walkthrough (instructor numeric example)

Parameters: `chunk_size = 5`, `chunk_overlap = 2` on token/word units.

```text
Text: "Hello. How are you? I am doing good. Roger Federer is a professional Swiss tennis player with 21 titles."

Chunk 1: Hello. How are you? I am          (5 units)
Chunk 2: are you? I am doing good. Roger   (overlap 2 from end of chunk 1)
Chunk 3: doing good. Roger Federer is a    (overlap 2)
... continues until end (6 chunks total in example)
```

Default-scale parameters mentioned for real projects: `chunk_size=1000`, `chunk_overlap=200`.

### Prompt template generation (instructor used ChatGPT live)

Approximate intent of the generated template:

```text
You are an expert research assistant who produces world-class answers ...
Use the provided research paper context ...
Explain clearly for users who may not read the full paper ...
```

(Full wording was displayed on screen as “blah, blah” in transcript; exact text not fully captured.)

### Local index vs. Pinecone switch (described procedure)

1. Create index in Pinecone UI.
2. In code, replace local index store/calls with Pinecone index client.
3. Re-run ingestion so vectors flow to remote index.

*No complete application code was shared in this session; instructor cited “no practicals today.”*

## Q&A / Discussion Points

- **What problems do LLMs have?** — Students: hallucination, bias, limited context, outdated knowledge, toxicity; instructor added automation limits, memory, privacy.
- **Context window limitations?** — Model forgets earlier conversation after long chats; may answer when it should refuse due to lost context.
- **How to detect hallucination?** — Often requires domain expertise; dodgy answers can be cross-checked online; hard for non-experts (e.g., math).
- **Why chunk overlap?** — Shared tokens between adjacent chunks preserve continuity and help identify chunk boundaries (instructor’s explanation).
- **If you can’t see hallucination, can you prevent it?** — Better models, good prompts, RAG + tuned templates for document apps; general public models improved since 2023.
- **LangChain?** — Framework for GenAI apps; covered practically tomorrow.
- **Vector index vs. vector DB?** — Local file-based index for PoC; managed DB for production; blog recommended for details.
- **Tomorrow’s practical choice?** — Vote leaned **research paper bot** (~10 min, easier); **automated insurance claim** (#3) more complex; **AI career coach** on Thursday doubt-clearing session.

## Action Items & Assignments

- [ ] Attend **tomorrow’s session**: live coding — **RAG-based research paper chatbot** (LangChain intro).
- [ ] **Thursday** doubt-clearing: instructor to demo **AI Career Coach** RAG example.
- [ ] **Next week**: open-source models, advanced topics, then **first assignment — RAG-based chatbot**.
- [ ] Sign up on **instructor’s platform** today (~10 credits); explore features; expect **coupon code tomorrow** (~100–200 credits).
- [ ] Provide **feedback** on instructor’s platform (bugs, missing features, UX).
- [ ] Read instructor-recommended **blog** on vector index vs. vector database (link not spoken in transcript).
- [ ] Bootcamp exercise (upcoming): store vector data in a **vector database** / index.

## Open Questions / Unclear Spots

- [unclear: exact name/URL of instructor’s platform and the vector index vs. database blog]
- [unclear: instructor said “Narendra **Singh** Modi” — likely meant Narendra Modi; used only as similarity-demo filler text]
- [unclear: “Redshift or something” as vector DB — may be a misremembered product name]
- [unclear: second FAISS local filename/extension — instructor recalled two files but not the second extension]
- Transcript typo: user question converted to a “**Word** document” — almost certainly **word embedding**, not Microsoft Word.
- Transcript typo: “PT” / “Generating PT” — means **prompt template**.
- Whether PoI.com query about a real paper vs. fabricated title was fully resolved live (instructor noted answer looked correct but could not verify model had the paper).
