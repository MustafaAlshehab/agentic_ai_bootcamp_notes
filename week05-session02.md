# Word2Vec, Transformers (BERT/GPT), LLMs, and Client LLM Selection

## TL;DR
This session built a hand-crafted Word2Vec-style example (king − man + woman ≈ queen), then introduced the transformer stack—from the original encoder–decoder architecture through the split into BERT (encoder-only) and GPT (decoder-only). It connected GPT to large language models (parameters, context window, tokens) and walked through a consulting-style scenario for choosing LLMs for a bank’s document/RAG chatbot (public vs. private APIs vs. open source). The class ended with cosine similarity (used in a BERT-based extractive summarization Colab demo) and a preview of LangChain/embeddings in upcoming weeks.

## Topics Covered
- Google News Word2Vec model (download, size, 300 dimensions)
- Building a custom small Word2Vec model with explicit features
- Vector arithmetic demo (king − man + woman)
- How NLP handles sentences: tokenization → per-word embeddings
- Transformers history (“Attention Is All You Need”, 2017)
- Encoder–decoder architecture vs. split into encoder-only (BERT) and decoder-only (GPT)
- BERT architecture and use cases (masked prediction, summarization)
- GPT / decoder models, prompts as input after the encoder link was removed
- Translation walkthrough (English → Spanish) with encoder/decoder layers
- Pre-trained vs. fine-tuning; definition of large language models (LLMs)
- LLM vendors, examples, parameters, context window, tokens
- Client scenario: banking RAG/document chatbot and how to recommend LLM deployment options
- Public APIs, private (cloud) APIs, aggregators, open source (Ollama, Hugging Face)
- Pros/cons comparison for POC and production
- Optional BERT extractive summarization demo in Colab
- Cosine similarity (intuition, 2D examples, bag-of-words formula)
- Bootcamp schedule (weeks 5–12, case study, possible break week)
- Wrap-up and Thursday doubt-clearing session

## Key Concepts Explained

### Google News Word2Vec
- Pre-trained Word2Vec on Google News (~1 GB file); vectors are **300-dimensional**. The exact features are **not interpretable** (unsupervised).
- Why it matters: Industry-standard word embeddings before transformers; still useful for understanding embedding space and analogies.
- Caveat: Too large for Codecademy to host; download via instructor Discord link or search “Google News Word2vec download.”

### Custom Word2Vec (hand-built embeddings)
- What it is: A toy model with a **small vocabulary** and **hand-chosen dimensions** (e.g., gender, money, power, weight, can-speak), values in **[0, 1]**.
- Why it matters: Shows that if dimensions encode meaningful attributes, **vector math can mirror semantics** (e.g., king − man + woman ≈ queen).
- Instructor emphasized: You define features and scores; results only work if features align with the analogy you care about.

### Sentences → tokens → embeddings (not whole-sentence Word2Vec)
- What it is: In classic NLP pipelines, a sentence is **tokenized**; **each token** gets its own embedding (e.g., 300-D from Google News).
- Why it matters: Foundation for later RAG/generative AI—many vectors per document, often stored in a **vector database**; retrieval uses **similarity** to the query embedding.
- Caveat: Traditional Word2Vec is **per word**, not per sentence (sentence-level similarity uses other methods, e.g., cosine on bag-of-words or later embedding models).

### Transformers (original encoder–decoder)
- What it is: Architecture from ~2017 (“Attention Is All You Need”); **encoder** processes input embeddings + positional encoding + multi-head self-attention + feed-forward; **decoder** generates output (e.g., translation) with self-attention, **cross-attention** to encoder, and masking during training.
- Why it matters: Replaced many seq2seq approaches; basis for all modern LLMs.
- Instructor emphasized: Encoder **understands** input; decoder **generates** output token by token (BOS/SOS start, EOS end).

### Split: BERT (encoder-only) vs. GPT (decoder-only)
- What it is: Researchers split the combined model so each half could specialize—**BERT** (Bidirectional Encoder Representations from Transformers), **GPT** (Generative Pre-trained Transformers).
- Why it matters: BERT strong for **understanding** tasks (e.g., fill-in-the-blank / next-token in context, summarization); GPT strong for **generation**.
- Caveats: Instructor noted **in 2026, decoder-only (GPT-style) models dominate**—they also handle summarization, translation, and prediction; **encoder-only models are largely unused** in production now. Optional to deep-dive BERT; use AI assistants if needed.

### Self-attention and multi-head attention
- What it is: Mechanism to decide **which tokens to focus on** when building representations (bidirectional in encoder self-attention).
- Why it matters: Enables context understanding in long sentences (query/key/value from same tokens in encoder).
- Cross-attention in decoder: connects decoder states to **encoder outputs** in full seq2seq models.

