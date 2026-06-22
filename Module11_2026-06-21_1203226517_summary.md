# End-to-End RAG Project on n8n with Pinecone & Hugging Face Embeddings

## TL;DR
This session walked through building a complete RAG (Retrieval-Augmented Generation) pipeline on n8n, starting from a basic flow with OpenAI embeddings and a simple vector store, then iterating by swapping in free Hugging Face embeddings and finally integrating Pinecone as a production-grade vector database. A bonus Gmail auto-reply automation was also demonstrated.

## Topics Covered
- RAG vs. fine-tuning refresher
- RAG architecture: data flow vs. retrieval flow
- Building a basic RAG chatbot on n8n (form upload → embeddings → vector store → AI agent)
- Swapping OpenAI embeddings for free Hugging Face embeddings
- Integrating Pinecone as a vector database (index creation, dimension matching)
- Troubleshooting embedding dimension mismatches
- Gmail auto-reply automation workflow
- Brief look at resume/CV screening workflow concept
- Open-ended n8n portfolio project assignment

## Key Concepts Explained

### RAG vs. Fine-Tuning
- Fine-tuning retrains an LLM on domain-specific data (X + Y), which is expensive and resource-intensive.
- RAG passes relevant context alongside the user query to an untrained LLM — no retraining needed.
- ~95% of generative AI use cases use RAG; fine-tuning is reserved for edge cases RAG can't handle.

### RAG Data Flow (Part 1)
- Documents are ingested (PDF, CSV, etc.), chunked, converted to vector embeddings via an embedding model, then stored in a vector store.
- The vector store can be a local file system (PoC) or a vector database (production).

### RAG Retrieval Flow (Part 2)
- User asks a question → question is embedded with the same algorithm → similarity search against stored embeddings → relevant chunks + question sent to LLM → LLM generates answer.
- The green line in n8n confirms the response came from the embeddings, not the LLM's built-in knowledge.

### Embedding Dimension Matching
- The vector store/index dimension must exactly match the embedding model's output dimension (e.g., 384 vs. 512 will fail).
- When creating a Pinecone index, choose dimensions that match your embedding model.
- Instructor emphasized: always verify dimensions before running the pipeline.

### Vector Store as Tool vs. as Chain
- In the n8n AI Agent node, connecting the vector store "as tool" means the agent decides when to query it; "as vector store for chain" forces retrieval on every interaction.
- If the AI agent answers from its own knowledge (no green line to embeddings), the tool connection may be misconfigured.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| n8n | Low-code automation platform used for all workflows |
| OpenAI text-embedding-3-small | Default embedding model (paid via OpenAI credits) |
| Hugging Face Embeddings | Free alternative; used `sentence-transformers/all-MiniLM-L6-v2` (384-dim) and `nli-bert-large` (1024-dim) |
| Pinecone | Managed vector database; free tier available at pinecone.io |
| GPT-4o mini | Chat model used to minimize credit consumption |
| Groq | Alternative free LLM API platform mentioned |
| AWS Bedrock | Enterprise option for embedding/LLM models (paid) |
| Qdrant, Supabase, ChromaDB, Redis, Weaviate, MongoDB Atlas | Other vector DB options available in n8n |
| n8n RAG Starter Template | Pre-built template searchable on n8n website |
| Default Data Loader (binary mode) | Handles PDF/CSV ingestion with automatic format detection |
| Simple Memory node | Adds conversation history to the AI agent |

## Code & Examples

### Basic RAG Flow Structure (n8n nodes)

**Data Flow:**
```
Form Submission (PDF upload) → In-Memory Vector Store (Insert Documents)
                                    ├── Embedding (OpenAI or Hugging Face)
                                    └── Default Data Loader (binary, auto-detect)
```

**Retrieval Flow:**
```
Chat Trigger → AI Agent
                 ├── Chat Model (GPT-4o mini)
                 ├── Simple Memory
                 └── Vector Store Tool (Retrieve Documents)
                       └── Embedding (same model as data flow)
```

### Pinecone Index Creation
1. Go to pinecone.io → sign up/log in
2. Go to API Keys → generate and copy key
3. Go to Database → Create Index
4. Set name (e.g., `agi03`), choose dimension matching your embedding model, metric = cosine
5. In n8n, add Pinecone Vector Store node → paste API key → select index name

