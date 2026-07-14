# Building RAG-Based Applications (Week 6)

## TL;DR

This session revisited the full RAG pipeline (split → embed → index → retrieve → prompt → LLM), then answered design questions about embeddings, chunking, vector databases, and LLM selection. Students ran hands-on LangChain + Groq exercises in Google Colab (basic chat, single-PDF context, multi-file directory chat without a vector index), then built an end-to-end **Streamlit RAG chatbot** with chunking, Hugging Face embeddings, and a local **FAISS** index using vibe coding. Evaluation of RAG systems was introduced at a high level; a dedicated metrics class is planned for week 8.

## Topics Covered

- Session agenda: RAG-based applications and RAG use cases
- RAG recap: purpose (reduce hallucination on custom data), step-by-step pipeline
- Instructor “open questions” on RAG architecture (embeddings, splitting, vector DB, LLM choice, evaluation, retrieval)
- Embedding algorithms: traditional vs modern; selection criteria
- Why document and query embeddings must use the same model
- Vector database / local index choice
- Document splitting techniques (page-based, fixed-length, sentence/paragraph-aware, layout-aware)
- LLM landscape: APIs vs open source; public, private, aggregators; budget-driven model picks
- Basic RAG chatbot evaluation (metric comparison + LLM-as-judge)
- Retrieval via similarity search (brief)
- Google Colab setup: copy notebook, Groq API keys (temporary, class-only)
- Practical 1: `pip install`, LangChain vs LangGraph vs CrewAI; basic `ChatGroq` invoke and streaming
- Practical 2: PyPDF loader + prompt template restricting answers to document context
- Practical 3: directory-based multi-PDF chat (still no vector store)
- Break (~8 minutes)
- Vibe coding a Streamlit RAG app: upload PDF → chunk → embed → FAISS local index → Q&A
- Walkthrough of generated `app.py` (Streamlit UI, FAISS folder, retriever `k=6`)
- Suggested use cases (e.g., resume vs job description scorer)
- Wrap-up: Drive uploads, API key hygiene, upcoming assignment and offline models (next class)

## Key Concepts Explained

### Retrieval-Augmented Generation (RAG)

- **What it is:** A pattern where user questions are embedded, similar chunks are retrieved from a vector store, and an LLM answers using retrieved context plus a prompt template—not from parametric memory alone.
- **Why it matters:** Reduces hallucination on proprietary or domain-specific documents; grounds answers in your data.
- **Emphasized:** RAG was framed as invented so LLMs “will not hallucinate anymore” on custom datasets (instructor’s simplification). Pipeline order: **split → embed/store → (on query) embed question → retrieve → pass chunks + prompt to LLM → chat completion**. LLMs are not involved until the query phase.

### Document splitting (chunking)

- **What it is:** Breaking large documents (e.g., PDFs) into smaller retrievable pieces before embedding.
- **Why it matters:** Smaller chunks improve retrieval precision and “understanding,” analogous to memorizing a few sentences vs an entire Wikipedia page. Instructor noted the **primary** reason taught here is better understanding; **context window** may also be a factor.
- **Emphasized:** Common production setting: **800–1,000 tokens** with **~200 token overlap**. Demo used **recursive character text splitter** with **chunk size 1,000** and **overlap 200**.

### Embeddings

- **What it is:** Converting text chunks (and queries) into dense vectors via an embedding model (e.g., OpenAI, Hugging Face, AWS Titan on Bedrock).
- **Why it matters:** Enables similarity search to find relevant chunks for a question.
- **Emphasized:** Traditional NLP vectorizers (Bag of Words, TF-IDF, Word2Vec—Google News Word2Vec cited as **300 dimensions**) vs modern embeddings (OpenAI **1,500 / 3,000** dims, Hugging Face variants, higher-dimensional models). **OpenAI embeddings are not Word2vec.** In 2026, instructor often picks **whatever is available** rather than spending heavy time on embedding bake-offs. Criteria when choosing: **multilingual** embeddings for mixed-language docs; **domain-specific** (e.g., finance) from Hugging Face if the use case matches training data (example model: **768 dimensions**, trained on sources like Stack Exchange, GoIQ, MS MARCO—verify fit before use).

### Same embedding for documents and queries

- **What it is:** The embedding model used at index time must match the model used to embed the user question.
- **Why it matters:** Different models/dimensions live in different vector spaces; retrieval will fail or be meaningless if mismatched.
- **Emphasized:** If documents are **1,500-dimensional**, the question must be embedded into the **same 1,500-dimensional** space.

