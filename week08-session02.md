# RAG Evaluation: Multi-Stage Metrics, RAGAS, and Comparing Groq Models (Week 8)

## TL;DR

This session continued Week 8 work on **evaluating RAG pipelines**, with a recap of evaluation at every RAG stage (chunking, embeddings, retrieval, LLM) and three practical approaches: **LLM-assisted judging**, **traditional NLP metrics**, and **RAGAS**. The instructor walked through a Colab notebook (`rag-metrics-evaluation`) on a Power BI PDF, fixed lingering RAGAS issues, and compared **Groq 3** vs **Groq 4** on context recall, faithfulness, and factual correctness. Students also ran **BLEU/ROUGE** and briefly **BERTScore/BARTScore** on the same outputs. A live attempt to **automate multi-model RAGAS evaluation** failed at the end; the working manual notebook remains the main deliverable.

## Topics Covered

- Announcements (course coupon codes, channel video request) and quick recap for a student who missed the prior session
- **Where evaluation happens in a RAG pipeline** (chunking → embeddings → retrieval → LLM)
- Recap of yesterday: LLM-assisted evaluation, BLEU/ROUGE/METEOR/BART/BERT, RAGAS
- Status of the **RAGAS code fix** from the previous class
- Week 8 folder setup (`AGAI 3`, `31st May`) and Colab files: `rag-metrics-evaluation` (+ a second RAGAS notebook)
- End-to-end walkthrough: PDF load → chunk → embed → FAISS → Groq RAG → RAGAS metrics
- **Chunk overlap math** (10,000 tokens → 13 chunks with size 1,000 / overlap 200)
- **Hugging Face** `sentence-transformers/all-MiniLM-L6-v2` (384-dimensional embeddings)
- **Dependency fix**: `langchain-community < 0.4` for RAGAS compatibility
- Comparing **Groq 3 latest** vs **Groq 4 latest** with RAGAS
- **n-gram metrics** demo: ROUGE and BLEU over evaluation `results`
- **Model-based metrics**: BERTScore and BARTScore (install + score lists)
- Organizing multi-LLM comparison in **spreadsheets**; per-metric winners vs overall winner
- **BART vs BERT vs GPT** architecture (encoder-decoder vs encoder-only vs decoder-only)
- Student Q&A: evaluating **large, table-shaped outputs** (UK apprenticeship scheme-of-work RAG)
- Second notebook: RAGAS on **“Attention Is All You Need”** PDF; default RAGAS uses OpenAI unless you wrap your LLM
- Break, then **vibe-coding automation** of three Groq models in a loop (failed)
- Class wrap-up: assignment review promised; fix automation before doubt-clearing session

## Key Concepts Explained

### Evaluation at multiple RAG stages
- What it is: RAG quality can be judged at **chunking**, **embedding/vectorization**, **retrieval relevance**, and **final LLM output**—not only at the answer layer.
- Why it matters: Blogs and Google results usually focus on **Q&A test sets + retrieval + LLM output**; other stages are often tuned indirectly by checking final answers and swapping embeddings.
- Instructor emphasis: **LLM-layer evaluation is the “game changer”** in practice; embedding choice is often “any strong embedding” (e.g. 1,500–3,000 dimensions) unless you have time to benchmark.

### LLM-assisted evaluation
- What it is: Build a **Q&A evaluation set**, have each candidate LLM answer every question, then pass all answers to a **judge LLM** with a rubric prompt (e.g. score each model out of 100).
- Why it matters: Works when answers are **long or semantically rich**, where n-gram overlap is weak.
- Caveats: Extra cost/latency; judge model and prompt quality affect scores.

### Traditional NLP metrics (n-gram and hybrid)
- What it is: **BLEU** (overlap precision-like), **ROUGE** (recall-like), **METEOR** (harmonic-style combo of BLEU and ROUGE in the instructor’s quick example), plus **model-based** **BERTScore** and **BARTScore**.
- Why it matters: Cheap, interpretable baselines when answers are **short and close to reference text**.
- Instructor emphasis: **Not effective for huge or semantically different outputs** in real projects; still taught for completeness. Use an **Excel-style grid**: questions, references, each model’s answer, then per-metric columns per model; aggregate or send the sheet to an LLM for a final verdict.

### RAGAS (RAG Assessment)
- What it is: A framework to evaluate RAG outputs with metrics such as **context recall**, **faithfulness**, and **factual correctness** (and in the second notebook: **answer relevancy**).
- Why it matters: Designed for **RAG-specific** quality beyond raw text overlap.
- Caveats: Had **version/install issues** (`langchain-community < 0.4`); **default `evaluate()` uses OpenAI** unless you pass a custom LLM wrapper (e.g. Groq via LangChain). Metric keys may be **snake_case**—automation failed partly due to key handling. A minor **KeyError** at the end of one script was described as almost fixed.