### Gmail Auto-Reply Automation (n8n nodes)
```
Schedule Trigger (every 5 hours) → Gmail (get unread, limit=1) → Get Details → AI Agent (with reply prompt) → Gmail (send reply)
```

The prompt template instructs the LLM to write only the email body with a greeting, response, and signature — no subject line.

## Q&A / Discussion Points
- **LangChain references in n8n JSON exports**: A student noted n8n uses LangChain internally rather than LangGraph. Instructor acknowledged it as a known criticism but didn't have details on adoption plans.
- **Hugging Face token permissions**: Give read and write permissions; the instructor recommended enabling all available permissions for simplicity.
- **Why embedding retrieval sometimes doesn't trigger**: If the query is too generic (e.g., "What is RAG?"), the embedding similarity score may be too low to trigger retrieval. Better embeddings and more specific queries help.
- **Vector store "as tool" vs. "as vector store for chain"**: Changing the mode affects whether the agent autonomously decides to query the store or always queries it.

## Action Items & Assignments
- [ ] Execute the basic RAG flow (download from Day 2 Google Drive folder)
- [ ] Execute the Hugging Face embeddings variant
- [ ] Practice changing: embedding models, chat models, input source (Google Drive instead of form upload)
- [ ] Set up Pinecone account, create index, run the Pinecone RAG flow
- [ ] **Portfolio Project (due June 26)**: Search "AI agents GitHub n8n", pick a relevant project, implement it end-to-end, create a report with AI, push to GitHub, then message instructor for review
- [ ] Explore GitHub repos for n8n agent workflows (Gmail, social media, HR/recruitment, etc.)
- [ ] Next week (Module 12 — final week): LangGraph, CrewAI, security/guardrails topics

## Open Questions / Unclear Spots
- [unclear: "Leak Guard 2"] — A student reported something downloading when accessing Pinecone; instructor said it shouldn't happen. Possibly a browser extension or unrelated download.
- The resume/CV screening workflow was started but not completed due to time constraints; instructor said it requires auto-triggering after embeddings are stored.
- n8n's internal use of LangChain vs. LangGraph was raised but not resolved.

## Flashcards

**Q: What are the two main parts of a RAG pipeline?**
A: (1) Data flow — ingest, chunk, embed, and store documents; (2) Retrieval flow — embed the user query, perform similarity search, and pass relevant context to the LLM.

**Q: Why is RAG preferred over fine-tuning in ~95% of gen AI use cases?**
A: RAG doesn't require retraining the model, making it far cheaper, faster to iterate, and less resource-intensive than fine-tuning.

**Q: What happens if your embedding model outputs 384 dimensions but your Pinecone index is configured for 512?**
A: The pipeline will fail with a dimension mismatch error. The index dimensions must exactly match the embedding model's output dimensions.

**Q: In n8n's RAG flow, what does a "green line" to the vector store indicate?**
A: It confirms the AI agent retrieved its answer from the embeddings/vector store rather than generating it from the LLM's built-in knowledge.

**Q: What is the difference between connecting a vector store "as tool" vs. "as vector store for chain" in n8n?**
A: "As tool" lets the AI agent decide when to query the store; "as vector store for chain" forces retrieval on every interaction.

**Q: Name three vector database options available as n8n integrations.**
A: Pinecone, Qdrant, Supabase (also: ChromaDB, Redis, Weaviate, MongoDB Atlas, Azure AI Search).

**Q: What free embedding alternative was demonstrated instead of OpenAI?**
A: Hugging Face embeddings, specifically the `sentence-transformers/all-MiniLM-L6-v2` model (384 dimensions).

**Q: In the Gmail auto-reply automation, why is the email limit set to 1?**
A: As a safety measure during testing — if the AI produces bad replies, only one email is sent rather than bulk-sending to all unread messages.

**Q: What should you verify before creating a Pinecone index for your RAG pipeline?**
A: The output dimension of your chosen embedding model, so you can set the index dimension to match exactly.

**Q: What was the portfolio project assignment for this week?**
A: Search GitHub for n8n AI agent workflows, pick a relevant project, implement it end-to-end, write a report, push to GitHub, and submit to the instructor by June 26.
