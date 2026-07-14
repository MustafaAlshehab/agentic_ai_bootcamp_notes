# APIs vs. Open Source Models: Ollama Setup and Local RAG

## TL;DR

This session opened with a deep RAG recap (chunking → embeddings → vector index → retrieval → LLM generation), then shifted to how large language models are accessed: paid public/private APIs versus free local open-source stacks. The main hands-on focus was **Ollama**: installing it, choosing models by parameter size and RAM, understanding quantization, and wiring **LangChain `ChatOllama`** into Jupyter and a Streamlit RAG chatbot that previously used an API model. Local inference is free but slow on modest hardware; the instructor compared **Llama 3.2 3B** vs **TinyLlama** on a PDF Q&A task and used **Groq** only as a quick rubric-based evaluator.

## Topics Covered

- Quick recap: generative AI, LLMs, and full RAG pipeline architecture
- RAG limitations (chunking, embeddings, retrieval, document quality); Excel/tabular chunking strategies
- Decoder architecture as the generative LLM in the RAG diagram
- Landscape of LLM products (GPT, Gemini, Claude, DeepSeek, Llama, Qwen, Mistral, etc.)
- **APIs vs. open source** models; subtypes: public APIs, private cloud APIs, aggregators
- When to recommend each approach to clients (cost, data privacy, existing cloud)
- Embedding model cost (mostly free on Hugging Face; paid on some cloud services)
- **Hugging Face** vs **Ollama** as open-source platforms
- Live install and demo of **Ollama** (Windows/macOS/Linux)
- Ollama CLI: list, run, remove models; model catalog on ollama.com
- Model size notation (**B** = billion parameters), RAM guidance for 8–16 GB systems
- Break, then parameters explained via a small neural-network weight-count analogy
- **Quantization**: Ollama (quantized) vs Hugging Face (raw) file sizes and accuracy tradeoffs
- Why **Google Colab cannot run Ollama** (Ollama is local-only)
- Jupyter exercise: `ChatOllama` basic chat and streaming
- PDF-based RAG chatbot with Ollama on a research paper; performance on weak hardware
- Model comparison using Groq-generated rubrics (Llama 3.2 vs TinyLlama)
- Streamlit RAG app migrated from `ChatXAI` to `ChatOllama`; LangChain version mismatch fix
- Preview: assignments tomorrow; next weekend classes canceled (official announcement promised)

## Key Concepts Explained

### Retrieval-Augmented Generation (RAG)

- **What it is:** Retrieval-augmented generation combines (1) retrieval of relevant document chunks from a vector index, (2) augmentation of the LLM prompt with those chunks, and (3) generation of a natural-language answer.
- **Why it matters:** Lets an LLM answer questions grounded in your own PDFs, Excel files, and internal docs instead of only its training data.
- **Instructor emphasis:** ~99% of generative AI use cases can be solved with RAG; poor answers usually trace to chunking, embeddings, retrieval, or document quality—not RAG itself being weak.

### Chunking and vector index build

- **What it is:** Documents are split into sub-documents (chunks) via format-specific strategies (e.g., one PDF page = one chunk, or paragraph-level splits). Chunks are embedded and stored in a vector database or local vector index.
- **Why it matters:** Chunk size and strategy directly control what text retrieval can find.
- **Caveats:** Excel with tens of thousands of rows needs different strategies (row + header batches, row-per-chunk, Excel→JSON, OCR, etc.). Instructor noted building a first app is fast (“vibe coding” + Streamlit); tuning for best quality takes iteration.

### Query-time RAG flow

- **What it is:** User question → NLP preprocessing (cleaning, normalization, stop-word removal) → same embedding model as indexing → vector similarity search → top-k chunks + optional chat history → prompt template → LLM → UI response.
- **Why it matters:** This is the runtime path every RAG chatbot implements.
- **Instructor emphasis:** The final answer is **generated**, not copied word-for-word from retrieved phrases; demo on Groq showed paraphrased output from three retrieved sentences about an “Agentic AI” bootcamp.

### Conversation history and context window

- **What it is:** Prompt templates can pass **context** (retrieved chunks) and **history** (prior Q&A) so follow-ups like “Is there any discount?” stay coherent. Each LLM has a **context window** (token limit); when full, older turns drop off.
- **Why it matters:** Without history, each message is treated as a new standalone query.
- **Instructor emphasis:** Prefer short sessions; personally limits to ~3 turns then starts a fresh session with a summary— even though modern models have large windows.

### APIs vs. open source LLMs

- **What it is:** LLMs are accessed either through paid **APIs** (public vendors, private cloud like AWS Bedrock, or **aggregators** that expose multiple models) or **open source** stacks where weights run locally (**Ollama**, **Hugging Face**).
- **Why it matters:** Choice drives cost, latency, data residency, and compliance.
- **Decision rubric taught:** No sensitive data → public APIs (cheap). Strict privacy → open source (download/run locally). Cost-sensitive but OK with cloud → public. “Safe bet” on existing AWS/Azure → private APIs (e.g., Bedrock with Nova, Anthropic, etc.).