### Prompt as decoder input (post-split)
- What it is: After encoder and decoder were separated for standalone GPT, the **encoder→decoder link** was removed; **user prompts** became the input to decoder-only chat models.
- Why it matters: Explains why ChatGPT-style apps take a **prompt**, not a separate encoded pipeline you manage.
- Instructor clarified confusion: Translation example (“How are you?” → Spanish) used **encoder + decoder together**; modern chat GPT only needs the **prompt** path.

### Pre-trained models and fine-tuning
- What it is: **Pre-trained** models are trained on huge corpora (Wikipedia, articles, repos, etc.) with unknown exact sources—**broad prior knowledge**. **Fine-tuning** adapts them to specific tasks (mentioned, not detailed this session).
- Why it matters: You usually start from pre-trained weights rather than training from scratch.

### Large language models (LLMs)
- What it is: **Generative, pre-trained** models for **text** (also audio/video/image elsewhere, but most industry use is text), trained on **large corpora**.
- Why it matters: Core of current generative AI products.
- Key characteristics discussed:
  - **Parameters** (e.g., 8B, 20B, 120B)—larger models → larger file sizes (e.g., ~3 GB vs. 10 GB vs. ~100 GB).
  - **Context window** (e.g., 2,156 tokens)—only recent tokens stay “in memory”; older chat turns drop out → **wrong answers / hallucinations** on long threads (especially free tiers).
  - **Input/output tokens**—billing and limits are often token-based.

### LLM deployment categories (client scenario)
- What it is: Framework for advising enterprises (e.g., bank + RAG over documents):
  - **Public APIs**: Direct credits from OpenAI, Anthropic, xAI (Grok), etc.
  - **Aggregators** (e.g., **Groq** platform): One API key, multiple models behind it—including some OpenAI models.
  - **Private APIs**: Via cloud—AWS **Bedrock**, Azure **AI Foundry**, GCP **Vertex AI** (Anthropic, DeepSeek, etc. available depending on provider).
  - **Open source**: **Ollama**, **Hugging Face**—run on **your server**; free model weights, higher ops burden.
- Why it matters: Banks care about **data privacy**; choice ties to existing cloud and compliance.
- Instructor’s pitch: Ask client about **AI maturity**, current LLMs, and **cloud posture**; explain ecosystem; for many banks **private APIs** balance cost, security, POC speed, and easy model switching; if **no cloud / strict privacy**, lean **open source on-prem** despite harder POC/production.

### Cosine similarity
- What it is: Similarity score in **[0, 1]** from the **angle** between vector representations of two texts (often bag-of-words counts for toy examples); **1 = very similar**, **0 = unrelated** (e.g., orthogonal “hello world” vs. “hello” on different axes → 0).
- Why it matters: Used in the BERT demo to pick **top sentences** most similar to the document representation for **extractive summarization**.
- Caveats: For high-dimensional **word embeddings**, pipelines use **similar** but not identical tools (e.g., **FAISS**, LSH)—covered next session. Instructor’s live formula on whiteboard had a moment of uncertainty but numeric example matched **~0.57** for “I love data science” vs. “I love Codecademy.”

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Google News Word2Vec | ~1 GB, 300-D vectors; external download |
| Word tokenizer | Breaks sentences into tokens before embedding |
| Transformers / “Attention Is All You Need” | 2017 foundation paper |
| BERT | Encoder-only; e.g., BERT-base = 12 transformer blocks; `bert-base-uncased` in demo |
| GPT / GPT-3.5, GPT-4o, 4o mini | OpenAI decoder / LLM examples |
| T5 | Mentioned alongside BERT as a transformer variant [instructor said “T4”—likely T5] |
| Hugging Face | Platform (not an LLM itself); hosts models |
| OpenAI, Anthropic (Claude), xAI (Grok), Meta (Llama), Google (Gemini; Bard legacy), AWS (Nova), Alibaba (Qwen), Mistral, DeepSeek, Perplexity | LLM ecosystem examples |
| Groq | **Aggregator** API platform (distinct from xAI’s **Grok**) |
| AWS Bedrock, Azure AI Foundry, GCP Vertex AI | Private/cloud-hosted model access |
| Ollama | Local open-source model runner |
| Google Colab | Ran Week 5 BERT summarization notebook |
| LangChain | Preview: core components, text splitting, embeddings—**next week** |
| FAISS (Facebook AI Similarity Search) | Mentioned for vector similarity at scale—next class |
| Locality Sensitive Hashing (LSH) | Mentioned alongside FAISS [transcript: “Locative Sensitive Hashing”] |
| Poe / ChatGPT | Instructor used AI to refresh transformer layer details and verify cosine math |
| Instructor YouTube channel | ~10 min video: AI-built dashboard in ~5 minutes with Groq |

## Code & Examples

### Custom Word2Vec (5 words × 5 features)

Vocabulary example: `king`, `man`, `woman`, `queen`, `elephant`.  
Features: `gender`, `money`, `power`, `weight`, `speak` (values 0–1).

