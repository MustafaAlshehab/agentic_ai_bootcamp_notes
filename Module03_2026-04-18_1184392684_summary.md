# Predictive AI, ML Foundations, and Generative vs. Agentic AI (High-Level)

## TL;DR
This session bridged the bootcamp’s completed Python/data-analysis stage with upcoming ML and GenAI topics. The instructor used a telecom churn scenario to introduce predictive AI, exploratory analysis for insights, and the classic train/test ML workflow (features, labels, multiple models, accuracy). After a break, the class covered generative AI (LLMs, SLMs, transformers, BERT vs. GPT) and contrasted it with agentic AI (LLM + tools/actions), including a hiring-automation case study with human-in-the-loop caveats.

## Topics Covered
- Bootcamp recap (Python, Pandas, NumPy, Matplotlib) and roadmap (ML → deep learning → NLP → generative → agentic AI)
- Predictive AI defined via Vodafone-style customer churn
- Churn rate formula and segment-level insight (gender example)
- Classification vs. regression as two predictive-AI branches (deferred to a future weekend class)
- Breakout: features/columns needed for churn prediction
- ML vocabulary: features (X), target (Y), independent vs. dependent variables
- Train/test split (80/20 thumb rule), proportional split math, stratified sampling
- Model training flow: X_train + Y_train → models → X_test only for predictions → compare to Y_test → accuracy
- Hyperparameter tuning (“grooming”) and model selection for deployment
- Break
- Generative AI vs. predictive AI; negative feedback / how to raise concerns with Codecademy
- LLMs vs. SLMs (parameters, latency, IoT use cases, quantization mentioned briefly)
- Transformers high level: “Attention Is All You Need,” encoder/decoder, embeddings, attention, BERT family, GPT
- Generative AI limits (no real-time actions) vs. modern ChatGPT-style tools (search = agentic)
- Agent architecture sketch (intent routing → tools vs. direct LLM)
- Agentic AI = multiple agents in a workflow
- Breakout: end-to-end hiring automation for Codecademy
- Instructor hiring flowchart, HITL, single-agent vs. multi-agent designs
- Preview of tomorrow (LLM basics, prompt engineering) and free ML/DL course for self-study

## Key Concepts Explained

### Predictive AI
- Uses **historical structured data** to analyze patterns and build ML models that predict outcomes (e.g., churn vs. active).
- Why it matters: Business decisions (retention offers) depend on predicting *who* will leave *before* they churn—not where they go.
- Instructor emphasized: predicting destination network (Airtel, Jio, etc.) does not help retention; predicting churn risk does.

### Churn rate
- **Churn rate** = (number of churners ÷ total customers) × 100.
- Segment churn (e.g., females: 2,500 churners among female churners in subset) can differ from overall rate (30% overall vs. ~50.5% for females in the worked example)—useful for targeting.

### Classification vs. regression
- **Classification**: predict a **category** (churned vs. active).
- **Regression**: predict a **continuous number** (e.g., temperature 36 vs. 36.5).
- Covered at overview level; metrics differ (e.g., temperature validation)—details in dedicated ML classes.

### Features, labels, and variable naming
- Input columns = **features** / **X** / **independent variables** (each may influence the outcome).
- Column to predict = **target** / **predictor** / **Y** / **dependent variable** (depends on features).

### Train / test split and why it exists
- Data split into **training** (model learns) and **testing** (unseen evaluation).
- Common thumb rule: **80/20** (not mandatory—“no right or wrong”).
- Example: 10,000 rows (8,000 active, 2,000 churned @ 20%) → 8,000 train / 2,000 test → if split **proportionally**: train 6,400 active + 1,600 churned; test 1,600 active + 400 churned.
- **Stratified sampling** preserves class proportions; other sampling techniques can yield different distributions—instructor noted ML “is not too easy” here.

### Model building and evaluation (high level)
- Feature engineering mentioned but deferred.
- Multiple models (M1…Mn) trained on full training set (X_train + Y_train).
- At prediction time, only **X_test** goes in; predictions compared to held-out **Y_test** → **accuracy** (example: 8/10 correct = 80%).
- Best model may be tuned via **hyperparameter optimization** (“grooming”)—instructor’s experience: ~85% can often reach ~90%.
- Deployed model scores **new** rows without known Y (e.g., churn probability/confidence).