### Aggregators (e.g., Groq)

- **What it is:** Platforms that host/API-wrap many underlying models rather than being a single foundation model.
- **Why it matters:** Students often confuse the aggregator name with an LLM brand.
- **Caveat:** Transcript conflates **Groq** (inference API aggregator) with **Grok** (xAI model family). Groq is **not** an LLM; **Copilot** and **GitHub Copilot** are products built on GPT-family models, not LLMs themselves.

### Ollama

- **What it is:** A local tool to download and run open-weight models with a simple UI and CLI; models are **quantized** and free to run offline.
- **Why it matters:** Bootcamp exercises for local, privacy-preserving inference without API keys.
- **Instructor emphasis:** Install Ollama locally; restart may be needed; use **`ollama run <model>`** from the website; **`ollama list`**, **`ollama rm <model>`** for management. For **8 GB RAM**, use smaller models (e.g., TinyLlama); **16 GB** base class assumption—stay around **≤8B** parameters, avoid much larger sizes on 16 GB.

### Parameters (B = billions)

- **What it is:** Count of learnable weights in the neural network; larger **B** generally means more capacity and higher RAM/disk needs.
- **Why it matters:** Guides which model fits your machine (e.g., 3B vs 8B vs 70B+).
- **Instructor emphasis:** LLMs are ultimately large neural networks; a toy 16-weight diagram illustrates why billions of weights imply enormous graphs you cannot draw by hand.

### Quantization

- **What it is:** Compressed model weights (e.g., **Q4** ≈ 25% of raw size) so Ollama builds are much smaller than Hugging Face “full” checkpoints; same parameter count, reduced precision/storage.
- **Why it matters:** Explains why **Llama 3.2 3B** might be ~2 GB on Ollama vs ~6.4 GB on Hugging Face for the “same” model name.
- **Caveats:** Quantized models are faster/lighter but typically **less accurate** than raw Hugging Face weights. Deeper math deferred to the next class. Ollama models are quantized by default; naming like **Q4**, **Q8**, **Q2** affects size and quality.

### LangChain + Ollama integration

- **What it is:** Replace API chat classes (e.g., `ChatXAI`) with **`langchain_ollama`**’s **`ChatOllama`**, passing the exact local model name from `ollama list`; no API key required.
- **Why it matters:** Same RAG notebook/Streamlit patterns as prior weeks, but inference runs on localhost (`http://localhost:11434` implied by stack).
- **Caveats:** Must run Jupyter **locally**, not Colab. Version mismatches between `langchain-core` and `langchain-ollama` can break imports—upgrade both if errors occur.

### Evaluating local model outputs

- **What it is:** Instructor used **Groq** with a rubric prompt (factual correctness, completeness, relevance, clarity; scores out of 100) to compare two Ollama answers to the same PDF question.
- **Why it matters:** Illustrates that smaller models (TinyLlama) can score far lower on domain PDF Q&A.
- **Caveats:** One-off LLM-as-judge is not rigorous evaluation; full evaluation strategies come a later week. Mask model names (“model one / model two”) to reduce bias toward known stronger models.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| OpenAI GPT family (3.5, 4o, 4o mini, etc.) | Public API LLMs |
| Google Gemini (2.5, 3.1, etc.) | Public API LLMs |
| Anthropic Claude (Sonnet 3.5/3.7, Opus, etc.) | Public API LLMs |
| DeepSeek | Chinese LLM company/models |
| Meta Llama | Open weights; common on Ollama |
| Alibaba Qwen | Open weights |
| Mistral / Mixtral / Zephyr | Mentioned in passing |
| Groq | **Aggregator** for fast API access to multiple models; used for demos and rubric grading |
| xAI Grok | [unclear in transcript: instructor said “Groq is by xAI” — likely meant **Grok**] |
| GitHub Copilot / Microsoft Copilot | Products using GPT models underneath |
| AWS Bedrock | Private API route; Nova, Anthropic, others |
| Hugging Face | Hub for raw models, datasets, embeddings (often free) |
| Ollama | Local runner; quantized models; CLI + desktop app |
| LangChain | `ChatOllama` from `langchain_ollama` |
| FAISS | Vector index in Streamlit RAG app (rebuilt on run) |
| Streamlit | `streamlit run app_ollama.py` (approximate filename) |
| Groq Playground | Live RAG answer demo without full app |
| Jupyter Notebook | Local notebooks for Week 7 Ollama exercises |
| Google Colab | **Not compatible** with Ollama for this session |
| Anaconda Prompt | Shell used on Windows for Ollama/Jupyter |
| `pip install langchain_ollama` | Fix `ModuleNotFoundError: langchain_ollama` |
| `ollama run llama3.2` | Download/run model (example) |
| `ollama list` | List installed models |
| `ollama rm <model>` | Remove a model |
| TinyLlama 1.1B | ~600 MB; recommended for low-RAM practice only |
| Llama 3.2 (1B / 3B variants) | Primary class demo model (~2 GB for 3B) |