| Word | gender | money | power | weight | speak |
|------|--------|-------|-------|--------|-------|
| king | 1 | 1 | 1 | 0.8 | 1 |
| man | 1 | 0.3 | 0.2 | 0.7 | 1 |
| woman | 0 | 0.3 | 0.2 | 0.5 | 1 |
| queen | 0 | 1 | 0.7 | 0.6 | 1 |
| elephant | 1 | 0 | ~0.8–0.9 | 0.95 | 0 |

**Analogy check:** `king − man + woman` → approximately `(0, 1, 0.7, 0.6, 1)` vs. queen’s `(0, 1, 0.7, 0.6, 1)` — close on each dimension.

### BERT masked / fill-in-the-blank (conceptual)

```
Ali is walking. Ali came from the ____.
```

Last token masked; prediction head scores candidates (e.g., `garden` 0.7, `city` 0.25) → fill **garden**.

### Translation encoder–decoder (training flow, approximate)

```
Input:  "How are you?"  → tokenize → embeddings (+ positional encoding)
        → encoder (multi-head self-attention + FFN + layer norm)
Decoder: starts with BOS/SOS; teacher-forced prior tokens during training
        → self-attention + cross-attention to encoder + FFN
        → prediction head → "cómo", "estás", … → EOS
```

Modern GPT chat: **prompt only** → decoder stack → generated text (no separate user-managed encoder).

### BERT extractive summarization (Colab — approximate flow from demo)

```python
# Imports + load pretrained tokenizer/model (bert-base-uncased)
# Long input blog text
# Sentence tokenization → sentence embeddings → document-level embedding
# Cosine similarity between document and each sentence
# Take top 5 sentences → join → printed summary
```

Instructor noted Hugging Face/token step had a minor issue on one cell but run completed with summary output.

### Cosine similarity (bag-of-words style)

For sentences as word count vectors **A**, **B**:

\[
\text{cosine similarity} = \frac{\sum_i A_i B_i}{\sqrt{\sum_i A_i^2} \cdot \sqrt{\sum_i B_i^2}}
\]

Example: “I love data science” vs. “I love Codecademy” → instructor computed **~0.57** (some similarity, not identical).

### Optional practice prompt (for AI tools)

Instructor suggested asking an LLM to generate **dummy BERT-12 extractive summarization Python** for a large blog input—exploratory only, not mandatory.

## Q&A / Discussion Points

- **Sentence vs. word embeddings:** Sentences are split into tokens; each token embedded separately; vectors may later live in a vector DB for retrieval (RAG preview).
- **Decoder input after cutting encoder link:** User **prompt** is the input to standalone GPT-style models.
- **“How are you” vs. prompt confusion:** Clarified with full **translation** encoder–decoder diagram and Roger Federer → Hindi analogy (brain processes input, outputs word-by-word).
- **Gmail-style suggestions:** Discussed in context of **predicting masked/next tokens** (encoder-style fill-in-the-blank); transcript once said “decoder”—likely slip given BERT discussion.
- **Data privacy / LLM guarantees:** Deferred into scenario discussion—open source on-prem vs. private cloud APIs.
- **Cosine similarity vs. word embeddings:** Cosine on bag-of-words shown today; embedding search uses **related** methods (FAISS, etc.)—**next class**.
- **Week 5 practicals on platform:** Instructor didn’t think official Week 5 hands-on labs were assigned on Codecademy.
- **Break week:** Not confirmed; only instructor considering time for RAG assignment after weeks 6–7; **Codecademy team** must confirm.

## Action Items & Assignments

- [ ] Download **Google News Word2Vec** if needed (Discord link from instructor or web search).
- [ ] Watch instructor’s **~10 minute YouTube video** on building an AI dashboard with **Groq** (~5 minutes to replicate); try with any Excel/dataset.
- [ ] Optional: reproduce **BERT summarization** Colab (`week 5 BERT`) using code/prompt instructor shared.
- [ ] Optional: explore BERT via AI-generated practice code—**not mandatory**.
- [ ] Complete **poll** if not voted (session feedback).
- [ ] Attend **Thursday doubt-clearing** session.
- [ ] Prepare for **Week 6+**: LangChain, embeddings, text splitting; **case study assignment before Week 8** (per schedule discussion).

## Open Questions / Unclear Spots

- Instructor said **“T4 and BERT”** as transformer examples—likely meant **T5** [unclear: exact model list].
- **Gmail autocomplete** attributed once to “decoder models” while earlier tied to **encoder/BERT**-style masking—instructor intent was masked next-token prediction; wording inconsistent in transcript.
- **Cosine formula** on whiteboard: instructor briefly doubted notation then confirmed summation form; numeric result matched AI check (**0.57**).
- **“Photo”** mentioned with GPT model names [unclear: possibly GPT-4o or another SKU].
- **Official break week** after Week 6/7: **not confirmed**—wait for Codecademy announcement.
- **“Week five practicals”** on Codecademy: instructor uncertain—verify in course portal.