### Vector storage: local index vs vector database

- **What it is:** Embedded chunks stored either on local disk (**vector local index**) or in a managed **vector database** (**vector database index**).
- **Why it matters:** POC/low budget → local; production/scale/budget → hosted vector DB.
- **Emphasized:** Choice driven by **budget** and **employer policy** (e.g., AWS → Redshift or S3 Vector; Azure → Cosmos DB; third-party OK → **Pinecone**). “All are same”—compare **cost** and use what you know.

### Splitting techniques

- **What it is:** Strategies to create chunks from large documents.
- **Why it matters:** Bad splits break tables, chapters, or semantics and hurt retrieval quality.
- **Types discussed:**
  - **Page-based:** ~one chunk per PDF page (plus overlap); easy but risky for tables split across pages, chapter boundaries, column layout issues.
  - **Fixed-length (token-based):** Most commonly used in practice (800–1K tokens, 200 overlap).
  - **Sentence / paragraph / heading-aware:** Respects linguistic boundaries.
  - **Layout-aware / table-aware:** Mentioned; instructor has **not** used table-aware.
- **Emphasized:** Default recommendation in class: **fixed-length chunking**, not page-based.

### LLM selection for RAG

- **What it is:** Choosing API or open-source models (OpenAI, Anthropic, DeepSeek, Llama via Hugging Face/Ollama; private clouds AWS/Azure/GCP; aggregators like **Groq**).
- **Why it matters:** Cost, latency, privacy, and answer quality vary by model and deployment.
- **Emphasized:** Budget-sensitive clients → smaller/cheaper models (examples: **GPT-4o mini**, **Claude Sonnet 3.5**, **DeepSeek R1**). **Ollama** (quantized models) covered in a **future class**. For insensitive data, public APIs may suffice.

### RAG evaluation (preview)

- **What it is:** Comparing model answers to a labeled Q&A set using metrics (e.g., **faithfulness**, **correctness**) or using an **LLM-as-judge** to rank multiple model outputs.
- **Why it matters:** Picks the best LLM for your RAG stack before production.
- **Emphasized:** Parallel to ML train/test: hold out Q&A pairs, get answers from multiple models, score per question, aggregate winners. LLM-assisted eval: pass anonymized answers (LLM1, LLM2, LLM3) to an expert-judge prompt; demo used **Groq** to score three fake answers to “Who is Roger Federer? Answer in two lines” (winner scored **94/100**). Full metrics class in **week 8**.

### Retrieval

- **What it is:** Finding nearest neighbor vectors in the index for the query embedding.
- **Why it matters:** Quality of retrieved chunks bounds answer quality.
- **Emphasized:** Based on **similarity algorithms**; demo retriever used **`k=6`** (six nearest neighbors).

### LangChain vs LangGraph vs CrewAI

- **What it is:** **LangChain** models sequential “chains” (task1 → task2 → …), typical for generative AI pipelines. **LangGraph** models non-linear **graphs/trees** for agents. **CrewAI** is a higher-level agent framework.
- **Why it matters:** Right abstraction reduces code size and complexity for agentic workflows.
- **Emphasized:** Agents are **non-linear**; LangGraph recommended over LangChain for agents (~200 vs ~100 lines). CrewAI can be ~50 lines for similar work. Course order: **LangChain → LangGraph → CrewAI**.

### “Simple RAG” vs full RAG in notebooks

- **What it is:** Early exercises passed **full PDF text** as `context` directly to the LLM—**no** embedding, **no** vector index.
- **Why it matters:** Illustrates prompt-grounding and “I don’t know” behavior; does not scale to large corpora.
- **Emphasized:** Later Streamlit app implements **true** RAG: chunk → embed → **FAISS** → retriever.

### Prompt templates for grounded chatbots

- **What it is:** Instructions that inject `context` + `question` and restrict the model to document-only answers.
- **Why it matters:** Prevents the model from using general world knowledge when you want document-only behavior.
- **Emphasized:** Adding lines like “if not in context, say I don’t have enough information…” made **“Who is Roger Federer?”** correctly return no answer when the PDF was unrelated. Prompt injection to override template was raised; instructor said **unlikely** but if it works, harden prompt (e.g., ignore user-supplied persona).

### Chatbot latency

- **What it is:** End-to-end time to generate a response.
- **Why it matters:** UX for production chatbots.
- **Emphasized:** **Very important**—depends on multiple factors in the stack.

### Streamlit for POC UIs

- **What it is:** Python framework to build web UIs without HTML/CSS (alternative: Flask, which needs front-end skills).
- **Why it matters:** Quick demos for RAG and ML projects.

