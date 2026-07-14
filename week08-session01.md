# RAG Evaluation: Choosing the Best LLM for Your Use Case

## TL;DR
This session covered three techniques for evaluating which LLM performs best in a RAG pipeline: LLM-as-a-judge, traditional NLP n-gram metrics (BLEU, ROUGE, METEOR), and the RAGAS assessment framework. The instructor demonstrated each approach using open-source Ollama models (TinyLlama, Llama 3.2 1B, Llama 3.2 3B) against a Q&A dataset derived from the "Attention Is All You Need" paper.

## Topics Covered
- Why evaluation matters for generative AI projects (client justification)
- LLM-assisted evaluation (LLM-as-a-judge)
- Traditional NLP metrics: BLEU, ROUGE, METEOR
- Memory calculation for quantized open-source models
- RAGAS (RAG Assessment) framework introduction
- Practical demo building a RAG pipeline with LangChain, FAISS, and HuggingFace embeddings

## Key Concepts Explained

### LLM-as-a-Judge Evaluation
- Use a stronger LLM (e.g., Grok-4) to score responses from candidate models against a ground-truth Q&A document provided by the client.
- Useful as a quick, automatable first-pass evaluation when you have multiple candidate models.
- Important caveat: mask model names (use "Model 1", "Model 2") so the judge LLM is not biased by brand recognition. The judge must be a stronger/smarter model than the candidates being evaluated.

### BLEU Score (Precision-like)
- Formula: (matching n-grams in LLM output) / (total n-grams in ground truth).
- Measures how much of the ground truth vocabulary appears in the LLM response.
- BLEU-1 uses unigrams, BLEU-2 uses bigrams, etc. Range: 0 to 1, higher is better.

### ROUGE Score (Recall-like)
- Formula: (matching n-grams in reference) / (total n-grams in LLM output).
- Measures how much of the LLM output is covered by the reference answer.
- ROUGE-1 uses unigrams, ROUGE-2 uses bigrams. Range: 0 to 1, higher is better.

### METEOR Score (F1-like)
- Formula: 2 * BLEU * ROUGE / (BLEU + ROUGE) -- harmonic mean of BLEU and ROUGE.
- Balances precision (BLEU) and recall (ROUGE) into a single metric.
- Analogous to F1 score in classification.

### RAGAS (RAG Assessment Framework)
- An open-source library that automates RAG evaluation by combining LLM-as-a-judge and NLP metrics internally.
- You feed it: question, retrieved contexts, generated answer, and ground truth. It returns scores for metrics like faithfulness, answer relevancy, context precision, and factual correctness.
- The evaluator LLM used by RAGAS must be a strong model (not one of the candidates being evaluated).

### Model Memory Calculation (Quantized Models)
- Formula: (parameters * 4 bytes) / (1024^3) / quantization_divisor = memory in GB.
- Example for Llama 3.2 3B with Q4 quantization: 3B * 4 bytes = 12 GB raw -> 12 / 1.073 = ~11.18 GB -> divided by 4 (for 4-bit quantization) = ~2.79 GB.
- 4 bytes per parameter because of FP32 (32-bit = 4 bytes); Q4 quantization reduces memory by 75%.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Ollama | Local inference server for open-source LLMs |
| Llama 3.2 3B / 1B | Open-source candidate models evaluated in demo |
| TinyLlama 1.1B | Smallest candidate model; scored lowest (30/100) |
| Groq-4 | Used as the LLM judge in the evaluation demo |
| RAGAS | Python library for automated RAG evaluation (`pip install ragas`) |
| LangChain | Orchestration framework; used for PDF loading, text splitting, retriever |
| FAISS | Facebook AI Similarity Search; vector store for embeddings |
| HuggingFace `all-MiniLM-L6-v2` | Sentence-transformer model used for embeddings |
| PyPDF / PyPDFLoader | Extracts text from PDF documents |
| Groq API | Fast inference API used for the RAG pipeline demo |

## Code & Examples

### LLM-as-a-Judge Prompt (approximate)

```text
I have three responses to a question based on the paper attached.
The ground truth (answer) to this question is: <ground_truth>

Model 1 response: <response_1>
Model 2 response: <response_2>
Model 3 response: <response_3>

Behave as an expert in the domain subject. Create a rubric, give a score
out of 100 to all of these responses, and conclude which model is
performing the best.
```

### BLEU / ROUGE / METEOR Calculation Example

```text
LLM answer:    "the sun is shining"
Ground truth:  "the sun is in the sky"

BLEU-1:  matching unigrams in GT / total unigrams in GT = 3/6 = 0.5
ROUGE-1: matching unigrams in LLM / total unigrams in LLM = 3/4 = 0.75
METEOR-1: 2 * 0.5 * 0.75 / (0.5 + 0.75) = 0.6
```

### RAGAS Pipeline (approximate structure from demo)

```python
from ragas import EvaluationDataset, evaluate
from ragas.metrics import context_recall, faithfulness, factual_correctness
from langchain_community.llms import ChatOpenAI  # or Groq wrapper

# Prepare dataset with columns: question, answer, contexts, ground_truth
dataset = EvaluationDataset.from_pandas(df)

results = evaluate(
    dataset=dataset,
    metrics=[faithfulness, context_recall, factual_correctness],
    llm=evaluator_llm,  # must be a strong model
)
```

### Memory Estimation Formula

```python
def estimate_memory_gb(params_billions, quantization_bits=4):
    raw_bytes = params_billions * 1e9 * 4  # FP32
    raw_gb = raw_bytes / (1024**3)
    return raw_gb / (32 / quantization_bits)

# Llama 3.2 3B Q4 -> ~2.79 GB
estimate_memory_gb(3, quantization_bits=4)
```

## Q&A / Discussion Points
- **Q: Can we run Ollama on Google Colab?** A: Not directly with your local Ollama. You'd need to install Ollama on the Colab instance itself.
- **Q: Is the evaluation framework the same for commercial LLMs (OpenAI, Groq)?** A: Yes, the same techniques (metrics, RAGAS, LLM-as-judge) apply. Just swap open-source models for API-based ones.
- **Q: Why not use one of the candidate models as the judge?** A: The judge must be a stronger, more reliable model to avoid bias and poor evaluation quality.

## Action Items & Assignments
- [ ] Submit the previous assignment by tomorrow 9 PM Hong Kong time (if you want instructor feedback)
- [ ] Practice the evaluation pipeline code (files to be shared after class)
- [ ] Attend doubt-clearing class where project submissions will be reviewed

## Open Questions / Unclear Spots
- The RAGAS framework demo hit dependency issues (PyArrow DLL, LangChain community imports) and was not completed live; instructor planned to fix and demonstrate in the next session.
- The exact RAGAS metrics computed and their interpretation were not fully walked through due to the technical issues.
- [unclear: "Vortex AI" reference] -- likely a misrecognition of "Vertex AI" in an import error message from `langchain_community.chat_models`.
