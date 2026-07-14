# RAG vs Fine-Tuning, Quantization, and Hybrid RAG Chatbot Assignment

## TL;DR
This session introduced fine-tuning as an alternative to RAG for generative AI problems, emphasizing that RAG and prompting solve ~99% of real use cases while full fine-tuning is costly in time, GPU, and money. The instructor compared when to choose each approach, explained why API-hosted models (OpenAI, Groq) cannot be fine-tuned while open-source models (Llama via Hugging Face/Ollama) can, covered full-parameter fine-tuning costs, quantization (FP32→FP4), and previewed PEFT/LoRA/QLoRA (deferred to next class). A RAG-based Streamlit assignment with web scraping and Q&A routing was announced (deadline May 29), bundled with a Codecademy LinkedIn contest.

## Topics Covered
- Class logistics: doubt-clearing schedule, canceled sessions, assignment and contest overview
- Generative AI problem-solving: RAG vs prompting vs fine-tuning (~99% / ~1% split)
- How RAG works vs how fine-tuning embeds domain data into the model
- Parameters, weights, batch size, epochs, and training time intuition
- Pre-training vs fine-tuning vs quantized fine-tuning; LoRA and QLoRA (intro only)
- RAG vs fine-tuning: accuracy trade-offs and instructor’s industry experience ratio
- Student Q&A: domain data vs RAG DB, hybrid RAG+fine-tune, open-source fine-tuning, API vs local models
- When to use RAG vs fine-tuning (scenarios and decision flow)
- Full-parameter fine-tuning and memory/GPU challenges
- PEFT overview and LoRA intuition (non-technical diagram)
- Quantization: definition, TinyLlama Hugging Face vs Ollama size, memory math, precision trade-offs
- Hugging Face token setup and Colab PDF chatbot demo (in progress at end of class)
- Assignment walkthrough: scrape website → PDFs → Q&A doc → RAG chatbot with Q&A-first routing
- LoRA/QLoRA mathematics deferred to next session

## Key Concepts Explained

### RAG vs fine-tuning (core distinction)
- **What it is:** RAG chunks documents, embeds them in a vector store, retrieves relevant chunks at query time, and passes them with a prompt to an unchanged LLM. Fine-tuning updates (or adapts) the model weights using domain-specific training data so knowledge is baked into the model.
- **Why it matters:** RAG is faster, cheaper, and easier when documents change often. Fine-tuning can yield higher accuracy on niche jargon but is rarely needed if RAG meets requirements.
- **Instructor emphasized:** ~99% of cases are solvable with RAG + prompting; in ~50+ POCs and 7+ deployments, only 1 was fine-tuning (client-driven). Don’t pursue fine-tuning for marginal accuracy gains (e.g., 95% → 96%) if RAG is satisfactory.

### Pre-training, fine-tuning, and quantized fine-tuning
- **What it is:** Pre-training builds a base LLM from large corpora. Fine-tuning continues training an existing model on domain-specific data. Quantized fine-tuning (e.g., QLoRA) combines lower-precision weights with efficient adaptation.
- **Why it matters:** Clarifies the lifecycle: public pre-trained model → domain fine-tune → optional deployment; distinct from RAG’s retrieval layer.
- **Caveats:** Fine-tuned models can answer from both public and domain knowledge unless restricted via prompt injection / prompt templates.

### Domain-specific data in fine-tuning vs RAG “database”
- **What it is:** Fine-tuning grows effective model knowledge (e.g., 50 GB base + 1 GB domain → retrain ~51 GB conceptually). RAG never trains the LLM; it only supplies context per query (question + retrieved chunks + template).
- **Why it matters:** Explains why uploading a PDF to ChatGPT-style tools feels instant—you’re not retraining, you’re passing context (sometimes without explicit vector DB, but same idea).
- **Instructor emphasized:** Separate sessions won’t remember uploaded docs unless the model was fine-tuned; otherwise you get hallucination without RAG.