### Vibe coding for RAG apps

- **What it is:** Using an AI assistant (e.g., Groq chat) with a detailed natural-language spec to generate a full app (`app.py`).
- **Why it matters:** Rapid POC; may need iteration for LangChain version compatibility.
- **Emphasized:** Two strategies for document chatbots: **(1)** fixed PDF trained/indexed in backend vs **(2)** user uploads PDF in frontend and index is rebuilt per upload (demo used **strategy 2**). **FAISS index recreated on every new PDF upload** and on code restart.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| LangChain | Framework for sequential generative AI chains; `langchain_openai`, `langchain_huggingface`, classic chains |
| LangGraph | For non-linear agent graphs; recommended over LangChain for agents |
| CrewAI | Higher-level agent framework; less code than LangGraph for some tasks |
| Groq / ChatXAI | API used in class (`ChatGroq` / `chatxai`); temporary shared API keys for session only |
| OpenAI | `ChatOpenAI`, embeddings; swap in place of Groq per instructor snippet |
| Hugging Face | Embeddings hub; domain models (e.g., finance); `sentence-transformers/all-MiniLM-L6-v2` in Streamlit app |
| PyPDF loader | Load PDF pages into documents |
| RecursiveCharacterTextSplitter | Chunk PDFs (1000 / 200 in app) |
| FAISS | Facebook AI Similarity Search; local `faiss_index` folder; `similarity_search` with `k=6` |
| Streamlit | `st.set_page_config`, `st.title`, sidebar for API key, file upload UI |
| Google Colab | Week 6 notebook; copy to Drive, don’t run until instructed |
| Spyder / VS Code / Cursor | Local `.py` development for Streamlit app |
| Pinecone | Third-party vector DB when allowed |
| AWS Bedrock (Titan) | Embedding option when AWS is required |
| Azure Cosmos DB | Mentioned for Azure shops |
| AWS Redshift / S3 Vector | Mentioned under AWS policies |
| Bag of Words, TF-IDF, Word2Vec | Traditional embeddings; Word2vec called “best” of the three traditional |
| OpenAI embeddings | Not Word2vec; 1500/3000 dim variants |
| GPT-4o mini, Claude Sonnet 3.5, DeepSeek R1 | Example budget-friendly API models |
| Ollama | Quantized local models; dedicated class next week |
| Open Router | Mentioned as alternative API routing |
| `pip install` | First Colab cell; pip dependency resolver warnings called ignorable/common |
| Gemini (free API) | Suggested for post-class practice if converting code via AI |
| Tableau (sample PDF) | Used to demo index swap when changing uploaded document |

## Code & Examples

### RAG pipeline (conceptual)

```text
Documents → Split → Embed (E1) → Store (local FAISS or vector DB)
User question → Embed (same E1) → Similarity search → Top-k chunks
Chunks + Prompt template → LLM → Answer
```

### Prompt pattern (Colab: document-only answers)

Approximate structure from the session:

```python
# context = concatenated PDF page text
# question = user query
prompt_template = """
You are an intelligent document assistant.
Answer based only on the following context.
If the answer is not clearly contained in the context, say:
"I don't have enough information in the document to answer this question accurately."

Context: {context}
Question: {question}
"""
```

### Switch Groq → OpenAI (instructor guidance)

```python
# from langchain_groq import ChatGroq  # class used Groq
from langchain_openai import ChatOpenAI

chat = ChatOpenAI(model="gpt-4o-mini", api_key="YOUR_OPENAI_KEY")
response = chat.invoke("Who is Roger Federer? Answer in two lines.")
```

### Basic Groq invoke + streaming (conceptual)

```python
# After setting XAI_API_KEY / Groq key
response = chat.invoke("Who is Roger Federer? Answer in two lines.")
# Streaming variant prints token-by-token (second exercise)
```

### Streamlit RAG app flow (reconstructed from walkthrough)

```python
# 1. User uploads PDF in Streamlit sidebar / main UI
# 2. PyPDFLoader loads file from path
# 3. RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
# 4. HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2")
# 5. FAISS.from_documents(chunks, embeddings) → save to local "faiss_index/" folder
# 6. On new PDF: delete/recreate index folder
# 7. retriever = vectorstore.as_retriever(search_kwargs={"k": 6})
# 8. ChatGroq + RetrievalQA-style chain with prompt template → answer
```

Run locally (approximate):

```bash
pip install streamlit langchain langchain-groq langchain-huggingface langchain-core faiss-cpu sentence-transformers pypdf
streamlit run app.py
```