### Chunk size and overlap
- What it is: With `chunk_size=1000` and `chunk_overlap=200`, each new chunk advances by **800** tokens (1000 − 200).
- Why it matters: Wrong chunk counts break retrieval evaluation and storage expectations.
- Instructor walkthrough: **10,000 tokens → 13 chunks** (last chunk ~200 tokens).

### Separating evaluation pipeline from production app
- What it is: Evaluation code (sample Q&A, multi-model runs) is a **separate pipeline** from the client-facing Streamlit/UI RAG app.
- Why it matters: You pick the winning model in evaluation, then deploy **one** model in production; clients may ask to see the evaluation artifact.

### Evaluating non-Q&A, token-heavy outputs (scheme of work)
- What it is: For table-like, long generated curricula, **summarize outputs** (e.g. add “summarize in ~200 words” or monthly vs weekly views) before **LLM-assisted** comparison to cut **latency and eval cost**.
- Why it matters: Victoria’s UK **Skills England** apprenticeship RAG produces large structured plans, not one-line answers.
- Instructor emphasis: **LLM-assisted > RAGAS > n-grams** for this use case; RAGAS may still work on summarized text; BERT/BART only for smaller interactive Q&A parts of the product.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| **RAGAS** | `evaluate()` on dataset built from questions, answers, references; metrics: context recall, faithfulness, factual correctness, answer relevancy |
| **LangChain** | `PyPDFLoader`, `RecursiveCharacterTextSplitter`, `HuggingFaceEmbeddings`, retriever, prompt template |
| **langchain-community** | Pin **`< 0.4`** to avoid RAGAS integration errors |
| **FAISS** | Facebook AI Similarity Search vector index for retrieval |
| **sentence-transformers/all-MiniLM-L6-v2** | 384-dim embeddings; “basic” vs OpenAI embeddings for best retrieval |
| **Groq API** | Models compared: **Groq 3 latest**, **Groq 3 mini**, **Groq 4 latest**; instructor shared API key in class (rotate/replace locally) |
| **ChatXAI / LangChain XAI** | Used as the chat LLM wrapper in the notebook [exact package name as spoken in session] |
| **Google Colab** | Week 8 path: `AGAI 3` → `31st May` → `rag-metrics-evaluation` |
| **ROUGE library** | Python package for ROUGE scores |
| **NLTK** | `nltk.translate.bleu_score` / `sentence_bleu` for BLEU |
| **bertscore** | Model-based similarity metric |
| **bartscore** / **bart-tokenizer** | BART-based scoring (initialization shown in notebook) |
| **Power BI ebook PDF** | Sample document for RAG questions (e.g. “components of Power BI”, Power Query editor) |
| **“Attention Is All You Need” PDF** | Second RAGAS demo document |
| **Ollama** | Mentioned for swapping in `ChatOllama`; slower eval (~15–20 min vs ~5 min on Groq) |
| **Streamlit** | Example production UI after model selection |
| **ChromaDB** | Mentioned by student Victoria for her curriculum RAG |

## Code & Examples

### RAG stack (conceptual flow from notebook)

```python
# Approximate flow from instructor walkthrough
from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.embeddings import HuggingFaceEmbeddings
from langchain_community.vectorstores import FAISS

loader = PyPDFLoader("power_bi_ebook.pdf")
docs = loader.load()

splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
chunks = splitter.split_documents(docs)

embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
vectorstore = FAISS.from_documents(chunks, embeddings)
retriever = vectorstore.as_retriever()

# Prompt pattern (paraphrased)
# "Answer using only the context below. If not in context, say I don't know."
```

### Sample evaluation dataframe

Five rows with columns like **`question`**, **`answer`** (model output), **`reference`** (ground truth), then fed into RAGAS `EvaluationDataset.from_lists(...)`.

### RAGAS evaluate (metrics named in session)

```python
# Metrics explicitly used in the main demo:
# - context recall (instructor also said "recall")
# - faithfulness
# - factual correctness

# Pseudocode — exact API as in Colab
from ragas import evaluate
from ragas.metrics import ...  # faithfulness, context_recall, factual_correctness

result = evaluate(
    dataset=evaluation_dataset,
    metrics=[...],
    llm=llm_wrapper,      # required to avoid default OpenAI
    embeddings=embeddings,
)
```

### Dependency workaround

```bash
pip install ragas  # plus other notebook deps
pip install "langchain-community<0.4"
```

### BLEU / ROUGE loop over `results`