### API-hosted (closed) vs open-source (local) models
- **What it is:** OpenAI, Google Gemini, Anthropic, Groq, etc. expose models via API keys in vendor data centers—you invoke, not own, weights. Open-source models on Hugging Face/Ollama can be downloaded and fine-tuned locally (e.g., Llama 3.2 3B on Ollama ~2 GB local vs ~7 GB on Hugging Face).
- **Why it matters:** Fine-tuning requires physical access to weights; API models cannot be fine-tuned by end users—only vendors release new versions (e.g., GPT-4o → GPT-5).
- **Caveats:** Groq/OpenAI calls still hit a physical remote model; Ollama `ollama list` shows models on your machine, fine-tunable in principle.

### Hybrid fine-tune + RAG
- **What it is:** Idea: fine-tune a small model for tone/format, use RAG for factual content.
- **Why it matters:** Theoretically doable but instructor considers it unnecessary—pick one path; small models are weaker (“partial knowledge”) even if fine-tuned.

### When to use RAG
- **Scenarios:** Frequently changing document sets (e.g., directory with 2→5→50 PDFs daily); default first try for enterprise; domain-specific docs (finance, etc.) until HITL validation fails after tweaks.
- **Why:** Re-indexing/ingesting is far cheaper than retraining daily.

### When to use fine-tuning
- **Scenarios:** Organization mandates secured on-prem open-source fine-tune; very niche jargon poorly handled by general chatbots after RAG + HITL still fails; client explicitly requires it.
- **Caveats:** Try RAG first with client Q&A verification (human-in-the-loop).

### Model size vs latency
- **What it is:** Smaller models (e.g., 3B) respond faster; larger models (e.g., 400B) slower but more factual—client chooses based on latency vs correctness needs.
- **Why it matters:** Smaller models fine for learning; production often favors larger when GPU budget allows.

### Full-parameter fine-tuning
- **What it is:** Updates all model weights (e.g., 7B model → 7B weight updates per training step), with batch/epoch combinatorics driving huge iteration counts and GPU memory needs.
- **Why it matters:** Explains why fine-tuning is “expensive”—storage and update of billions of weights.
- **Instructor emphasized:** This is the problem PEFT methods address.

### PEFT, LoRA, and QLoRA (preview)
- **What it is:** Parameter-Efficient Fine-Tuning; LoRA = Low-Rank Adaptation (adapter layers on important weights only); QLoRA = quantized LoRA.
- **Why it matters:** Train/adapt fewer effective parameters than full fine-tune—faster and cheaper.
- **Caveats:** Mathematical detail deferred to next class; non-technical analogy: skip updating “unimportant” neurons to cut training time.

### Quantization
- **What it is:** Reducing numeric precision of weights (e.g., FP32 → FP16 → FP8 → FP4 → FP2), lowering memory and compute while trading accuracy—like running a game at 480p vs 4K.
- **Why it matters:** Enables local/edge deployment; Ollama ships quantized models (e.g., TinyLlama Q4 ~638 MB vs Hugging Face ~2.2 GB, ~71% size reduction).
- **Memory rule of thumb:** ~4 bytes per parameter (FP32) → 1B params ≈ 4 GB bytes ≈ ~3.7 GB RAM to run; 70B params ≈ ~261 GB RAM (unquantized). Quantized Ollama TinyLlama ~1 GB RAM vs ~3.7 GB for full-precision HF.
- **Trade-off table (instructor):** FP32 best accuracy, highest cost; FP16 ~50% memory, balanced; FP8 ~25%; FP4 edge/IoT; FP2 very fast, poor accuracy. Ollama TinyLlama confirmed Q4 (4-bit).

