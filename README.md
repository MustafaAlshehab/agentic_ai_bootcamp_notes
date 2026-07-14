# Codecademy Agentic AI Bootcamp — Session Summaries

AI-generated summaries for the **Building Agentic AI Applications for Beginners** bootcamp hosted by [Codecademy](https://www.codecademy.com/). The bootcamp is a 12+ week live program covering the full path from Python fundamentals to agentic AI systems.

> **Disclaimer:** These summaries were generated using large language models (LLMs) and may contain inaccuracies, misinterpretations, or missing context. They are intended as study aids, not as a replacement for attending sessions or reading official materials. If you spot an error, please open an issue or submit a pull request — every contribution is welcome!



## What's Inside

Each file is a detailed summary of a single live session, following a consistent structure:

- **TL;DR** — a one-paragraph overview of the session
- **Topics Covered** — bullet-point list of everything discussed
- **Key Concepts Explained** — deeper breakdowns with definitions, examples, and caveats
- **Code Snippets** — pseudocode or actual code walked through during the session
- **Mistakes / Corrections** — notable errata or live debugging moments
- **Recap of Q&A** — student questions and instructor answers

## Curriculum Overview


| Week   | Sessions | Topics                                                                                                                                                                |
| ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **01** | 3        | Python fundamentals — variables, data types, operators, strings, slicing, lists, tuples, sets, bootcamp logistics                                                     |
| **02** | 3        | Dictionaries, control flow, loops, functions, NumPy, Pandas, Matplotlib/Seaborn, intro to agentic AI concepts                                                         |
| **03** | 3        | Predictive AI, ML foundations (train/test, features/labels), generative vs. agentic AI overview, tokenization, embeddings, context windows, RAG intro, LLM landscape  |
| **04** | 3        | Supervised learning (regression, classification), feature scaling, KNN, decision trees, random forest, ensemble methods, customer churn practical, K-Means clustering |
| **05** | 3        | NLP (tokenization, stemming, lemmatization, POS tagging, stop words), Bag of Words, TF-IDF, Word2Vec, transformers (BERT/GPT), prompt engineering                     |
| **06** | 3        | Generative AI deep dive, LLMs, RAG architecture (chunking, embeddings, vector DBs, FAISS), building RAG apps with LangChain and Streamlit, project reviews            |
| **07** | 3        | APIs vs. open-source models, Ollama setup, local RAG, quantization, RAG vs. fine-tuning, Hugging Face pipelines, hybrid RAG chatbot assignment                        |
| **08** | 3        | RAG evaluation — LLM-as-a-judge, BLEU/ROUGE/METEOR, RAGAS framework, model comparison (TinyLlama, Llama 3.2, Groq), RAG project showcase                              |
| **09** | 2        | Agentic AI concepts & components, ReAct, LangChain tools, React agents, LangGraph state graphs                                                                        |
| **10** | 3        | CrewAI multi-agent orchestration, content marketing pipeline, AWS EC2 deployment, interview prep, intro to n8n                                                        |
| **11** | 2        | n8n workflows — lead capture, social media automation, AI news agent, end-to-end RAG with Pinecone & Hugging Face                                                     |
| **12** | 3        | Multi-agent systems & guardrails (LangGraph + n8n), MCP, end-to-end LangGraph/CrewAI projects, career prep                                                            |




## Session Index



### Week 01 — Python Fundamentals


| Date       | File                                       | Title                                                      | Topics Discussed                                                                                   |
| ---------- | ------------------------------------------ | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| 2026-03-21 | [week01-session01.md](week01-session01.md) | Bootcamp Overview & Python Basics                          | Bootcamp logistics, AI industry timeline, variables, keywords, data types, operators, Google Colab |
| 2026-03-22 | [week01-session02.md](week01-session02.md) | Strings, Slicing, and Data Structures                      | Strings, slicing, lists, tuples, sets, mutability                                                  |
| 2026-03-26 | [week01-session03.md](week01-session03.md) | Doubt-Clearing — Logistics, Career Paths & Client Delivery | Break week plan, career paths, client delivery, Q&A                                                |




### Week 02 — Data Science Foundations


| Date       | File                                       | Title                                              | Topics Discussed                                             |
| ---------- | ------------------------------------------ | -------------------------------------------------- | ------------------------------------------------------------ |
| 2026-03-28 | [week02-session01.md](week02-session01.md) | Dictionaries, Control Flow, Loops & Functions      | Dicts, if/else, for/while loops, functions, arguments        |
| 2026-03-29 | [week02-session02.md](week02-session02.md) | NumPy, Pandas & Visualization Libraries            | NumPy arrays, Pandas DataFrames, Matplotlib, Seaborn         |
| 2026-04-02 | [week02-session03.md](week02-session03.md) | Break Week Plan, Assignments & Intro to Agentic AI | Assignment workflow, agentic AI preview, break-week guidance |




### Week 03 — ML Foundations & GenAI Concepts


| Date       | File                                       | Title                                                        | Topics Discussed                                                      |
| ---------- | ------------------------------------------ | ------------------------------------------------------------ | --------------------------------------------------------------------- |
| 2026-04-18 | [week03-session01.md](week03-session01.md) | Predictive AI, ML Foundations & Generative vs. Agentic AI    | Predictive AI, train/test split, features/labels, GenAI vs agentic AI |
| 2026-04-19 | [week03-session02.md](week03-session02.md) | Tokenization, Embeddings, Context Windows, RAG & LLM Options | Tokenization, embeddings, context windows, RAG intro, LLM landscape   |
| 2026-04-23 | [week03-session03.md](week03-session03.md) | Doubt-Clearing — APIs, Portfolio & Assignment Walkthrough    | APIs, portfolio tips, assignment walkthrough                          |




### Week 04 — Supervised & Unsupervised Learning


| Date       | File                                       | Title                                                        | Topics Discussed                                                  |
| ---------- | ------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------------------- |
| 2026-04-25 | [week04-session01.md](week04-session01.md) | Regression, Feature Scaling & Classification Intro           | Linear/logistic regression, feature scaling, classification intro |
| 2026-04-26 | [week04-session02.md](week04-session02.md) | Classification, Ensemble Learning & Customer Churn Practical | KNN, decision trees, random forest, ensemble methods, churn demo  |
| 2026-04-30 | [week04-session03.md](week04-session03.md) | Doubt-Clearing — K-Means, Elbow Method & Transfer Learning   | K-Means clustering, elbow method, transfer learning               |




### Week 05 — NLP & Transformers


| Date       | File                                       | Title                                                           | Topics Discussed                                            |
| ---------- | ------------------------------------------ | --------------------------------------------------------------- | ----------------------------------------------------------- |
| 2026-05-02 | [week05-session01.md](week05-session01.md) | NLP Basics — Tokenization, Preprocessing & Classical Embeddings | Tokenization, stemming, lemmatization, POS, BoW, TF-IDF     |
| 2026-05-03 | [week05-session02.md](week05-session02.md) | Word2Vec, Transformers (BERT/GPT), LLMs & Client LLM Selection  | Word2Vec, BERT/GPT, transformer architecture, LLM selection |
| 2026-05-07 | [week05-session03.md](week05-session03.md) | Q&A, Prompt Engineering & AI Dashboard Demo                     | Prompt engineering, Q&A, AI dashboard demo                  |




### Week 06 — Generative AI & RAG


| Date       | File                                       | Title                                                                | Topics Discussed                                                  |
| ---------- | ------------------------------------------ | -------------------------------------------------------------------- | ----------------------------------------------------------------- |
| 2026-05-09 | [week06-session01.md](week06-session01.md) | Generative AI Deep Dive — LLMs, RAG, Vector DBs & Chunking           | LLM internals, RAG architecture, chunking, embeddings, vector DBs |
| 2026-05-10 | [week06-session02.md](week06-session02.md) | Building RAG-Based Applications                                      | LangChain RAG pipeline, FAISS, Streamlit RAG apps                 |
| 2026-05-14 | [week06-session03.md](week06-session03.md) | Doubt-Clearing — Project Reviews, Copilot Agents & LinkedIn Strategy | Project reviews, Copilot agents, LinkedIn strategy                |




### Week 07 — Open-Source Models & Advanced RAG


| Date       | File                                       | Title                                                             | Topics Discussed                                               |
| ---------- | ------------------------------------------ | ----------------------------------------------------------------- | -------------------------------------------------------------- |
| 2026-05-16 | [week07-session01.md](week07-session01.md) | APIs vs. Open Source Models — Ollama Setup & Local RAG            | API vs local models, Ollama, local RAG setup                   |
| 2026-05-17 | [week07-session02.md](week07-session02.md) | RAG vs. Fine-Tuning, Quantization & Hybrid RAG Chatbot Assignment | RAG vs fine-tuning, quantization, Hugging Face, hybrid chatbot |
| 2026-05-21 | [week07-session03.md](week07-session03.md) | Doubt-Clearing — Website Chatbot Assignment (RAG + Q&A)           | Website chatbot assignment, RAG Q&A                            |




### Week 08 — RAG Evaluation


| Date       | File                                       | Title                                              | Topics Discussed                                                   |
| ---------- | ------------------------------------------ | -------------------------------------------------- | ------------------------------------------------------------------ |
| 2026-05-30 | [week08-session01.md](week08-session01.md) | Choosing the Best LLM for Your Use Case            | LLM selection criteria, evaluation overview                        |
| 2026-05-31 | [week08-session02.md](week08-session02.md) | Multi-Stage Metrics, RAGAS & Comparing Groq Models | BLEU/ROUGE/METEOR, RAGAS, TinyLlama vs Llama 3.2 vs Groq           |
| 2026-06-04 | [week08-session03.md](week08-session03.md) | RAG Project Showcase & Feedback Session            | Student project demos, AWS EC2 deployment tips, guardrails preview |




### Week 09 — Introduction to Agentic AI


| Date       | File                                       | Title                                             | Topics Discussed                                                                   |
| ---------- | ------------------------------------------ | ------------------------------------------------- | ---------------------------------------------------------------------------------- |
| 2026-06-06 | [week09-session01.md](week09-session01.md) | Agentic AI Concepts, Components & LangChain Tools | GenAI vs agents, ReAct, agent characteristics & components, custom LangChain tools |
| 2026-06-07 | [week09-session02.md](week09-session02.md) | LangChain React Agents, LangGraph & State Graphs  | React agents, tool binding, LangChain vs LangGraph, nodes/edges/state graphs       |




### Week 10 — CrewAI & Deployment


| Date       | File                                       | Title                                                  | Topics Discussed                                                                |
| ---------- | ------------------------------------------ | ------------------------------------------------------ | ------------------------------------------------------------------------------- |
| 2026-06-13 | [week10-session01.md](week10-session01.md) | Introduction to CrewAI: Multi-Agent Orchestration      | Crews, agents, tasks, `kickoff()`, report summarization & email writer projects |
| 2026-06-14 | [week10-session02.md](week10-session02.md) | CrewAI Content Marketing Pipeline & AWS EC2 Deployment | Multi-agent content pipeline, Streamlit deploy on EC2, `nohup`                  |
| 2026-06-18 | [week10-session03.md](week10-session03.md) | Interview Prep, CV Review & Intro to n8n               | Resume XYZ format, RAG interview tips, n8n lead-capture workflow                |




### Week 11 — n8n Workflow Automation


| Date       | File                                       | Title                                                   | Topics Discussed                                                               |
| ---------- | ------------------------------------------ | ------------------------------------------------------- | ------------------------------------------------------------------------------ |
| 2026-06-20 | [week11-session01.md](week11-session01.md) | Lead Capturing, Social Media Automation & AI News Agent | Lead form → Sheets → Gmail, social media automation, AI news agent with memory |
| 2026-06-21 | [week11-session02.md](week11-session02.md) | End-to-End RAG on n8n with Pinecone & Hugging Face      | n8n RAG pipeline, HF embeddings, Pinecone, Gmail auto-reply                    |




### Week 12 — Guardrails, MCP & Career Prep


| Date       | File                                       | Title                                              | Topics Discussed                                                                |
| ---------- | ------------------------------------------ | -------------------------------------------------- | ------------------------------------------------------------------------------- |
| 2026-06-27 | [week12-session01.md](week12-session01.md) | Multi-Agent Systems & Guardrails (LangGraph + n8n) | Single vs multi-agent, input/output guardrails, LangGraph & n8n implementations |
| 2026-06-28 | [week12-session02.md](week12-session02.md) | MCP & End-to-End Projects (LangGraph + CrewAI)     | Model Context Protocol, MCP in n8n, LangGraph & CrewAI assistant projects       |
| 2026-07-02 | [week12-session03.md](week12-session03.md) | Career Prep — Interviews, LinkedIn & Resumes       | Portfolio projects, interview Qs, LinkedIn automation, resume best practices    |




## Contributing

Contributions of any kind are welcome! You can help by:

- **Fixing errors** — correct inaccuracies, typos, or misleading explanations
- **Adding context** — fill in gaps or expand on topics that were covered too briefly
- **Improving formatting** — clean up code snippets, tables, or section structure
- **Adding resources** — link to relevant documentation, papers, tutorials, or tools mentioned in sessions
- **Adding new summaries** — contribute notes from sessions not yet covered

To contribute, fork the repository, make your changes, and open a pull request. If you're unsure about something, feel free to open an issue to discuss it first.

## License

This project is licensed under the [MIT License](LICENSE).