```python
# Approximate — instructor iterated evaluation results
for item in results:
    reference = item.reference.split()  # or similar
    hypothesis = item.response  # [field name approximate]
    bleu_scores.append(sentence_bleu([reference], hypothesis))
    rouge_scores.append(rouge.get_scores(hypothesis, reference))
```

### Switching LLM provider (Groq → Ollama)

```python
# Instructor: change model string in one place, rerun pipeline
from langchain_ollama import ChatOllama  # paraphrased
# Replace Groq chat wrapper and re-execute cells through RAGAS
```

### Chunk count exercise

- Tokens 0–1000 → chunk 1; next starts at 801 → … → **13 chunks** for 10,000 tokens with overlap 200.

### METEOR example (instructor’s on-the-spot math)

- LLM: “the sun is shining”; reference: “the sun is in the sky”
- BLEU-1 ≈ 3/6 = **0.5**; ROUGE-1 ≈ 3/4 = **0.75**
- METEOR ≈ 2×0.5×0.75 / (0.5+0.75) = **0.6**

### Automation attempt (end of class)

- Goal: loop `model_name in [groq-3-latest, groq-3-mini, groq-4-latest]`, run RAGAS, print best model + metric values.
- Outcome: **“No models were evaluated”**, empty `model_scores`, snake_case metric key issues; ChatGPT-suggested `get_score()` helper still failed (`EvaluationResult` attribute error). Code uploaded as `evaluation pipeline` for students to debug.

## Q&A / Discussion Points

- **Tao (late):** Yesterday covered **RAG pipeline evaluation**; instructor gave a 5-minute recap.
- **Chunk count:** With 10k tokens, chunk 1000, overlap 200 → **13 chunks** (not 10); must account for overlap stride of 800.
- **Embedding choice:** `all-MiniLM-L6-v2` is **384-dim**, basic; **OpenAI embeddings** preferred for best retrieval if cost allows.
- **Victoria — scheme-of-work RAG:** Scrapes **Skills England** standards into **ChromaDB**, multi-agent, outputs **tables** (12/18-month plans). No gold lesson plans for reference-based metrics. Instructor: use **LLM-assisted** evaluation on **summarized** outputs; monthly dropdown already reduces tokens; BERT/BART only for smaller Q&A-on-criteria features; will think if a better approach exists.
- **BART vs BERT vs GPT:** BART = encoder-decoder transformer; BERT = encoder-only; GPT = decoder-only—they are **not** “LLMs” in the same sense as GPT-scale chat models.
- **Ajith:** File/version confusion across batches; instructor re-uploaded fixed notebook to Drive.
- **End of class:** Automation errors on Groq 4; instructor asked students to try fixing before doubt-clearing.

## Action Items & Assignments

- [ ] Open **Week 8, 31 May** Colab files (`rag-metrics-evaluation` and related) from shared Drive links
- [ ] Run full notebook: install deps, use **`langchain-community < 0.4`**, upload Power BI PDF, set **Groq API key**
- [ ] Compare models by re-running pipeline with **Groq 3** vs **Groq 4** (and optionally Ollama/OpenAI per swap instructions)
- [ ] Run **BLEU/ROUGE** (and optional **BERTScore/BARTScore**) cells; interpret or LLM-summarize results
- [ ] Try second **RAGAS** notebook on **Attention Is All You Need** if time (long run)
- [ ] Debug **`evaluation pipeline`** automation notebook; instructor will fix before **doubt-clearing class**
- [ ] Instructor will **evaluate assignments** and follow up
- [ ] Optional: enroll via shared Udemy coupon (first 100 users); watch/comment on instructor’s YouTube video [promotional, not technical]

## Open Questions / Unclear Spots

- Exact final numeric scores for **Groq 4** were partially lost when cells were re-run; instructor kept **Groq 3** copy-paste showing context recall **0.55**, faithfulness **0.44**, factual correctness **[~0.13–0.1380 in transcript]**—verify in notebook.
- Remaining **KeyError** at end of original RAGAS script (“last key zero”)—described as minor/fixable.
- **`ChatXAI` / `LangChain XAI`** exact import path vs Groq naming in materials [unclear: branding in slides vs package name].
- Whether **`factual correctness` 0.1380** is a typo for **0.138** or another scale—confirm from Colab output.
- Automation fix details (snake_case RAGAS metric keys, `get_score` helper) were not fully demonstrated working live.
- **Fine-tuning Ollama practicals** deferred to doubt-clearing session.
- Transcript typos corrected where obvious (**RAGAS**, **Groq**, **vibe coding**); student name **Esim** and poll “silent hater” are incidental, not technical.