### Generative AI
- AI for **generating** text, images, videos; backbone often **large language models (LLMs)**.
- Contrasts with predictive AI: generation vs. prediction from historical labels.

### SLM vs. LLM (informal bootcamp framing)
- **SLM**: &lt; ~8B parameters, less data, weaker output/accuracy, **lower latency**—suited to smartwatches, IoT, small devices; **quantized** models mentioned for later.
- **LLM**: larger (&gt; ~8B parameters), more data, better output/accuracy, **higher latency**—enterprise / large apps.
- Instructor noted “small language model” as a community split, not a rigid formal taxonomy.

### Transformers (NLP)
- Popularized by Google’s **“Attention Is All You Need”** (2017; instructor corrected year from 2016).
- **Encoder** + **decoder**; user text → **embeddings** → layers (feed-forward, **attention**) → output (e.g., translation “how are you” → Spanish).
- **Attention**: focus on subject/object/context (Roger Federer example).
- Later split: **encoder-only** (e.g., **BERT**—Bidirectional Encoder Representations from Transformers; RoBERTa, ALBERT, DistilBERT) strong at summarization / next-token tasks; **decoder-only** (**GPT**—Generative Pre-trained Transformer) strong at generation.
- Instructor’s practical take: standalone encoder/transformer stacks largely superseded by **decoder/GPT-style** products for many tasks today; still taught for foundations.

### Embeddings
- Human language converted to **numbers** for any NLP architecture (encoder, decoder, etc.)—not the same as “encoder” in the English sense.
- Distinct from architectural “encoder” module.

### Agentic AI vs. generative AI
- **Generative AI alone**: generates answers from training data; **cannot** reliably do real-time actions (weather API, booking, news) without extra capabilities.
- **AI agent**: LLM + **actions** (API calls: hotels, flights, weather, news, etc.).
- **Agentic AI workflow**: multiple agents coordinating (e.g., hiring pipeline).
- Modern **ChatGPT / Grok / Poe** often include **web search** → behavior is **agentic**, not pure generative (demo: older GPT-3.5 on Poe trained through ~Sept 2021 but can answer “today’s date” via search APIs).

### Agent routing (Alexa-style example)
- User input → **intent classification** (LLM) → route to tool (weather, music, FX) or **direct LLM** for general knowledge questions.

### Hiring automation case study
- Stages: JD creation → **human review** → post (e.g., LinkedIn APIs) → wait for applicants → optional **Plan B** (headhunters) → screen → schedule interviews → offer/negotiation.
- **Auditing / bias-check agents** raised (Amazon ATS filtering females)—responsible AI.
- **HITL (human-in-the-loop)** mandatory; 100% automation risks flaws.
- **Single agent** with many tools vs. **multiple specialized agents**—design choice only.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| Python, Pandas, NumPy, Matplotlib | Recap as stage-one bootcamp topics |
| ChatGPT, Grok | Examples of GenAI / agentic consumer tools |
| poe.com | Used to demo older **GPT-3.5** and training cutoff (~Sept 2021) vs. live answers |
| “Attention Is All You Need” (2017) | Foundational transformers paper (Google) |
| BERT, RoBERTa, ALBERT, DistilBERT | Encoder-family models |
| GPT | Decoder / generative pre-trained models |
| Transformers (architecture) | Encoder-decoder; later encoder-only vs. decoder-only |
| Embeddings | Text → numeric vectors |
| Stratified sampling | Keeps class ratios in train/test |
| Hyperparameter optimization | “Grooming” to improve accuracy |
| LinkedIn, Glassdoor | Example job-posting platforms (APIs per platform) |
| Skyscanner, Klook, weather/news APIs | Example agent tools |
| Amazon Alexa | Intent-routing analogy for agents |
| Instructor’s 6-hour EDA video | Prerequisite recommendation |
| Free ML (~50h) + DL (~10–15h) courses | Promised later for self-study outside bootcamp hours |