### Assignment Q&A routing (not vectorizing FAQ by default)
- **What it is:** On user question, check handcrafted Q&A first (semantic/NLP similarity or optional LLM reframing); only if no match, vectorize scraped PDFs with FAISS and run standard RAG + LLM.
- **Why it matters:** FAQs are exact/high-confidence—vector DB + LLM adds cost; similarity thresholds (e.g., >0.7) avoid extra infra.
- **Three strategies given:** (1) NLP similarity only, no vector/LLM; (2) similarity + LLM polish; (3) vectorize Q&A + full RAG.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| RAG | Chunk → embed → vector DB → retrieve → LLM + prompt template |
| Fine-tuning | Domain data trained into existing LLM weights |
| Ollama | Local models (`ollama list`); Llama 3.2 3B ~2 GB; fine-tunable locally |
| Hugging Face | Full-precision downloads (~7 GB Llama 3.2 3B example); HF access token for Colab |
| Groq API | Remote model via API key; not fine-tunable by user |
| OpenAI / Gemini / Anthropic | API-only; vendor updates models |
| LangChain | `langchain_huggingface_pipeline`, `ChatOpenAI`, `ChatOllama`, `PyPDFLoader` |
| Streamlit | Required for assignment and contest UI |
| FAISS | Vector index for assignment (no separate vector DB) |
| LoRA / QLoRA | Under PEFT; detailed math next class |
| PEFT | Parameter-Efficient Fine-Tuning umbrella |
| Quantization (Q4, FP32–FP2) | Ollama uses 4-bit for TinyLlama |
| TinyLlama | 1.1B params; HF ~2.2 GB vs Ollama ~638 MB |
| Google Colab | HF PDF chatbot demo; secrets for `HF_token` |
| PyPDFLoader | Loading PDFs in HF pipeline example |
| Human-in-the-loop (HITL) | Client validates RAG answers before fine-tune |
| Discord | Assignment/contest docs; project showcase submission |
| Codecademy contest | LinkedIn post + tags + hashtag; GitHub + report |
| Web scraping | Assignment step 1; manual 10–15 pages if auto scrape fails |
| Prompt injection | Restrict answers to domain when fine-tuned model still has public knowledge |

## Code & Examples

### RAG flow (conceptual)
Documents are not fed wholesale into the LLM. Pipeline: chunk → embeddings → vector store → on query: embed question → retrieve similar chunks → send `{question, relevant chunks, prompt template}` to LLM.

### Fine-tuning vs RAG (Roger Federer analogy)
- **Not fine-tuning:** Pass “Roger Federer is a Swiss tennis player; 310 titles” + question to LLM with template → answer from context.
- **Fine-tuning:** Model weights updated so facts are internalized across sessions without retrieval.

### Batch/epoch time intuition (instructor whiteboard math)
Approximate: 800 training samples, batch size 32 → 25 iterations per epoch; ~10 s/iteration → ~250 s/epoch; scales with more data, epochs, and parameter count.

### Memory estimate (TinyLlama, 1B parameters)
```
params = 1_000_000_000
bytes_fp32 ≈ params × 4  # ≈ 4 GB bytes
GB ≈ bytes / (1024**3)   # ≈ 3.72 GB RAM to run (HF ~2.2 GB on disk)
```
70B params (unquantized): ~280 GB bytes → ~261 GB RAM needed.

### Hugging Face PDF chatbot on Colab (AI-generated, demo incomplete)
Instructor prompted an AI tool for code similar to an Ollama RAG chatbot:

```python
# Approximate structure from transcript — verify against shared notebook if provided
from langchain.document_loaders import PyPDFLoader
from langchain_huggingface import HuggingFacePipeline

# model_id e.g. TinyLlama; may use 4-bit quantization config (like Ollama Q4)
# loader = PyPDFLoader("path/to/paper.pdf")
# pipeline = HuggingFacePipeline.from_model_id(...)
```

Colab failed mid-run (missing modules); instructor planned to fix and share. Requires Hugging Face token in Colab secrets (`HF_token`).

### Hugging Face access token setup
1. Hugging Face → Settings → Access Tokens → Create (read/write/inference as needed).
2. Copy token once; store in Colab Secrets (key icon) as e.g. `HF_token`.
3. Alternative: download model files from model page → local inference with AI-assisted code.