### Vibe-coding prompt (paraphrased spec given to Groq)

- Build a **RAG chatbot** where the user **uploads a PDF in the frontend**
- Use **PyPDF loader**, **chunk**, **Hugging Face embeddings**, store in **local FAISS** under a `files`/`faiss_index` folder
- **Recreate indexes** on every restart and every new PDF upload
- User asks document-related questions; answers grounded in uploaded PDF
- **Streamlit** UI; LLM strictly **ChatXAI / Groq**

### Evaluation demo (LLM-as-judge, conceptual)

```text
Provide 3 anonymized answers (LLM1, LLM2, LLM3) to the same question.
Ask judge LLM: prepare rubrics, score /100, declare winner.
Example outcome: LLM3 = 94 (best two-line Roger Federer answer).
```

## Q&A / Discussion Points

- **Why split documents?** Smaller chunks aid understanding/retrieval; context window may also matter.
- **Splitting criteria / techniques?** Covered: page-based, fixed-length, sentence/paragraph-aware, layout-aware.
- **Best embedding algorithm?** In 2026, many are good; pick by availability, multilingual needs, or domain-specific Hugging Face models after checking training data.
- **Is OpenAI embedding Word2vec?** No—different from traditional algorithms.
- **Why same embedding for docs and queries?** Must share vector space/dimensions for similarity search.
- **Which vector DB?** Familiarity + company policy + cost (Pinecone, Redshift, Cosmos DB, S3 Vector, local FAISS).
- **How to evaluate RAG / pick LLM?** Labeled Q&A + faithfulness/correctness metrics, or LLM-as-judge; details in week 8.
- **How does retrieval work?** Similarity-based (already covered prior day).
- **Colab pip dependency warning?** Common version noise in GenAI; often ignorable or fix with AI help.
- **Chatbot response time?** Yes—latency matters; depends on multiple stack factors.
- **Groq “model at capacity” errors?** Transient Groq-side issue; retry.
- **AdaBoost yes/no from ML book + research paper directory chat?** Initially wrong/unavailable; later “no” for random subset claim; instructor noted answer may be incorrect.
- **Can user question override system prompt template?** Student tried; instructor skeptical but suggested hardening prompt if injection works.
- **Hundreds of documents without loading all into context?** Need vector index (shown after break)—earlier exercises intentionally skipped vectors.
- **Where to get API key for later practice?** Paid Groq (~$5–10), free Gemini (convert code with AI), or wait for **offline/open-source models** next class.
- **Break / holiday next week?** Weekend class continues next week; possible break later—wait for official notice.
- **Negative session feedback?** Instructor asked dissatisfied students to contact Codecademy directly.

## Action Items & Assignments

- [ ] Open the shared **Google Colab** link, **save a copy to Drive**, do not execute ahead of instructor (initial setup).
- [ ] Replace API key in notebook with your own **Groq** key (class keys deleted after session) or convert to **OpenAI** / **Gemini** using AI-assisted rewrites.
- [ ] Complete Colab exercises: installs, basic `invoke`/stream, single-PDF grounded chat, optional **directory-based multi-PDF** folder exercise.
- [ ] Build or run the **Streamlit RAG chatbot** (`app.py` pattern): PDF upload → FAISS index → Q&A; optional prompt tweaks via AI.
- [ ] Optional practice: **resume + job description** scoring app (rubric, score /100, strengths/weaknesses)—instructor said resume example is practice only; **formal assignment coming**.
- [ ] Download shared research paper / materials from Drive when posted (or use your own documents).
- [ ] If investing in APIs: ~**$5–10** on Groq suggested; or wait for next class on **offline models** (no keys).
- [ ] Use AI to explain line-by-line vibe-coded output if needed; ask instructor for **block-level** explanations.

## Open Questions / Unclear Spots

- Instructor was uncertain whether **context window** or **comprehension** is the deeper reason for chunking—both were mentioned.
- **“Redshift”** was listed alongside vector options (may mean a specific AWS vector offering or was conflated with another service—verify against your cloud docs).
- **“Lancashire core.memory”** in transcript was likely **`langchain_core.memory`** (ASR error) during live debugging.
- **“Chat XAI”** vs **Groq** naming in code—treat as Groq’s LangChain integration used in class.
- Exact **assignment spec and deadline** were not given in this session (“I will be giving you an assignment”).
- **Table-aware** and unused splitting variants were named but not demonstrated.
- Fine-tuning was mentioned as a **future topic** with only a brief aside today.