## Code & Examples

No live coding in this session. Reconstructed procedures and formulas:

**Churn rate (overall or segment):**
```
churn_rate = (number_of_churners / total_in_group) * 100
```

**Proportional 80/20 split** (20% churn rate, 10,000 rows):
```
train: 8000 rows → 6400 active, 1600 churned
test:  2000 rows → 1600 active, 400 churned
```

**ML evaluation sketch (10 test rows):**
- Compare each model prediction to actual Y_test labels; count matches → accuracy (e.g., 8/10 = 80%).

**Telecom feature categories** (from class + breakout):
- Demographics (age, gender, location, salary range optional)
- Price plan (prepaid/postpaid, data caps, recharge history)
- Usage (calls, data/Wi-Fi)
- Tenure, number of services, customer type
- Call-center voice recordings (unhappy customers call support)

**High-level agent flow (weather question):**
```
user_input → intent_classifier (LLM) → if weather → weather_API_tool
                                      → else if music → youtube_tool
                                      → else → LLM_direct_answer
```

**Hiring flow (instructor whiteboard, approximate):**
```
create_JD → human_review? ─no→ revise_JD
                │
               yes
                ↓
         post_to_platforms (e.g., LinkedIn API)
                ↓
         wait_for_applications ──(fallback)──→ headhunters / Plan B
                ↓
         shortlist → schedule_interviews → offers / negotiation
```
(Each step may be one agent or one agent with multiple tools.)

## Q&A / Discussion Points

- **Why learn EDA/ML if the bootcamp focuses on agentic AI?** Building blocks; instructor recommends 6-hour EDA prerequisite (optional but encouraged).
- **Are we predicting on the same labeled data?** Historical labels train the model; **new** customers without labels get predictions at deployment.
- **Why split data?** (Victoria) Without a holdout set you cannot measure generalization—confirmed.
- **Testing data visibility** Model sees training data only; test X is “unseen”; Y_test used only for evaluation.
- **Classification vs. regression metrics** Different metrics for temperature/regression—deferred to ML class.
- **Embedding vs. encoder** Embeddings convert language to numbers for all NLP inputs; architectural encoder ≠ everyday “encode/decode” English.
- **Why can ChatGPT give today’s weather?** Search/tools → agentic, not pure generative.
- **Encoder input to GPT if link broken?** Prompt goes to decoder/GPT directly.
- **Team one breakout confusion** “Department” interpreted as internal telecom departments vs. **customer** data—corrected.
- **Female churn rate calculation** Class pushed to use formula (2,500 / total females) not guess “~60%”.
- **Negative session feedback** Instructor asked students to share concerns via Codecademy (e.g., Nazia) for actionable fixes.

## Action Items & Assignments

- [ ] Watch instructor’s **6-hour EDA** prerequisite video if not done (recommended).
- [ ] Review **doubt-clearing** answers when shared; validate your break-week exercise work.
- [ ] Attend **tomorrow**: LLM basics, prompt engineering, and related topics.
- [ ] **Next week**: ML class (classification/regression in more depth).
- [ ] Plan to complete **free ML (~50 hours) and DL (~10–15 hours)** courses on your own when provided (not fully covered in bootcamp schedule).
- [ ] Optional: pen-and-paper hiring flow / architecture (breakout already done in session).

## Open Questions / Unclear Spots

- Exact **sampling technique** names beyond stratified sampling were not detailed.
- **Feature engineering** steps referenced but not demonstrated.
- **Quantization** for SLMs flagged for a later class only.
- Instructor’s **SLM &lt; 8B parameters** threshold is a teaching shorthand, not a universal industry definition.
- Statement that **“transformers are of no use”** today was rhetorical/contrast to GPT-era products—instructor still plans deep transformer coverage later.
- **GPT-33.5** on Poe likely means **GPT-3.5** [unclear: transcript ASR].
- **Career Engine AI** on learning dashboard—mentioned in passing without explanation [unclear: product name].
- **“de-identifier”** on dashboard agenda—transcript garble [unclear: topic name].