### Assignment pipeline (high level)
1. Pick a meaningful website (government, immigration, travel, e-commerce, etc.).
2. Web-scrape; save major pages as PDFs (full site or manual 10–20 pages).
3. Generate a small Q&A doc (~5+ FAQs) via AI assist.
4. Build Streamlit RAG chatbot:
   - If user question matches FAQ (semantic/NLP/optional LLM)—return FAQ answer.
   - Else vectorize scraped PDFs with **FAISS** (in-memory index, no external vector DB) → retrieve → LLM answer.
5. Repo: separate Python files for model logic and Streamlit, `requirements.txt`, README, dataset if used, report (problem, context, features, objectives, how it works).
6. Post on LinkedIn for contest; submit link in Discord project showcase.

## Q&A / Discussion Points

| Topic | Clarification |
|-------|----------------|
| How is fine-tuning domain data different from RAG DB? | RAG does not train the LLM; fine-tuning merges new data into weights with heavy retrain cost. |
| Fine-tune underlying LLM inside RAG architecture? | No—separate processes; combining is “baseless” for most cases. |
| Fine-tune open-source models? | Yes—that’s their purpose; cannot fine-tune closed API models. |
| Nadia: Groq vs Ollama physically | Groq hits remote hosted weights via API; Ollama Llama is local (`ollama list`), fine-tunable. |
| Titan: RAG = content from docs, tone from LLM; fine-tune changes network for tone/format | Confirmed; hybrid possible but unnecessary per instructor. |
| Smaller model + RAG for speed | Valid for latency-bound cases; accuracy often worse on small models. |
| Bianca: Vectorize Q&A into DB? | Not required—check FAQ first via similarity; vectorizing FAQs is optional strategy #3. |
| Ollama exercise | Covered prior session (recording); can revisit in doubt-clearing if needed. |
| Chandav: Practical Ollama share | Same exercise as Groq RAG but with Ollama. |

## Action Items & Assignments

- [ ] Complete **RAG assignment** (document on Discord; instructor also described live): website scrape → PDFs → Q&A → Streamlit chatbot with FAQ-first routing + FAISS RAG fallback.
- [ ] **Deadline: May 29, 2026** (per transcript; confirm in official assignment document).
- [ ] Deliverables: model Python file, Streamlit Python file, `requirements.txt`, README, dataset if applicable, comprehensive report, GitHub repo.
- [ ] **Codecademy contest:** fully functional Streamlit app, clean UI, LinkedIn post tagging Codecademy + hashtag (instructor optional tag), submit LinkedIn URL in project showcase; instructor reviews GitHub for feedback and winner selection.
- [ ] Discord: tag instructor “Sent” after LinkedIn post in project showcase channel.
- [ ] Practice: Ollama document Q&A; try Hugging Face TinyLlama download/compare vs Ollama quantized; HF token + Colab exercise when notebook shared.
- [ ] Watch **prior class recording** for Ollama setup if missed.
- [ ] Research FAQ matching strategies (NLP similarity vs LLM vs vectorize FAQs).
- [ ] Await **Word assignment file** and contest details on Discord (end of class or next day).
- [ ] Next doubt-clearing: **May 21**; sessions **May 23, 24, 28** canceled per instructor; next full class referenced around **May 30**.
- [ ] **LoRA/QLoRA** deep dive in next class.

## Open Questions / Unclear Spots

- LoRA and QLoRA mathematics and implementation—explicitly postponed to next session.
- Hugging Face Colab PDF chatbot notebook—failed during class; instructor to debug and share if possible.
- Exact FP level for all Ollama models—instructor cross-checked TinyLlama as **Q4 (4-bit)**; initially guessed FP8 from size ratio.
- Instructor stated “100 billion model will have 400 billion weights”—likely slip of tongue vs parameter count; treat as informal unless corrected in materials.
- [unclear: “Whiskey fur level”] in chat confirmation—intended **4-bit quantized** Ollama variant.