## Code & Examples

### RAG prompt template (conceptual)

```text
You are an intelligent AI system that understands various types of documents and their context.
Given a question, answer as a career coach for Codecademy.
The user question: {question}
Retrieved phrases (closest matches from the vector database): {chunks}
Answer concisely.
```

### Groq demo: generation from retrieved phrases (not full RAG code)

Instructor pasted the user question, three retrieved sentences (bootcamp date, instructor name, price), and a short-answer instruction; Groq paraphrased into ~two lines—illustrating **generative** output vs verbatim retrieval.

### Ollama CLI

```bash
# Install from https://ollama.com (OS-specific installer)
ollama run llama3.2          # pull and start interactive chat
ollama list                  # installed models
ollama rm tinyllama          # remove a model
```

Interactive example: `Who is Roger Federer? Answer in two lines.`

### LangChain + Ollama (Jupyter) — approximate from transcript

```python
# pip install langchain_ollama
from langchain_ollama import ChatOllama

llm = ChatOllama(model="llama3.2")  # use exact name from `ollama list`
result = llm.invoke("Who is Roger Federer?")
print(result.content)

# Streaming variant also demonstrated (chat stream)
```

PDF RAG notebook: reused prior-week code (load PDF → chunk → embed → vector store → prompt template → invoke). On the research paper, asking about Roger Federer correctly returned “not in document”; asking **“What is the objective of this paper?”** produced a substantive summary (voice pathology / ML).

### Streamlit RAG app migration — approximate

```python
# from langchain_ollama import ChatOllama
# llm = ChatXAI(...)  # comment out API model
llm = ChatOllama(model="llama3.2:latest")  # match `ollama list` tag

# streamlit run app_ollama.py
```

If import/runtime errors mention version mismatch:

```bash
pip install -U langchain-core langchain-ollama
```

(Exact pin versions were read from a Groq-assisted fix during class—upgrade both packages together.)

### Groq rubric evaluation (prompt pattern)

Attached two model answers to the same question (“What is the objective of this paper?”) and asked Groq to score each out of 100 with criteria: factual correctness, completeness, relevance/focus, clarity. **Llama 3.2** scored ~92; **TinyLlama** ~38, with notes that TinyLlama hallucinated autoencoders/labels not in the paper.

## Q&A / Discussion Points

- **RAG chatbot could not answer “author of research paper” (Gemini API key setup):** Likely chunking, embedding, retrieval, or document parsing—not RAG being inadequate. Excel and non-PDF formats need different chunk pipelines.
- **Where is decoder architecture in the diagram?** The LLM box in the RAG flow **is** the decoder-style generative model.
- **Can Hugging Face embeddings + Groq LLM be mixed?** Yes—components are interchangeable.
- **Are embedding models free?** Mostly yes on Hugging Face; cloud-hosted embeddings (e.g., Bedrock) cost money.
- **Why aren’t Groq evaluation scores trusted blindly?** Single LLM-judge pass can be biased; proper evaluation comes later; anonymize outputs as model one/two.
- **`langchain_ollama` not found:** Run `pip install langchain_ollama`.
- **Why not Colab for Ollama?** Ollama runs on the local machine; Colab cannot access it.
- **“Teach us ways to do it for free”:** Ollama is free but **time-consuming** on 8–16 GB RAM without strong GPU/vCPU.

## Action Items & Assignments

- [ ] Install **Ollama** on your machine (Windows/macOS/Linux) and confirm the llama icon in the system tray/menu bar
- [ ] Pull at least one model (`llama3.2` or **TinyLlama** if RAM ≤ 8 GB); share OS/RAM/GPU in class chat if asked
- [ ] Complete **Week 7 Ollama Jupyter** exercises locally (not Colab): `ChatOllama` chat, streaming, PDF RAG Q&A
- [ ] Reuse prior RAG code: swap `ChatXAI` → `ChatOllama`, align `model=` with `ollama list`, install `langchain_ollama`
- [ ] Run **Streamlit** RAG app variant with Ollama; fix LangChain version conflicts if needed
- [ ] Practice patience on local inference; use **TinyLlama** only for speed practice on weak machines
- [ ] Watch for **assignments announced at end of next session**; **next weekend classes canceled** (official notice promised tomorrow)

## Open Questions / Unclear Spots

- [unclear: transcript says “Groq is by xAI” and “Groq 4, Groq 3” — likely **Grok (xAI)**, not **Groq (groq.com)**]
- [unclear: instructor said Llama 3.2 docs mention **8B** while the UI showed **1B and 3B** variants — follow `ollama list` / model card on ollama.com for your install]
- [unclear: DeepSeek parent company — ByteDance vs standalone DeepSeek]
- [unclear: Mistral ownership — not stated]
- [unclear: “Opus” as separate from Claude — clarified as Claude Opus variant]
- Quantization theory deferred to **next class** (full treatment tomorrow)
- Formal **evaluation** methodologies deferred to a later week
- Streamlit demo was still loading at session end; students told to finish asynchronously with same file edits
